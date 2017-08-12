---
title: Matrices y nombres
author: manuel
layout: post
permalink: /2017/08/12/matricces-y-nombres
categories:
  - Uncategorized
---


Esta semana, la [Dirección Nacional de Datos e Información Pública](https://datosgobar.github.io/) publicó ["Tu nombre en los últimos 100 años"](http://nombres.historias.datos.gob.ar), un sitio muy divertido que permite consultar frecuencias de uso de nombres propios. Parecido al [Popular Baby Names](https://www.ssa.gov/oact/babynames/) de la Social Security Administration de Estados Unidos. Junto con el sitio, el equipo de datos públicos subió [el dataset al portal de datos públicos](http://datos.gob.ar/dataset/nombres-personas-fisicas).

El diario La Nación [publicó un enlace al sitio de nombres en su home page](http://www.lanacion.com.ar/2051547-conoce-en-que-ano-tu-nombre-fue-el-mas-utilizado-en-la-argentina)…y se vino abajo por el tráfico.

Para poner a andar un ratito la croqueta, me puse a pensar cómo hacer un método eficiente de consulta de esta información. Dado uno o varios nombres, quiero obtener la serie temporal de sus frecuencias. No es nada del otro mundo, y es apenas un prototipo. 


## Preparando el dataset

```bash
cat historico-nombres.csv | uconv  -t ASCII -x nfd -c | tr '[:upper:]' '[:lower:]' | tr -s ' ' | sed -E 's/^ *//' | csvfix sort -smq -rh -f 1:S,3:N > sorted-ascii-historico-nombres.csv > ascii-historico-nombres.csv
```

Ese _pipeline_ de comandos procesa el archivo original aplicando las siguientes transformaciones:

  - `uconv -t ASCII -x nfd -c`: Aplicar la forma de normalización _Canonical Decomposition_ de Unicode (NFD). En criollo, sacarle acentos a los caracteres
  - `tr '[:upper:]' '[:lower:]'`: pasar todo a minísculas
  - `tr -s ' '`: convertir espacios repetidos a uno sólo.
  - `sed -E 's/^ *//'`: sacar espacios del principio de cada línea.
  - `csvfix sort -smq -rh -f 1:S,3:N`: ordenar la tabla según nombre y luego año.
  
Nos queda algo así:

| nombre                         | cantidad | anio | 
|--------------------------------|----------|------| 
| aage tomasen                   | 2        | 1931 | 
| aago peter                     | 1        | 1987 | 
| aakash                         | 2        | 1985 | 
| aalam yamir                    | 2        | 2013 | 
| aale rene                      | 1        | 1987 | 
| aalejandro daniel              | 1        | 2002 | 
| aaleyah nayara                 | 2        | 2013 | 



## Una estructura eficiente

Lo más simple que se me ocurrió es [pivotear](https://en.wikipedia.org/wiki/Pivot_table) ese dataset, para convertirlo en una matriz donde cada fila es un nombre y cada columna es un año. Tenemos 3044402 nombres únicos y un período de 94 años. Es decir, una matriz de 3044402 x 94. 

Para poder obtener la fila correspondiente al nombre que nos interesa, también construimos un diccionario `NAMES` cuyas claves son los nombres y sus valores el índice de la fila de la matriz que contiene la serie temporal de frecuencias.

El siguiente script construye esas estructuras de datos.


```python
# coding: utf-8
import csv, sys, pickle
import numpy as np

YEAR_MIN, YEAR_MAX = 1922, 2015
YEARS_Q = YEAR_MAX - YEAR_MIN
NAMES_Q = 3044402 # count-distinct on the name column
NAMES = {}

FREQS = np.zeros((NAMES_Q, YEARS_Q+1), int)

reader = csv.reader(sys.stdin)
next(reader) # skip header

# Pivotear el dataset de nombres:
# a partir de una tabla de (nombre, frecuencia, año), construir una matriz
# de frecuencias |nobmres| x |años|
cur_name, cur_row, i = None, None, -1
for row in reader:
    if row[0] != cur_name:
        NAMES[row[0]] = i
        i += 1

    if i % 2000 == 0:
        print("%d names processed" % i)

    FREQS[i, int(row[2]) - YEAR_MIN] = int(row[1])
    cur_name = row[0]

# save FREQS
np.save('freqs', FREQS)
# save NAMES
with open('names.pickle', 'wb') as f:
    pickle.dump(NAMES, f)

```

# Consultando las frecuencias

Cómo consultamos esto? Fácil. Obtenemos el índice del nombre que nos interesa, y con él, la fila correspondiente en la matriz:


```python
import numpy as np
import pickle

FREQS = np.load('freqs.npy')
with open('names.pickle', 'rb') as f:
    NAMES = pickle.load(f)
```


```python
FREQS[NAMES['manuel']]
```




    array([  56,    2,  119,  122,    2,    1,    4,  231,    6,    2,  326,
            268,  330,    1,  330,  332,    6,  356,    3,    3,    4,    2,
              3,  494,  464,  510,    8,    4,    1,  365,  374,    3,  317,
              3,    3,  308,    3,  253,    8,    5,    1,    1,    1,  180,
            167,    3,  161,  151,  193,  156,  144,  173,    5,  223,  269,
              4,  242,    2,    3,  238,  268,    8,    1,  436,  456,  415,
            487,  458,  566,  627,  555,  801, 1013, 1135, 1012,  783,  786,
            760,  729,  678,  736,  815,    2,    0,  718,  726,  705,  650,
            581,  600,  814,  789,  849,  711])



Visualizamos el resultado para verificar que al menos se parezca a lo que reporta. Para esto, también vamos a calcular el _pormilaje_ (?) del nombre de interés para cada año. Con los datos en esta matriz, es fácil: la cantidad de nombres en cada año es la suma de cada columna.


```python
manuel_1000ct = (FREQS[NAMES['manuel']] / np.sum(FREQS, axis=0)) * 1000
```


```python
from altair import Chart, Bin, X, Axis
import pandas as pd

data = pd.DataFrame({'year': list(range(1922,2016)), 'freq': manuel_1000ct})
chart = Chart(data).mark_line().encode(
    x='year:N',
    y='freq:Q',
)
chart
```


<div class="vega-embed" id="bab3a57f-3dca-4e02-be04-4166ccec52af"></div>

<style>
.vega-embed svg, .vega-embed canvas {
  border: 1px dotted gray;
}

.vega-embed .vega-actions a {
  margin-right: 6px;
}
</style>






![png](/wp-content/uploads/2017/08/chart.png)


Es _parecido_, pero no igual 😔. Quizás el sitio oficial esté calculando los _pormilajes_ con otros valores, o cometí algún error que no estoy viendo.

# Mirá, mamá: sin base de datos.

El tamaño de la matriz `FREQS` es relativamente chico, apenas 2.13 gigabytes en memoria.


```python
FREQS.nbytes / 1024**3
```




    2.132160872220993



El diccionario de nombres (`NAMES`) tampoco ocupa mucho; 160 megabytes.


```python
sys.getsizeof(NAMES) / 1024**2
```




    160.0000991821289



Este pequeño ejercicio se puede exponer a través de un _endpoint_ HTTP muy simple que mantenga esta matriz `numpy` en memoria y envíe los datos serializados en la respuesta.

Con eso, estimo, se pueden mejorar bastante la estabilidad y robustez del servicio

[El código está disponible aquí: [https://gist.github.com/jazzido/1050fd9169adb7fd9ff1d1002649fd16](https://gist.github.com/jazzido/1050fd9169adb7fd9ff1d1002649fd16)]

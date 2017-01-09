---
title: Procesando los microdatos de la Encuesta Permanente de Hogares
author: manuel
layout: post
permalink: /2017/01/09/procesando-microdatos-eph
categories:
  - Uncategorized
---

<style type="text/css">
table.dataframe {
    margin-left: auto;
    margin-right: auto;
    border: 1px solid black;
    border-collapse: collapse;
}

table.dataframe tr,
table.dataframe th,
table.dataframe td
{
border: 1px solid black;
border-collapse: collapse;
margin: 1em 2em;
}
</style>

Cada vez que el [INDEC](http://www.indec.gob.ar) publica datos de la *Encuesta Permanente de Hogares* (EPH), hay repercusiones mediáticas de algunas de las variables que mide esa encuesta. El ingreso suele ser de particular interés para la cobertura periodística, por ejemplo en [esta nota de La Nación](http://www.lanacion.com.ar/1973472-la-mitad-de-los-argentinos-tiene-ingresos-inferiores-a-8000-por-mes).

Esos artículos, en general, usan como fuente a los "cuadros" que publica el INDEC. Esos cuadros contienen estadísticas calculadas a partir de los registros individuales de la EPH. Es decir, las repuestas de cada individuo que fue encuestado. A esas tablas, en la jerga, se las llama *microdatos*.

Inspirado por [el post](https://medium.com/@fernandezpablo/an%C3%A1lisis-de-deciles-de-ingresos-d6fddc885aff#.uec4863wk) de [Pablo Fernández](https://twitter.com/fernandezpablo) en el que analizó los cuadros de población según escala de ingreso individual para el tercer trimestre de 2016, voy a intentar reproducirlos a partir de los *microdatos* de la EPH. Vamos a usar el lenguaje de programación Python, y la librería de análisis y manipulación de datos [pandas](http://pandas.pydata.org/).


```python
import pandas as pd
```

A diferencia de otros organismos de estadística pública, como [las APIs del Census Bureau de Estados Unidos](http://www.census.gov/data/developers/data-sets.html), el INDEC no ofrece demasiada sistematización para obtener las bases de datos. El sistema de distribución de esa información es un rudimentario *download* de un archivo ZIP, que contiene las tablas en formato TXT o Excel. Vamos a trabajar con [el último *dataset* disponible a la fecha, que corresponde al segundo trimestre de 2016](http://www.indec.gov.ar/ftp/cuadros/menusuperior/eph/EPH_usu_2doTrim_2016_xls.zip).

Una vez abierto el archivo ZIP, leemos la tabla de respuestas de individuos.


```python
eph = pd.read_excel('usu_individual_T216.xls')
```

Queremos reproducir un [*cuadro de población total según escala de ingreso individual*](http://www.indec.gov.ar/ftp/cuadros/sociedad/pob_ingreso_individual_2t16.xls). Consultamos el documento de [*Diseño de Registro y Estructura para las bases preliminares Hogar y Personas*](http://www.indec.gov.ar/ftp/cuadros/menusuperior/eph/EPH_registro_2_trim_2016.pdf) y vemos que la variable que contiene el monto de ingreso total individual se llama `P47T`. Filtramos la tabla y obtenemos los registros que declaran algún ingreso. Esto es, aquellos en los que `P47T > 0`.


```python
with_income = eph[eph['P47T'] > 0]
```

Dado que las encuestas como la EPH contienen una muestra de la población, las variables suelen estar ponderadas (*weighted*) por un factor de ajuste que debemos usar para el cálculo de estadísticas. Para tratar el ingreso total individual, la EPH provee la variable `PONDII`, documentada en el [Anexo I del documento de diseño](http://www.indec.gov.ar/ftp/cuadros/menusuperior/eph/EPH_registro_2_trim_2016.pdf).

El *decil* o *grupo decílico* para el ingreso total individual, ya está provisto por la EPH en la variable `DECINDR`, pero no contiene los *límites* de cada uno. Los calculamos con la ayuda de la librería [`weightedcalcs`](https://github.com/jsvine/weightedcalcs).


```python
import weightedcalcs
upper = []
lower = []

wc = weightedcalcs.Calculator('PONDII')
lowerb = with_income['P47T'].min()
for q in range(1,11):
    upperb = wc.quantile(with_income, 'P47T', q/10.0)
    lower.append(lowerb)
    upper.append(upperb)
    lowerb = upperb
deciles = pd.DataFrame({'lbound': lower, 'ubound': upper}).reset_index().rename(columns={'index': 'decile'})
deciles['decile'] = deciles['decile'] + 1
```

Verificamos que las cotas de los deciles son iguales a los del cuadro que estamos replicando:

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>decile</th>
      <th>lbound</th>
      <th>ubound</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>22.0</td>
      <td>2470.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2470.0</td>
      <td>4000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4000.0</td>
      <td>4800.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4800.0</td>
      <td>6000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>6000.0</td>
      <td>7200.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>7200.0</td>
      <td>9000.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>9000.0</td>
      <td>10500.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>10500.0</td>
      <td>14000.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>14000.0</td>
      <td>20000.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>20000.0</td>
      <td>715000.0</td>
    </tr>
  </tbody>
</table>



Usamos el *grupo decílico* provisto en la tabla para agregarle los intervalos de cada uno que acabamos de calcular.


```python
with_income = with_income.merge(deciles, left_on='DECINDR', right_on='decile')
```

Sumando el ponderador (`PONDII`) para cada grupo decílico, obtenemos la población de cada uno. Creamos un nuevo `DataFrame` en el que almacenaremos las variables del cuadro que estamos replicando.


```python
cuadro = pd.DataFrame(with_income.groupby('DECINDR')['PONDII'].sum()).rename(columns={'PONDII': 'decile_population'})
```

Agregamos una variable con el ingreso *total* de cada decil.


```python
with_income.loc[:, 'weighted_income'] = with_income['P47T'] * with_income['PONDII']
cuadro['decile_total_income'] = pd.DataFrame(with_income.groupby('DECINDR')['weighted_income'].sum() / 1000)
```

Agregamos otra con el porcentaje del ingreso de cada decil sobre el ingreso total


```python
cuadro['decile_percentage_income'] = (cuadro['decile_total_income'] / cuadro['decile_total_income'].sum()) * 100
```

Calculamos el ingreso medio por decil


```python
cuadro['decile_mean_income'] = cuadro['decile_total_income'] / cuadro['decile_population']
```

Finalmente, agregamos las cotas de cada decil


```python
cuadro = cuadro.reset_index().merge(deciles, left_on='DECINDR', right_on='decile').rename(columns={'lbound': 'decile_lower_bound', 'ubound': 'decile_upper_bound'})
del cuadro['decile']
```

Como decía la revista Anteojito, acá el *modelo terminado*:


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DECINDR</th>
      <th>decile_population</th>
      <th>decile_total_income</th>
      <th>decile_percentage_income</th>
      <th>decile_mean_income</th>
      <th>decile_lower_bound</th>
      <th>decile_upper_bound</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1618265</td>
      <td>2.121558e+06</td>
      <td>1.350246</td>
      <td>1.311008</td>
      <td>22.0</td>
      <td>2470.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1616771</td>
      <td>5.337631e+06</td>
      <td>3.397086</td>
      <td>3.301414</td>
      <td>2470.0</td>
      <td>4000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1617945</td>
      <td>7.243509e+06</td>
      <td>4.610064</td>
      <td>4.476981</td>
      <td>4000.0</td>
      <td>4800.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1615707</td>
      <td>8.257147e+06</td>
      <td>5.255185</td>
      <td>5.110547</td>
      <td>4800.0</td>
      <td>6000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1617192</td>
      <td>1.046035e+07</td>
      <td>6.657395</td>
      <td>6.468220</td>
      <td>6000.0</td>
      <td>7200.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>1617391</td>
      <td>1.318178e+07</td>
      <td>8.389421</td>
      <td>8.150026</td>
      <td>7200.0</td>
      <td>9000.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>1617847</td>
      <td>1.577172e+07</td>
      <td>10.037767</td>
      <td>9.748588</td>
      <td>9000.0</td>
      <td>10500.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>1616376</td>
      <td>1.957398e+07</td>
      <td>12.457680</td>
      <td>12.109795</td>
      <td>10500.0</td>
      <td>14000.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>1617180</td>
      <td>2.605983e+07</td>
      <td>16.585536</td>
      <td>16.114364</td>
      <td>14000.0</td>
      <td>20000.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>1617037</td>
      <td>4.911631e+07</td>
      <td>31.259620</td>
      <td>30.374264</td>
      <td>20000.0</td>
      <td>715000.0</td>
    </tr>
  </tbody>
</table>


Por supuesto, este ejercicio es apenas un ejemplo de lo que se puede hacer con los *microdatos* de la Encuesta Permanente de Hogares. El procesamiento de otras variables queda como actividad para el lector entusiasta.

El *notebook* completo está disponible acá: [https://gist.github.com/jazzido/d067c834d0990fe3cf932cf6e7ec1881](https://gist.github.com/jazzido/d067c834d0990fe3cf932cf6e7ec1881)

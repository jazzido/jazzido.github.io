---
title: IPython, matplotlib y transporte público en Bahía Blanca
author: manuel
layout: post
permalink: /2014/10/26/ipython-matplotlib-y-transporte-publico-en-bahia-blanca/
dsq_thread_id:
  - 3158666767
categories:
  - Uncategorized
---
Hace un tiempo, la ciudad de Bahía Blanca introdujo tarjetas de proximidad para el pago de la tarifa del servicio de transporte público. Junto con el sistema AVL con el que cuentan las unidades, los registros de uso de las tarjetas de proximidad son una fuente de información valiosa.

Como excusa para aprender un poco más sobre [IPython Notebook][1], [matplotlib][2] y [Basemap][3], estuve jugando con aproximadamente 4.3 millones de registros del sistema de transporte público bahiense, puestos a disposición por la [Agencia de Innovación y Gobierno Abierto][4] de la ciudad.

El *notebook* completo se puede ver acá: [ Datos de Automated Fare Collection del sistema de transporte público de Bahía Blanca][5]

## La información

Los campos más importantes de los registros sobre los que trabajamos son:

*   Fecha y Hora — *momento en que se registró la transacción*
*   ID tarjeta — *identificador numérico, único y no vinculable con los datos personales del usuario*
*   Línea Ómnibus — *Línea (servicio) de ómnibus*
*   Locación — *Lectura del GPS de la unidad al momento de realizarse la transacción*
*   Tipo Pasaje — Normal, frecuente, escolar, etc.

Una vez procesados y guardados en una tabla de una base de datos PostgreSQL/PostGIS, podemos empezar a hacer algunos análisis simples.

## Promedio de viajes por hora

Podemos ver, por ejemplo, la cantidad promedio de viajes efectuados en cada hora del día para los días hábiles de la semana. El pico de actividad en el mediodía, quizás se deba al horario comercial &#8220;cortado&#8221; que se acostumbra en Bahía Blanca y otras ciudades del interior.

![Cantidad promedio de viajes por ahora, durante los días hábiles][6]

## &#8220;Perfil&#8221; de usuario.

Es razonable considerar análisis que requieran tipificar usuarios en base a sus patrones de uso. Una posible forma de construir estos perfiles es la siguiente:

![Perfil de uso semanal para un usuario][7]

![Perfil de uso semanal para un usuario][8]

Los gráficos muestran la cantidad de viajes por hora y día de la semana para un período determinado. En el primero vemos uso consistente alrededor de las 7 de la mañana y de las 6 de la tarde.

## Viajes encadenados

Nos interesa ver las combinaciones más frecuentes de dos líneas de ómnibus. Esta estadística puede indicarnos qué áreas de la ciudad no están bien conectadas por una única línea.

Decimos que los viajes *v<sub>1</sub>* y *v<sub>2</sub>* del pasajero *p* están encadenados si:

*   La diferencia de tiempo entre *v<sub>2</sub>* y *v<sub>1</sub>* es menor a 45 minutos
*   *v<sub>1</sub>* y *v<sub>2</sub>* fueron realizados en diferentes líneas (o sea, no consideramos viajes de retorno hacia el punto de partida)

Graficamos la matriz de combinaciones de líneas de omnibus:

![Matriz de cantidad de viajes entre líneas][9]

También es interesante ver *dónde* comienzan sus viajes los usuarios que realizan la combinación 514-517, una de las más frecuentes:

![viajes 514-517][10]

 *[AVL]: Automatic Vehicle Location

 [1]: http://ipython.org/notebook.html
 [2]: http://matplotlib.org/
 [3]: http://matplotlib.org/basemap/
 [4]: http://gabierto.bahiablanca.gov.ar/
 [5]: http://nbviewer.ipython.org/gist/jazzido/dc5cc9b5126943ae82ea
 [6]: http://blog.jazzido.com/wp-content/uploads/2014/10/avg_trips.png
 [7]: http://blog.jazzido.com/wp-content/uploads/2014/10/perfil_usuario_1.png
 [8]: http://blog.jazzido.com/wp-content/uploads/2014/10/perfil_usuario_2.png
 [9]: http://blog.jazzido.com/wp-content/uploads/2014/10/matriz_lineas.png
 [10]: http://blog.jazzido.com/wp-content/uploads/2014/10/514-517.png
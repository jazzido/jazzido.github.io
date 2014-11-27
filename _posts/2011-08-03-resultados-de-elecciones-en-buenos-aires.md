---
title: Resultados de elecciones en Buenos Aires
author: manuel
layout: post
permalink: /2011/08/03/resultados-de-elecciones-en-buenos-aires/
dsq_thread_id:
  - 376051865
categories:
  - Uncategorized
tags:
  - datos abiertos
  - opendata
---
<small>(Publicado originalmente en <a href="http://groups.google.com/group/garagelab-gov2/browse_thread/thread/68cd54b3bf0db191">la lista de correo</a> de <a href="http://garagelab.tumblr.com">GarageLab</a>)</small>

En lo que ya podría llamarse &#8220;el mapa de la semana&#8221;, hice una visualización con datos de la primera vuelta de la elección para jefe de gobierno en Capital Federal:

<iframe style="margin: 0 auto;" width="100%" height="350px" scrolling="no"  src="https://www.google.com/fusiontables/embedviz?viz=MAP&#038;q=select+*+from+1235135+&#038;h=false&#038;lat=-34.616867500000005&#038;lng=-58.4415375&#038;z=10&#038;t=1&#038;l=col6%3E%3E1"></iframe>
<small>Resultados primera vuelta elección jefe de gobierno. Cuanto más oscuro el color, mayor la diferencia que sacó Macri frente a Filmus.</small>

### Un poco de background técnico

Los porcentajes de la elección salieron de [la página que puso el GCBA][1]. No publican [CSVs como en Santa Fe][2], pero los grafiquitos los generan con un Javascript convenientemente copypasteable (ahorrándonos el scraping), que tiene esta forma: `candidatos_comuna_10 = {
&nbsp;&nbsp;&nbsp;'label': ['Porcentaje de Votos'],
&nbsp;&nbsp;&nbsp;'color': ['#9bc3d4','#547980','#45ada8','#9de0ad','#e5fcc2','#afb7e9'],
&nbsp;&nbsp;&nbsp;'values': [{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'label': 'PRO',
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'values': [40.23]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'label': 'FPV',
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;'values': [27.13]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;},
    ....
&nbsp;&nbsp;&nbsp;]
};`

Esos objectos javascript los transformé en un CSV con Python y después un poco de limpieza con [Google Refine][3]. De ahí, [derecho a un Fusion Table][4].

El mapa de las comunas porteñas está construído a partir de dos fuentes de datos: [el shapefile de Barrios que publicó el USIG del GCBA][5] y la [relación Barrios-Comunas][6] que creó [Juan Codagnone][7] en [Freebase][8] . Este proceso se hizo a mano editando el mapa con [Quantum GIS][9]. Exporté eso a KML y también lo subí a Fusion Tables.

Finalmente, se combinan (merge) las dos Fusion Tables, se configura el estilo de visualización para el mapa y listo: [tabla final][10].

Quizás lo más aprovechable de todo esto sea [el mapa de Comunas (en KML)][11], que varios andaban necesitando .

 [1]: http://resultados-elecciones.buenosaires.gob.ar/master/1/
 [2]: http://blog.jazzido.com/2011/07/26/datos-abiertos-en-elecciones-de-santa-fe/
 [3]: http://code.google.com/p/google-refine/
 [4]: https://www.google.com/fusiontables/DataSource?dsrcid=1234289
 [5]: http://mapa1.buenosaires.gov.ar/sig/info/AplicacionesWebEspacialesConSoftLibre.html
 [6]: http://www.freebase.com/queryeditor?q=&#x7b;&#x25;22type%22%3A%22%2Flocation%2Far_commune%22%2C%22%2Flocation%2Flocation%2Fcontains%22%3A%5B%5D%2C%22name%22%3A%5B%5D}
 [7]: http://twitter.com/#!/juam
 [8]: http://freebase.com
 [9]: http://www.qgis.org
 [10]: https://www.google.com/fusiontables/DataSource?dsrcid=1234544
 [11]: https://www.google.com/fusiontables/exporttable?query=select+*+from+1234544+&#038;o=kmllink&#038;g=col6%3E%3E1

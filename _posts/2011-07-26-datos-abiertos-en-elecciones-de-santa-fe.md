---
title: Datos abiertos en elecciones de Santa Fé
author: manuel
layout: post
permalink: /2011/07/26/datos-abiertos-en-elecciones-de-santa-fe/
dsq_thread_id:
  - 368984849
categories:
  - Uncategorized
tags:
  - datos abiertos
  - opendata
---
La [provincia de Santa Fé][1] hizo un trabajo buenísimo con la [publicación en la web de los resultados de las elecciones 24 de julio de 2011][2]. También sorprendieron con la publicación de los *datos crudos* de los escrutinios. Con esto, no sólo permiten el acceso libre e irrestricto a la información sino que incentivan el análisis independiente y la combinación (*mashup*) con otras fuentes de datos.

### Un ejemplo

Teniendo a disposición los datos en formato CSV, nos ahorramos el *scraping* (proceso largo, tedioso y cuya robustez depende de factores que están fuera del alcance del programador).
Combinando los resultados de votación por departamento con un mapa departamental de Santa Fe (extraído de [GeoINTA][3]), sale este mapa:

<iframe style="margin: 0 auto;" width="100%" height="350px" scrolling="no"  src="https://www.google.com/fusiontables/embedviz?viz=MAP&#038;q=select+col1%3E%3E0%2C+col0%3E%3E0%2C+col2%3E%3E0%2C+col3%3E%3E0%2C+col4%3E%3E0%2C+col5%3E%3E0%2C+col6%3E%3E0%2C+col7%3E%3E0%2C+col8%3E%3E0%2C+col9%3E%3E0%2C+col10%3E%3E0%2C+col11%3E%3E0%2C+col12%3E%3E0%2C+col13%3E%3E0%2C+col14%3E%3E0%2C+col15%3E%3E0%2C+col16%3E%3E0%2C+col17%3E%3E0%2C+col18%3E%3E0%2C+col19%3E%3E0%2C+col20%3E%3E0%2C+col21%3E%3E0%2C+col22%3E%3E0%2C+col23%3E%3E0%2C+col24%3E%3E0%2C+col25%3E%3E0%2C+col26%3E%3E0%2C+col27%3E%3E0%2C+col28%3E%3E0%2C+col29%3E%3E0%2C+col30%3E%3E0%2C+col31%3E%3E0%2C+col32%3E%3E0%2C+col33%3E%3E0%2C+col34%3E%3E0%2C+col35%3E%3E0%2C+col36%3E%3E0%2C+col37%3E%3E0%2C+col38%3E%3E0%2C+col39%3E%3E0%2C+col40%3E%3E0%2C+col41%3E%3E0%2C+col42%3E%3E0%2C+col43%3E%3E0%2C+col44%3E%3E0%2C+col45%3E%3E0%2C+col46%3E%3E0%2C+col47%3E%3E0%2C+col48%3E%3E0%2C+col49%3E%3E0%2C+col50%3E%3E0%2C+col51%3E%3E0%2C+col52%3E%3E0%2C+col53%3E%3E0%2C+col54%3E%3E0%2C+col55%3E%3E0%2C+col56%3E%3E0%2C+col57%3E%3E0%2C+col58%3E%3E0%2C+col3%3E%3E1%2C+col4%3E%3E1%2C+col6%3E%3E1%2C+col7%3E%3E1%2C+col8%3E%3E1%2C+col9%3E%3E1%2C+col10%3E%3E1+from+1196140+&#038;h=false&#038;lat=-31.188576000000005&#038;lng=-60.880688&#038;z=6&#038;t=1&#038;l=col10%3E%3E1"></iframe>

<small>(Elección de Gobernador. Amarillo: victoria de Del Sel, Rojo: victoria de Bonfatti. Hacé click sobre el mapa para ver los porcentajes. <a href="https://www.google.com/fusiontables/exporttable?query=select+col1%3E%3E0%2C+col0%3E%3E0%2C+col2%3E%3E0%2C+col3%3E%3E0%2C+col4%3E%3E0%2C+col5%3E%3E0%2C+col6%3E%3E0%2C+col7%3E%3E0%2C+col8%3E%3E0%2C+col9%3E%3E0%2C+col10%3E%3E0%2C+col11%3E%3E0%2C+col12%3E%3E0%2C+col13%3E%3E0%2C+col14%3E%3E0%2C+col15%3E%3E0%2C+col16%3E%3E0%2C+col17%3E%3E0%2C+col18%3E%3E0%2C+col19%3E%3E0%2C+col20%3E%3E0%2C+col21%3E%3E0%2C+col22%3E%3E0%2C+col23%3E%3E0%2C+col24%3E%3E0%2C+col25%3E%3E0%2C+col26%3E%3E0%2C+col27%3E%3E0%2C+col28%3E%3E0%2C+col29%3E%3E0%2C+col30%3E%3E0%2C+col31%3E%3E0%2C+col32%3E%3E0%2C+col33%3E%3E0%2C+col34%3E%3E0%2C+col35%3E%3E0%2C+col36%3E%3E0%2C+col37%3E%3E0%2C+col38%3E%3E0%2C+col39%3E%3E0%2C+col40%3E%3E0%2C+col41%3E%3E0%2C+col42%3E%3E0%2C+col43%3E%3E0%2C+col44%3E%3E0%2C+col45%3E%3E0%2C+col46%3E%3E0%2C+col47%3E%3E0%2C+col48%3E%3E0%2C+col49%3E%3E0%2C+col50%3E%3E0%2C+col51%3E%3E0%2C+col52%3E%3E0%2C+col53%3E%3E0%2C+col54%3E%3E0%2C+col55%3E%3E0%2C+col56%3E%3E0%2C+col57%3E%3E0%2C+col58%3E%3E0%2C+col3%3E%3E1%2C+col4%3E%3E1%2C+col6%3E%3E1%2C+col7%3E%3E1%2C+col8%3E%3E1%2C+col9%3E%3E1%2C+col10%3E%3E1+from+1196140+&#038;o=kmllink&#038;g=col10%3E%3E1">Mapa en formato KML</a>)</small>

Este es el ejemplo de aplicación más simple que se me ocurrió. Con un poco más de trabajo, combinando con otras fuentes de información e introduciendo otras herramientas, podemos alcanzar perspectivas diferentes de esta información. Ese es el espíritu de la trasparencia y la publicación de información: poner los datos bajo otra luz, aprovechar las potentísimas herramientas que tenemos a disposición, aprender nuevas maneras de leerlos.

 [1]: http://www.santafe.gov.ar/
 [2]: http://www.elecciones.santafe.gov.ar/web-gral2011/
 [3]: http://geointa.inta.gov.ar/

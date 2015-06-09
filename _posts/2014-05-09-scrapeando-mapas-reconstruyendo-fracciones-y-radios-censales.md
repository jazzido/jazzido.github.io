---
title: 'Scrapeando mapas: reconstruyendo fracciones y radios censales'
author: manuel
layout: post
permalink: /2014/05/09/scrapeando-mapas-reconstruyendo-fracciones-y-radios-censales/
dsq_thread_id:
  - 2673284194
categories:
  - Uncategorized
---

**ACTUALIZACIÓN 9 de Junio de 2015**: INDEC publicó mapas de todas las provincias een formato SHP que contienen la división censal por radios. El siguiente post se mantendrá por razones históricas, pero recomiendo usar la fuente oficial disponible aquí: [http://www.indec.gov.ar/codgeo.asp](http://www.indec.gov.ar/codgeo.asp)

Como a todos los que trabajan con información pública, me interesa conseguir datos al menor nivel de *agregación* posible. Para decirlo de otro modo, en alta definición. Esa fue la idea detrás del [mapa de resultados electorales a nivel de *centro de votación*][1] que hice el año pasado en [LaNacion.com][2], como becario [Knight-Mozilla OpenNews][3]. El primer proyecto que desarrollé durante esa beca, fue una [visualización interactiva de los datos arrojados por los dos últimos censos de población en Argentina][4]. El plan original para ese proyecto también era mostrar los datos al menor nivel de agregación posible que para el censo son los *radios censales*, [definidos][5] como una porción del espacio geográfico que contiene en promedio 300 viviendas. Por desgracia, no estaban disponibles los mapas que definen los radios, ni los datos de las variables censales a ese nivel.

Los radios censales son partes de una *fracción censal*, que están a su vez contenidas en los departamentos o partidos, el segundo nivel de división administrativa en Argentina. Hoy es posible disponer de los datos del último censo a nivel de radio censal. Cabe destacar que no fueron publicados oficialmente, sino que [aparecieron en el *tracker* de BitTorrent The Pirate Bay][6] (!). Fueron publicados dentro de una aplicación basada en el sistema [REDATAM][7] y en [febrero pasado convertí esas tablas a un formato más universal][8]. Notar que es necesario el uso de REDATAM para análisis tales como tabulaciones cruzadas (tablas de contingencia). Por ejemplo, *universitarios que tienen acceso a internet por fracción censal en la provincia x*.

## La otra pieza del rompecabezas

Por supuesto, para visualizar estos resultados nos hacen falta las descripciones geográficas de las fracciones y radios censales. A partir de los datos del censo, podemos acceder a todas las variables —por ejemplo— del radio 300080111 y sabemos que está en la localidad de Colón de la provincia de Entre Ríos, pero no conocemos sus límites exactos. Sólo la [provincia de Buenos Aires][9] y la [Ciudad Autónoma de Buenos Aires][10] publican cartografía censal en formatos estándar. El [INDEC][11] mantiene un [sitio informativo sobre &#8220;unidades geoestadísticas&#8221;][12] en el que publica información geográfica hasta el nivel de radio censal (en formato SVG) pero desprovista de georeferenciación. Es decir, podemos obtener la geometría de cualquier provincia, departamento/partido, fracción o radio censal pero no está asociada al espacio físico. Este post describirá un método de georeferenciación de esos gráficos vectoriales, usando *otro* mapa publicado por INDEC como referencia.

## ¿Cómo se *scrapea* un mapa?

Podemos definir *scraping* como un proceso en el que recojemos información no estructurada y la ajustamos a un esquema predefinido. Por ejemplo, un *scraper* de resultados de búsqueda de Google que los almacene en una base de datos estructurada. Para este experimento, vamos a llevar a cabo un procedimiento análogo, pero aplicado a mapas. Es decir, vamos a tomar gráficos vectoriales no-georeferenciados del sitio de unidades geoestadísticas de INDEC y vamos a ajustarlo a una proyección geográfica (POSGAR 94 Argentina 3).

## Malabares vectoriales

Los gráficos publicados en el nivel &#8220;departamento&#8221; de los SVG de INDEC son un gráfico vectorial de un partido/departamento que contiene *fracciones*, que son el siguiente nivel en la jerarquía de unidades geoestadísticas:

![Fracciones de Bahía Blanca][13]

En el gráfico de arriba, vemos los límites del partido de Bahía Blanca y sus divisiones internas (fracciones).

Aunque INDEC no publica las unidades geoestadísticas de menor nivel en un formato georeferenciado, sí pone a disposición un [mapa de departamentos][14] tal como fueron considerados para el último censo. Ese mapa será nuestra referencia para georeferenciar los SVGs ya mencionados.

Para eso, vamos a tomar los puntos extremos (extremos de la envolvente) de un partido tal como fue publicado en el SVG y del *mismo* partido tal como fue publicado en el mapa de departamentos, que sí está georeferenciado:

![Puntos correspondientes][15]

Los puntos del mapa de la derecha están georeferenciados, y &#8220;sabemos&#8221; (asumimos, en realidad) que se corresponden con los puntos del de la izquierda. Con ese par de conjuntos de puntos, podemos calcular una transformación tal que convierta los vectores no-georeferenciados al espacio de coordenadas del mapa georeferenciado. Para eso, vamos a usar el módulo [`transform`][16] de la librería [`scikit-image`][17]. Aclaremos cuanto antes, para que no se ofendan los cartógrafos y geómetras: este procedimiento es muy poco formal (se muy poco de cartografía y de geometría) y poco preciso (el SVG está muy simplificado con respecto al mapa original).

Aplicando esa transformación a todas las fracciones contenidas en el departamento, y procediendo de manera análoga para el siguiente nivel geoestadístico (radios), vamos a obtener una aproximación bastante burda a un mapa de fracciones y radios censales.

## Implementación

El procedimiento, que se ha descripto de manera muy resumida, está implementado en el programa [`georef_svg.py`][18]. Para correrlo hay que instalar las dependencias listadas en `requirements.txt` y bajar los SVGs del sitio de INDEC. Para eso, se provee el script [`source_svg/download.sh`][19]. Ejecutando `python georef_svg.py .`, obtendremos dos *shapefiles* `fracciones.shp` y `radios.shp`, que contienen el resultado del proceso. Un ejemplo del resultado de este proceso se incluye en el directorio [`output_shp`][20].

El resultado no debe ser considerado como un mapa usable de fracciones y radios, dadas las imprecisiones y errores que contiene. En el mejor de los casos, quizás pueda ser un punto de partida para la confección de un mapa apropiado&#8230;hasta que INDEC (o quien corresponda) publique la información en un formato geográfico estándar.

 [1]: http://interactivos.lanacion.com.ar/mapa-elecciones-2013/
 [2]: http://lanacion.com
 [3]: http://opennews.org
 [4]: http://interactivos.lanacion.com.ar/censo/#Poblacion_Total-intercensal
 [5]: https://www.santafe.gov.ar/index.php/web/content/view/full/163619/%28subtema%29/93664 "SantaFe.gov.ar"
 [6]: https://thepiratebay.se/torrent/9642504/CPV2010BArgVer1.exe
 [7]: http://www.eclac.cl/redatam/default.asp?idioma=IN
 [8]: http://blog.jazzido.com/2014/02/24/resultados-censo-2010-radio-censal/
 [9]: http://www.ec.gba.gov.ar/estadistica/censo2010/cartografia.html
 [10]: http://www.buenosaires.gob.ar/areas/hacienda/sis_estadistico/cartografia_censal_cnphv_2010.php?menu_id=35240
 [11]: http://www.indec.gov.ar
 [12]: http://www.opex.sig.indec.gov.ar/codgeo/index.php?pagina=mapas
 [13]: https://github.com/jazzido/svg-map-scrape/raw/master/img/06056.png?raw=True
 [14]: http://www.indec.gov.ar/default_gis.htm
 [15]: https://github.com/jazzido/svg-map-scrape/raw/master/img/puntos_correspondientes.png?raw=true
 [16]: http://scikit-image.org/docs/dev/api/skimage.transform.html
 [17]: http://scikit-image.org/
 [18]: https://github.com/jazzido/svg-map-scrape/blob/master/georef_svg.py
 [19]: https://github.com/jazzido/svg-map-scrape/blob/master/source_svgs/download.sh
 [20]: https://github.com/jazzido/svg-map-scrape/tree/master/output_shp

---
title: Resultados del Censo 2010 Argentina a nivel de radio censal
author: manuel
layout: post
permalink: /2014/02/24/resultados-censo-2010-radio-censal/
dsq_thread_id:
  - 2313902604
categories:
  - Uncategorized
---
Los censos de población son uno de los conjuntos de datos fundamentales que produce el Estado. En Argentina, el último Censo Nacional de Población, Hogares y Viviendas se realizó el día 27 de octubre de 2010. Más de 3 años después, [aparecieron en la web][1] los resultados del cuestionario básico, desagregados a nivel de *radio censal* (la división espacial más chica en la que se publica el censo) y publicados en forma de una aplicación Windows desarrollada en base al sistema [REDATAM][2]

**¿Qué es REDATAM?**

Como muchos institutos nacionales de estadística, el [INDEC][3] usa el programa REDATAM para la confección y publicación de la base de datos de resultados censales. Este sistema es desarrollado por el [CELADE][4], dependiente de la [Comisión Económica para América Latina y el Caribe][5].

Considerando su adopción por numerosos organismos gubernamentales y el prestigio de sus responsables, no dudo de la alta calidad del sistema. No obstante, [como bien señaló Andrés Vázquez en su blog][6], REDATAM presenta algunas complicaciones a la hora de reutilizar los datos con las herramientas, prácticas y convenciones a las que nos hemos acostumbrado en los últimos años. Llama la atención, además, que no estén públicamente disponibles ni su código fuente, ni la especificación de los formatos que utiliza para almacenar la información.

REDATAM es una aplicación con interfaz gráfica de usuario, pero también incluye un &#8220;procesador estadístico&#8221; ([R+SP Process][7]) que permite definir y exportar tablas mediante programas escritos en un lenguaje propio del sistema. Dado el [diccionario de variables y entidades almacenadas en REDATAM][8], es posible construir consultas que exporten todas las variables a un archivo para luego convertirlo a un formato abierto que facilite su reutilización.

***Liberando* la información**

Publiqué en GitHub [un conjunto de scripts en lenguaje Python][9], que generan las *queries* apropiadas para ser ejecutadas por REDATAM y exportar casi todas las variables a DBF. Estos últimos son luego convertidos a archivos CSV (valores separados por comas). También publiqué los [resultados de este procesamiento][10].

Cabe aclarar que estos archivos *no son una fuente oficial de información y no asumo ninguna responsabilidad sobre su uso.*. Las consultas generadas funcionan únicamente para los datos mencionados antes, pero es posible que esta metodología sea aplicable a otras bases de información publicadas con REDATAM.

**¿Dónde están los radios censales?**

Los radios censales —contenidos en las *fracciones *censales— son una división administrativa del espacio. Su tamaño está definido por la cantidad de viviendas que contienen: una fracción censal contiene un promedio de 5000 viviendas y un radio contiene un promedio de 300 ([fuente][11]). Sólo la[ provincia de Buenos Aires][12] y la [Ciudad Autónoma de Buenos Aires][13] publican la definición de estas divisiones en formatos geográficos apropiados. El INDEC mantiene un sitio informativo sobre &#8220;[Unidades Geoestadísticas][14]&#8221; para todo el país, pero publica los polígonos de los radios y fracciones censales en forma de archivos SVG desprovistos de información geográfica (imprescindible para *georeferenciar* los datos)

**Un ejemplo**

Creo que visualizar información pública en &#8220;alta resolución&#8221; es valioso. El año pasado, como becario del programa [OpenNews][15] en el diario [La Nación][16], participé en el desarrollo de una [infografía interactiva sobre los resultados de las elecciones legislativas][17] que introdujo la novedad de mostrar los resultados para cada *centro de votación*, en lugar de hacerlo a nivel distrital como suele ser el caso. Para mi sorpresa, tuvo muchísima repercusión —asumí que solo iba a interesarle a unos pocos *nerds* de la política y la información pública. Combinando un mapa de radios censales de Bahía Blanca y la información extraída de REDATAM, se puede hacer —por ejemplo— un mapa del porcentaje de hogares con algún indicador de necesidades básicas insatisfechas:

<iframe width="100%" height="520" frameborder="0" src="http://manuelo.cartodb.com/viz/62da691e-9d72-11e3-8e37-0ed66c7bc7f3/embed_map?title=true&#038;description=true&#038;search=false&#038;shareable=true&#038;cartodb_logo=true&#038;layer_selector=false&#038;legends=true&#038;scrollwheel=true&#038;sublayer_options=1&#038;sql=SELECT%20(bahia_hogares_nbi.connbi%20%2F%20bahia_hogares_nbi.total)%20*%20100%20as%20pct_nbi%2C%20bahia_hogares_nbi.connbi%20as%20hogares_con_algun_nbi%2C%20bahia_hogares_nbi.total%20as%20total_hogares%2C%20%20bahia_radios.*%20FROM%20bahia_hogares_nbi%0Ainner%20join%20bahia_radios%20on%20bahia_radios.fraccion%20%3D%20%20substring(bahia_hogares_nbi.radio%20from%206%20for%202)%20and%20substring(bahia_hogares_nbi.radio%20from%208)%20%3D%20bahia_radios.radio&#038;sw_lat=-38.76479194327963&#038;sw_lon=-62.417449951171875&#038;ne_lat=-38.633500077966396&#038;ne_lon=-62.087860107421875"></iframe>

Este ejemplo es lo mínimo que puede hacerse con esta información. La publicación en un formato más ameno que REDATAM, espero, facilitará su utilización y aumentará la conciencia sobre el valor de la información pública publicada de forma apropiada.

[Muchas gracias a [Andy Tow][18] y a [Andrés Vázquez][19] por la ayuda y comentarios]

 [1]: https://thepiratebay.se/torrent/9642504/CPV2010BArgVer1.exe
 [2]: http://www.eclac.cl/redatam/default.asp?idioma=IN
 [3]: http://www.indec.gov.ar
 [4]: http://www.eclac.cl/celade/default.asp?idioma=ES
 [5]: http://www.eclac.cl/
 [6]: http://asesorensistemas.com.ar/blog/Anatomia-de-los-censos-en-latinoamerica
 [7]: http://www.redatam.org/cdr/Tutoriales/Process_Esp.html#s6
 [8]: https://github.com/jazzido/liberacion-del-censo/blob/master/variables.ini
 [9]: https://github.com/jazzido/liberacion-del-censo
 [10]: http://dump.jazzido.com/CNPHV2010-RADIO/
 [11]: https://www.santafe.gov.ar/index.php/web/content/view/full/163619/(subtema)/93664
 [12]: http://www.ec.gba.gov.ar/estadistica/censo2010/cartografia.html
 [13]: http://www.buenosaires.gob.ar/areas/hacienda/sis_estadistico/cartografia_censal_cnphv_2010.php?menu_id=35240
 [14]: http://www.opex.sig.indec.gov.ar/codgeo/
 [15]: http://www.opennews.org
 [16]: http://www.lanacion.com.ar
 [17]: http://interactivos.lanacion.com.ar/mapa-elecciones-2013/
 [18]: http://twitter.com/andy_tow
 [19]: http://twitter.com/elnomoteta

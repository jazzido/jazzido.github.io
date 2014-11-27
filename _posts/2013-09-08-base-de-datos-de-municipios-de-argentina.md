---
title: Base de datos de Municipios de Argentina
author: manuel
layout: post
permalink: /2013/09/08/base-de-datos-de-municipios-de-argentina/
dsq_thread_id:
  - 1736515218
categories:
  - Uncategorized
---
La [Secretaría de Asuntos Municipales][1] del [Ministerio del Interior][2] (Argentina) confecciona una base de datos bastante interesante: el listado de todos los municipios del país. Como mucha de la información generada por los estados —municipios, provincias o la nación—, no está publicada de manera tal que podamos usarla fácilmente. Esto está empezando a cambiar; algunos gobiernos están creando portales de información pública ([datospublicos.gob.ar][3], [data.buenosaires.gob.ar][4], [gabierto.bahiablanca.gob.ar][5], entre otros) donde se ponen a disposición algunas de las bases de datos que genera el Estado *(vemos que la información publicada en estos portales es bastante inocua —salvo honrosas excepciones—, pero también entendemos que esto recién comienza).* Como proyecto de domingo, escribí un *scraper* para extraer el listado de municipios del sitio web en el que está publicado. [El código está disponible][6] bajo licencia MIT y [los datos los subí a un Fusion Table][7].

<iframe src="https://www.google.com/fusiontables/embedviz?q=select+col4+from+1QzJGCPOEaUh6RhWYaR8MBhHCRpQ_IK-amkS39o8&viz=MAP&h=false&lat=-37.028125614582585&lng=-64.82016987109375&t=1&z=4&l=col4&y=4&tmplt=6&hml=TWO_COL_LAT_LNG" height="300" width="100%" frameborder="no" scrolling="no"></iframe>

Hay información interesante en esta base de datos: nombre del jefe de gobierno de cada municipio, ubicación geográfica, fecha de fundación, página web, información de contacto. Notablemente, sólo para el 30% de los más de 2200 municipios que figuran en esta base de datos hay un sitio web.

 [1]: http://www.mininterior.gov.ar/municipios/municipios_home.php?idName=municipios&idNameSubMenu=&idNameSubMenuDer=
 [2]: http://www.mininterior.gov.ar/
 [3]: http://datospublicos.gob.ar
 [4]: http://data.buenosaires.gob.ar
 [5]: http://gabierto.bahiablanca.gob.ar
 [6]: https://github.com/jazzido/muniscraper
 [7]: https://www.google.com/fusiontables/DataSource?docid=1QzJGCPOEaUh6RhWYaR8MBhHCRpQ_IK-amkS39o8

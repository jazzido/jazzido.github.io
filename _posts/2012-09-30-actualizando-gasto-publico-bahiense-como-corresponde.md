---
title: Actualizando Gasto Público Bahiense como corresponde
author: manuel
layout: post
permalink: /2012/09/30/actualizando-gasto-publico-bahiense-como-corresponde/
dsq_thread_id:
  - 865945015
categories:
  - Uncategorized
---
Desde que está online, <a title="Gasto Público Bahiense" href="http://gastopublicobahiense.org" target="_blank">Gasto Público Bahiense</a> se actualizaba mediante la infame técnica de *<a href="http://es.wikipedia.org/wiki/Screen_scraping" target="_blank">screen scraping</a>, *que consiste en hacer que un programa acceda a interfaces pensadas para ser usadas por nosotros los seres humanos. Los *scrapers* son programas frágiles, que no resisten la menor modificación de la &#8220;pantalla&#8221; de la que extraen información. Esto se vió el año pasado, cuando la Municipalidad rediseño su site e introdujo un [CAPTCHA][1], causando que se rompa el *scraper*  y haciendo un ruido mediático que —a la distancia— fue muy divertido, ayudó a promocionar el proyecto y algunos conceptos sobre transparencia, rendición de cuentas y datos abiertos. Inclusive <a href="http://www.lanacion.com.ar/1387132-complican-al-acceso-a-informacion-publica-en-bahia-blancacomplican-al-acceso-a-informacion-publica-en-bahia-blanca?utm_source=twitterfeed&utm_medium=twitter" target="_blank">salimos en la <em>home</em> de La Nación</a>, [en Perfil.com][2], [FOPEA manifestó su preocupación][3] y [el entonces secretario de hacienda Ramiro Villalba dijo que yo era un *&#8220;tremendo mentiroso malintencionado o un ignorante tecnológico&#8221;*][4].

Pero desde hace un par de días, la Municipalidad de Bahía Blanca, a instancias de su Secretario de Innovación y Gobierno Abierto, le dio a GPB acceso a un *web service* a través del que actualizar los datos de órdenes de compra es fácil y rápido. (comparar el scraper anterior  [[<tt>compras.py</tt>][5]] con el cliente actual [[<tt>compras_ws.py</tt>][6]]).

Hay que decirlo, nobleza obliga: hace un año [nos obstaculizaban el laburo con CAPTCHAs][7], hoy nos lo hacen fácil con un *web service*. &nbsp;

 [1]: http://es.wikipedia.org/wiki/CAPTCHA
 [2]: http://www.perfil.com/contenidos/2011/11/17/noticia_0030.html
 [3]: http://fopea.org/Inicio/Fopea_expresa_su_preocupacion_por_cambios_en_el_sitio_web_del_gobierno_de_Bahia_Blanca
 [4]: http://www.cafexmediodigital.com.ar/index.php?option=com_content&task=view&id=14148&Itemid=1
 [5]: https://github.com/jazzido/GPB/blob/master/gpbscraper/gpbscraper/spiders/compras.py
 [6]: https://github.com/jazzido/GPB/blob/master/gpbscraper/gpbscraper/spiders/compras_ws.py
 [7]: http://blog.jazzido.com/2011/07/05/algunas-notas-sobre-el-captcha-gate/

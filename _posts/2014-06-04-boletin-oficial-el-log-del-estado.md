---
title: 'Boletín Oficial: el log del estado'
author: manuel
layout: post
permalink: /2014/06/04/boletin-oficial-el-log-del-estado/
dsq_thread_id:
  - 2736934834
categories:
  - Uncategorized
---
La metáfora surgió en el ya legendario [primer *hackatón* de información pública][1] que organizamos en [GarageLab][2] hace casi 4 años: así como el software suele informar su actividad en un *log*, el estado hace lo mismo a través de los boletines oficiales que publica diariamente. La versión del estado nacional estuvo dividida históricamente en tres secciones: Legislación y avisos oficiales, sociedades, contrataciones. Hace pocas semanas se incorporó una cuarta sección, donde se publican los nuevos nombres de dominio registrados a través de [NIC.ar][3].

Desde GarageLab experimentamos con el problema de recuperar, estructurar y analizar la información contenida en las primeras 3 secciones del Boletín. Durante la edición 2011 de [Desarrollando América Latina][4], los miembros de [Banquito][5] desarrollaron [un prototipo de *scraper* e interfaz de consulta para la tercera sección][6], mientras que [Damián Janowski][7] y yo trabajamos en un [prototipo de scraper y *named entity recognizer* para la sección de sociedades][8].

En preparación para el hackatón panamericano [La Ruta del Dinero][9] que se va a hacer este sábado 7 de junio de 2014, retomé las ideas con las que estuvimos jugando hace unos años. La composición de los directorios, novedades, quiebras y edictos de las empresas registradas en el país son una fuente de información importante para seguir la ruta del dinero.

## boletinoficial.gov.ar

Además de ser publicado en papel y en PDF, el Boletín Oficial tiene un sitio web que publica la información de manera más o menos estructurada. Su usabilidad y performance dejan bastante que desear, pero es bastante fácil de *scrapear* gracias a que las páginas se generar a partir de un servicio que emite documentos XML ([Ver ejemplo][10])

Es decir, el sistema que publica el Boletín en la web, almacena los datos estructurados pero no nos ofrece la posiblidad de obtenerlos de esa manera.

## Entonces hay que *scrapear*

Publiqué en [GitHub][11] un script en Python que obtiene los avisos de la segunda sección para una fecha dada y emite un archivo CSV: <https://github.com/jazzido/boscrap>.

Para usarlo, bajar el contenido del repositorio, instalar las dependencias con `pip install -r requirements.txt` y ejecutar:

`python boscrap.py 2014-06-04`

 [1]: http://garagelab.tumblr.com/post/1087778689/hackathongobiernoabierto
 [2]: http://garagelab.cc
 [3]: http://nic.ar
 [4]: http://desarrollandoamerica.org/
 [5]: http://www.elgalpondebanquito.com.ar/
 [6]: https://github.com/banquito/boletin_oficial
 [7]: https://github.com/djanowski
 [8]: https://github.com/garagelab/boletin-sociedades
 [9]: http://larutadeldinero.info/
 [10]: http://www.boletinoficial.gov.ar/Content/XML/Avisos/02/2014/05/26/4578905.xml
 [11]: http://github.com
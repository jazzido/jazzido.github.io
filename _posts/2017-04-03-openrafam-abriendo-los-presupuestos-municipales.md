---
title: OpenRAFAM&#58; Abriendo los Presupuestos Municipales
author: manuel
layout: post
permalink: /2017/04/03/openrafam-abriendo-los-presupuestos-municipales
categories:
  - Uncategorized
---

Han ocurrido muchas cosas desde los primeros esfuerzos para construir recursos de información basados en datos públicos gubernamentales. Desde [Dinero y Política](https://web-beta.archive.org/web/20091010004652/www.dineroypolitica.org), esfuerzo fundacional de [Gonzalo Iglesias](http://twitter.com/gonzaloiglesias) y [Poder Ciudadano](http://poderciudadano.org), y [Gasto Público Bahiense](http://blog.jazzido.com/2010/07/18/un-experimento-sobre-tecnologia-y-transparencia-gubernamental/) (GPB) —de quien escribe—, algunos gobiernos en Argentina adoptaron políticas de transparencia acompañadas de implementaciones de herramientas online. Pese a estos 8 años de historia y evolución en el país, los resultados son bastante pobres. Los proyectos surgidos de la sociedad civil tienen problemas de sustentabilidad, el sector privado no se interesa en el tema, y el sector público tiene un [enorme déficit en su capacidad de construir servicios públicos digitales](http://blog.jazzido.com/2016/04/10/digital-public-services-user-experience-matters).

El uso de los recursos públicos es uno de las fuentes de información más importante que genera el gobierno en cualquiera de sus niveles. En particular, el presupuesto y su ejecución quizás sea el *dataset* más importante que produce y mantiene una administración pública.

Un sistema que corre en al menos 135 municipios
-----------------------------------------------

Unas de las primeras veces que hablé públicamente sobre GPB fue hace casi 7 años en el [Hackatón de Datos Públicos y Gobierno Abierto](http://www.redusers.com/noticias/primer-%25E2%2580%259Chackathon%25E2%2580%259D-de-datos-publicos-y-gobierno-abierto-en-argentina/) que organizamos con Garage Lab junto al Programa de Gobierno Electrónico de la Universidad de San Andrés. [Hay video de esa charla](https://vimeo.com/15558781). Durante el evento de dos días, que recuerdo como el mejor hackatón al que haya asistido, alguien me comentó que la información administrativa (presupuestos, personal, tasas, etc) de los 135 municipios bonaerenses se almacenaba en un sistema llamado *RAFAM*, que corre en todos los partidos de la provincia. Con la candidez que tenemos los ingenieros con poca experiencia en política, dijimos —«Obvio! Hay que escribir software que extraiga los datos de esos 135 servidores Oracle, convencer a los intendentes que lo instalen, y ya está: información fiscal para todos y todas». Para eso, necesitábamos acceder a alguno de esos sistemas para poder hacer la *ingeniería reversa* correspondiente. No llegamos muy lejos; como suele suceder en muchos esfuerzos voluntaristas el entusiasmo se fue apagando luego del hackatón. Además, los contactos iniciales con algunos municipios nos hicieron pensar que ningún secretario de hacienda iba a darnos acceso irrestricto a su sistema contable para poder *reversearlo*.

Pero la idea de un componente de software que fuese capaz de extraer datos de 135 bases de datos municipales era demasiado potente como para dejarla ir así no más.

La conexión bahiense
--------------------

[Ya he contado la historia muchas veces](https://www.youtube.com/watch?v=bSBh6Cm2Hpg&app=desktop): cuando lancé GPB, el [gobierno municipal estaba abiertamente en contra de la iniciativa](http://youtube.com/watch?v=w75JkBS4tfE). La nueva administración que asumiría en 2012, en cambio, creó una de las primeras oficinas dedicadas a la innovación y gobierno abierto del país con la que tuve excelente relación desde el comienzo hasta el nuevo cambio de intendente a fines de 2015. Fue con esa dependencia, a través de su secretario Esteban Mirofsky, que [organizamos otro hackatón](http://www.bahiablanca.gov.ar/noticias/vernoticia.aspx?k=TE5oss/VcQY=) del que surgieron proyectos pioneros como [un sistema de información ambiental en tiempo real](http://www.quepasabahiablanca.gov.ar/) y un prototipo de plataforma de información basada en los datos de la tarjeta de transporte. Sobre este último, [escribí un blog post](http://blog.jazzido.com/2014/10/26/ipython-matplotlib-y-transporte-publico-en-bahia-blanca/) y, junto a Cristian Jara Figueroa, un [trabajo final para un curso de modelos de movilidad humana](http://blog.jazzido.com/wp-content/uploads/2017/07/tap-locations-hotspots.pdf) que tomé durante mi maestría en MIT.

El último proyecto en el que colaboré con la Agencia de Innovación y Gobierno Abierto fue una plataforma para visualizar prespuestos públicos, en el marco de mi tesis de maestría. Finalmente, 5 años después del mítico hackatón de 2010, un municipio bonaerense estuvo dispuesto a darme acceso a su sistema RAFAM para estudiarlo y poder extraer la información que almacena.

Levantando el capot de RAFAM
----------------------------

Pensaba que escribir las consultas SQL para extraer datos de RAFAM sería una tarea relativamente fácil, pero fui víctima una vez más del optimismo del que sufrimos los ingenieros. Cuando me conecté a esa base de datos, me encontré con un sistema con casi 2000 tablas, cientos de vistas y cientos de *stored procedures*. No iba a ser tan fácil.

![Muchas tablas](http://blog.jazzido.com/wp-content/uploads/2017/04/tablas_rafam.png)

Por suerte, el Ministerio de Economía de la provincia de Buenos Aires publica en su sitio web actualizaciones del sistema RAFAM, que contienen archivos de Crystal Reports. Estos, a su vez, contienen las queries necesarias para generar diversos reportes. Entre ellos, estaban los que nos interesan: ejecución presupuestaria.

Armados con esta información, junto al [Dr. Gastón Ávila](https://twitter.com/AvilaGas) escribimos una serie de queries que permitieron extraer tablas del estado de ejecución presupuestaria —tanto de gastos como de recursos— desagregadas según todos los criterios en los que se clasifica un presupuesto público ([ver mi post anterior sobre el tema](http://blog.jazzido.com/2016/10/12/datos-presupuestarios-argentina-sitio-del-ciudadano))

¿Y el código?
-------------

Siempre tuve la intención de liberar el código de Presupuesto Abierto y [SpendView](http://spendview.media.mit.edu), ambos desarrollados dentro de mi tesis de maestría. El optimismo ingenieril otra vez al ataque: el código escrito durante una carrera de posgrado está lejos de ser publicable, pero quiero empezar con ese proceso más temprano que tarde. Hoy empiezo publicando el código del programa que sirve para *extraer* los datos de ejecución presupuestaria de una instancia de RAFAM. Consiste en dos módulos escritos en lenguaje Python.

-   `rafam_db`: encapsula las consultas SQL que escribimos basándonos en los reportes que genera el sistema RAFAM.
-   `rafam_extract`: comando para ejecutar las consultas del modulo `rafam_db`

Si este código aporta algún valor (espero que sí), [está en las consultas SQL](https://github.com/jazzido/OpenRAFAM/tree/master/rafam_db/rafam_db/queries). Los comandos escritos en Python son bastante genéricos, y no es necesario usarlos.

El código está en mi perfil de GitHub: <https://github.com/jazzido/openrafam>

¿Para qué sirve todo esto?
--------------------------

Pese a los anuncios grandilocuentes, los eventos, los subsidios y los hackatones, los servicios públicos basados en datos avanzaron muy poco en los ~8 años de historia del movimiento de datos abiertos en Argentina. Estoy convencido de que la falta de voluntad política es la razón más importante, pero también la falta de capacidades técnicas en el sector público.

Quizás, ayudando desde afuera con lo que sabemos hacer (programar), animemos a los municipios a construir sistemas de información basados en datos tanto para consumo interno como externo.




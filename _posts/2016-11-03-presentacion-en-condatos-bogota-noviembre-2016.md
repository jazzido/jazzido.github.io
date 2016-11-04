---
title: Presentación en CONDATOS (Bogotá, Noviembre 2016)
author: manuel
layout: post
permalink: /2016/11/03/presentacion-en-condatos-bogota-noviembre-2016
categories:
  - Uncategorized
---

A continuación, algunas notas que usé para la presentación que di en la [Conferencia CONDATOS 2016](http://condatos.org), realizada en Bogotá el 3 de Noviembre de 2016

Presentación
============

Espero que los 20 minutos me alcancen para lo que quiero hablar:

-   Una breve historia de mi primer proyecto de *Open Data*: Gasto Público Bahiense
-   Luego de ¿8? años el movimiento de Datos y Gobierno Abierto aprendió muchas cosas, pero seguimos manteniendo algunos mitos. Me gustaría hacer un ejercicio de refutación.
-   ¿Cómo podemos construir recursos de información pública más eficientes?

Historia de Gasto Público Bahiense
==================================

Inicios
-------

Bahía Blanca, la ciudad del sur de Argentina donde nací, publica información detallada acerca de sus compras desde el año 2001, mucho antes de que se inventara el concepto de *datos abiertos*. El sistema donde se publica esa información, con los sucesivos alcaldes, se achicó progresivamente; cada vez publicaba menos información.

![Así publicaba las compras el Municipio de Bahía Blanca en 2010](/wp-content/uploads/2016/11/bahia_compras.png)

En Julio de 2010, gracias a la *procrastinación* (estaba trabajando en un proyecto un poco aburrido). Noté que esos datos (un listado de órdenes de compra) estaban disponibles en el sitio del Municipio y construí un sitio web muy simple que facilitaba su consulta y navegación.

![Así era gastopublicobahiense.org en 2010](/wp-content/uploads/2016/11/gpb_2010.png)

Lo puse online, y me explotó en la cara. De repente, empecé a atender llamados de periodistas que me preguntaban "quién financiaba el proyecto", y cuáles eran mis intenciones.

Ignorado por las autoridades
----------------------------

Pese a que el proyecto fue declarado de interés municipal por un legislador local, y tuvo un gran repercusión mediática y política, el gobierno municipal de entonces no acusó recibo de la herramienta durante un año.

CAPTCHA y repercusión
---------------------

Hasta que justo 1 año después de haber lanzado GPB, el gobierno rediseño su sitio web y puso un CAPTCHA para evitar que "los robots se roben la información y hagan más lento al sistema".

Al contrario de lo que el Secretario de Haciendo de entonces —supongo— esperaba lograr con la introducción de un CAPTCHA en el sitio web donde publicaban las órdenes de compra, su decisión ayudó a la difusión del proyecto. Por primera vez, Gasto Público Bahiense alcanzó la prensa nacional:

 - [Complican al acceso a información pública en Bahía Blanca](http://www.lanacion.com.ar/1387132-impiden-al-acceso-a-informacion-publica-en-bahia-blanca) (La Nación, 6/7/2011)
 - [Fopea expresa su preocupación por cambios en el sitio web del gobierno de Bahía Blanca](http://www.fopea.org/fopea-expresa-su-preocupacion-por-cambios-en-el-sitio-web-del-gobierno-de-bahia-blanca/) (Foro de Periodismo Argentino, 13/7/2011)
 - [Bahía Blanca: Pagos Millonarios, pauta en alza](https://web.archive.org/web/20110325071355/http://www.desafioeconomico.com/noticia_detalle_1.php?noticia_id=4794) (Revista Desafío Económico, 14/3/2011)

Modificamos el sistema de extracción de datos (saltar ese CAPTCHA era **muy** fácil) e hicimos que GPB siguiera funcionando. Y ya que estábamos, con la colaboración de amigos y colegas, rediseñamos el sitio.

![Así quedó GPB después del rediseño (mucho más lindo, ¿no?)](/wp-content/uploads/2016/11/gpb_2015.png)

Creación de la Secretaría de Innovación y Gobierno Abierto
----------------------------------------------------------

En 2012 —GPB ya llevaba 2 años funcionando— cambió el alcalde de la ciudad. El nuevo gobierno creó una Secretaría de Innovación y Gobierno Abierto, inspirado por la experiencia de nuestro proyecto. Fue la primera vez que tuve contacto con un representante del Municipio. La nueva Secretaría, en lugar de entorpecer el acceso a la información, nos lo facilitó dándonos acceso a un *web service*.

Demanda de los datos por parte del sector público
-------------------------------------------------

La visibilidad de estos recursos de información producida y publicada por el estado rindió sus frutos. Hoy, en Bahía Blanca, los políticos, periodistas y los ciudadanos interesados, consideran el acceso a los datos como un *derecho adquirido*. En los últimos días, esta problemática apareció en la agenda política y mediática.

-   [Denuncian la desaparición de decretos y resoluciones de la web Gobierno Abierto](http://www.lanueva.com/la-ciudad/882609/denuncian-la-desaparicion-de-decretos-y-resoluciones-de-la-web-gobierno-abierto.html) (La Nueva Provincia, 20/10/2016)
-   [Datos abiertos: "Hay que preguntar a Sapem por qué no los suben"](http://www.lanueva.com/la-ciudad/881037/datos-abiertos--hay-que-preguntar-a-sapem-por-que-no-los-suben.html) (La Nueva Provincia, 05/10/2016)

Cierre y apoyo de la Municipalidad
----------------------------------

Hace alrededor de 1 mes, decidí dar de baja al proyecto Gasto Público Bahiense. Fueron más de 6 años de mantener el sistema funcionando, y de pagar el servidor donde estaba alojado. Apenas anuncié el cierre, me contactaron políticos de la oposición y del gobierno actual para proponerme soluciones. Finalmente, la [Secretaría de Modernización y Gobierno Abierto](http://gabierto.bahiablanca.gob.ar/) de la Municipalidad de Bahía Blanca ofreció proporcionarnos un servidor en el que alojar la aplicación, para que pueda seguir funcionando. En los próximos días, GPB volverá a estar en línea.

Mitos de Open Data
==================

"Publicaremos datos y los desarrolladores construirán aplicaciones, y florecerán cientos de nuevas compañías"
-------------------------------------------------------------------------------------------------------------

La idea de "Open Data para el desarrollo de emprendimientos privados", frecuentemente se ilustra con proyectos de gran escala como el Sistema de Posicionamiento Global (GPS) o el servicio meteorológico de EE.UU. Semejantes obras de infraestructura digital no pueden ser comparadas con liberar un pocos datasets y pretender que los emprendedores privados los tomen para producir valor.

Además, en mi opinión, en este mito subyace una cierta idea de voluntarismo: ciudadanos cumpliendo el rol del estado. No hay nada de mal en eso, pero —tomando prestado un concepto de ingeniería— *no escala*. Los proyectos sustentables necesitan de la infraestructura y profesionalismo que sólo puede aportar una organización.

Los "ciudadanos comunes" se involucrarán en el control y auditoría de la gestión pública
----------------------------------------------------------------------------------------

La idea de *ciudadano común* no está clara, la experiencia muestra que no existen —en el contexto de usuarios ideales de un recurso de información pública. Todo aquel que se embarca en la tarea de interpretar cuentas públicas, o procesar indicadores socio-económicos, tiene un *propósito*. Si no, ¿por qué va a ponerse a trabajar en eso, que es tan complicado?

Cualquier "data" es buena: PDFs, planillas Excel mal hechas, APIs.
------------------------------------------------------------------

Señal/ruido: Lo que es señal para usted, puede ser ruido para mí, o viceversa. El diseño de un dataset (medidas de un fenómeno), implica necesariamente una perspectiva u opinión sobre este.

¿Cómo mejorar los portales de datos abiertos?
=============================================

La presencia en la web de las iniciativas gubernamentales de Open Data, en general, se materializa en la forma de un *portal de Datos Abiertos*. Hay productos, abiertos y comerciales, que permiten a una administración pública implementar este tipo de sitios con relativo poco esfuerzo. Algunos de ellos:

-   [CKAN](http://ckan.org/)
-   [Junar](http://junar.com/?lang=en)
-   [Socrata](https://socrata.com/)

Por supuesto, un repositorio de información es un requisito *necesario* para una iniciativa seria de publicación de datos generados por el estado, pero no es *suficiente*.

Tomo prestada una metáfora de mi supervisor de tesis en MIT César Hidalgo:

> Imagínense ir a hacer las compras a un supermercado donde todos los productos están en cajas idénticas. Pasta, shampoo, aceite: todos en la misma caja. La experiencia de comprar en ese supermercado es parecida a la de buscar una base de datos en casi cualquier portal de datos públicos. Para poder saber qué contienen, tengo que descargarlo y abrirlo con una aplicación, o interactuar con una API; un procedimiento que requiere de conocimientos relativamente avanzados de programación ([What's Wrong with Open-Data Sites--and How We Can Fix Them](https://blogs.scientificamerican.com/guest-blog/what-s-wrong-with-open-data-sites-and-how-we-can-fix-them/), Scientific American)

El paradigma de diseño de los portales de datos, por otro lado, establece un sesgo hacia abrir *más* datasets y no hacia abrirlos *mejor*. Notemos, por ejemplo, que el *número de datasets* disponibles se enfatiza en todos los portales de datos. No obstante, es fácil fabricar datasets. Por ejemplo, es frecuente ver en muchos portales de datos, una tabla de presupuesto "partida" en varios archivos. Con ese viejo truco, podemos fabricar varios datasets a partir de uno.

La *descubribilidad* de la información publicada en los portales de datos también es un problema. Google —todavía— no sabe indexar tablas llenas de números. Entonces, así como generamos visualizaciones a partir de los datos, también podemos generar *texto* (lo único que Google sabe indexar).

También es crucial facilitar el *sharing* en redes sociales, que es el origen de una porción sustancial del tráfico de cualquier sitio web.

El paradigma clásico del *dashboard* (tablero de control), no es suficiente para satisfacer el objetivo de hacer nuestra información más accesible, transparente y compensible. Los dashboards, tales como el panel de control de un vehículo, están diseñados para obtener —en un golpe de vista– la información necesaria para tener control de un sistema o proceso. Este diseño no se traslada muy bien a la web. En principio, un dashboard no aprovecha la interacción más natural que tenemos en cualquier dispositivo: el scroll. De hecho, mucha gente está "descubriendo" que a los usuarios nos gusta scrollear, más que apretar botones [1].

Algunos ejemplos salidos de [Datawheel](http://datawheel.us) y [Macroconnections](http://macro.media.mit.edu):

SpendView
---------

Mi proyecto de tesis de Maestría en el grupo Macroconnections de MIT Media Lab: <https://spendview.media.mit.edu>

![](/wp-content/uploads/2016/11/spendview.png)

DataUSA
-------

Plataforma de visualización de datos públicos publicados por el gobierno de EE.UU: <http://datausa.io>

(¡Ayer ganó medalla de oro en los [premios Kantar Information is Beautiful](http://www.informationisbeautifulawards.com/news/188-2016-the-winners)!)

![](/wp-content/uploads/2016/11/datausa.png)

DataChile
---------

Actualmente estamos desarrollando un proyecto similar a DataUSA para información generada por el Gobierno de Chile.

![](/wp-content/uploads/2016/11/datachile.png)

Observatory of Economic Complexity
----------------------------------

Visualización de 60 años de datos de comercio internacional: <http://atlas.media.mit.edu>

![](/wp-content/uploads/2016/11/atlas_colombia.png)

Data Viva
---------

Plataforma de visualización de indicadores socioeconómicos de Brasil: <http://legacy.dataviva.info>

![](/wp-content/uploads/2016/11/dataviva.png)

Conclusión
==========

Es posible construir recursos de información útiles. Pero, en mi opinión, es crucial trabajar en la *experiencia* de uso de esos sistemas. Los servicios masivos de Internet nos acostumbraron a interactuar con sistemas fluídos, y siempre disponibles. Esto aumenta las expectativas de los usuarios que usarán nuestros sistemas que —por supuesto— no cuentan con los recursos de los que disponen las grandes empresas de tecnología . Si implementamos una función de búsqueda en nuestro sitio, debe funcionar bien. Nuestro sistema debe poder accederse más o menos bien desde un dispositivo móvil [2].

Entonces, ¿cómo hace un departamento de tecnología de un gobierno para encarar esos desafíos técnicos y políticos?

Podemos encontrar una posible respuesta a esta pregunta en uno de los principios del así llamado "Gobierno Abierto": **colaboración**. Sin diálogo y participación de las comunidades de desarrolladores de software, periodistas, académicos, miembros de la sociedad civil y de sector privado, es poco probable que una oficina pública pueda llevar a cabo una iniciativa exitosa de datos abiertos.

En Argentina, gracias al empuje de organizaciones como *[Hacks-Hackers](https://www.meetup.com/HacksHackersBA/)*, [*La Nación Data*](http://www.lanacion.com.ar/data) y muchos otros, se establecieron puentes de diálogo y colaboración con el sector público. Por supuesto, habrá tensión: en última instancia estamos haciendo política y es natural que no estemos todos tomados de la mano cantando canciones de amor. Los periodistas, por ejemplo, están naturalmente en oposición a los gobiernos (de otra manera estarían haciendo propaganda). Las organizaciones de la sociedad civil también: existen para llenar un espacio que no ocupa el estado. Aún así, es posible establecer diálogos productivos y maduros.

Desarrollar un software, un portal de datos o una visualización es relativamente fácil. Al fin de cuentas, no estamos resolviendo problemas técnicos demasiado desafiantes: las bases de datos tal como las conocemos hoy existen hace más de 40 años. La comunidad de software libre produce una cantidad ENORME de herramientas que podemos usar gratuitamente [3]. Armar software no es el problema principal. Pero cambiar una cultura de gobierno es un proceso largo y tortuoso: en Argentina costó muchos años lograr una Ley Nacional de Acceso a la Información Pública.

Entonces, los desafíos a los que nos enfrentamos son mucho más viejos que las computadoras, los gráficos estadísticos y los datos abiertos. Son retos políticos, de planeamiento y de gestión. Los *hackatones* y las conferencias son una gran oportunidad que tenemos las comunidades que estamos interesadas en estos temas para conocernos y para compartir conocimiento y experiencias. Pero las iniciativas de datos abiertos e información pública no deben terminar ahí. Para lograr impacto real e "innovación" (ese concepto tan de moda pero que no sabemos realmente qué significa), necesitamos pensar en el largo plazo y atraer a profesionales competentes [4].

Honestamente, no se cómo se construye un ecosistema de innovación vibrante y productivo [5]. Pero he sido partícipe de algunos. En todos, he visto apertura, diálogo, financiamiento, incentivos para todos sus participantes e impacto en el mundo real. Creo que el Estado, como garante de derechos y oportunidades para todos, está en una posición inmejorable para generar esos espacios.

¡Muchas gracias!

Footnotes
=========

[1] ["Why are we doing fewer interactives"](https://raw.githubusercontent.com/archietse/malofiej-2016/master/tse-malofiej-2016-slides.pdf), presentación en Malofiej 2016 de Archie Tse, Deputy Graphics Director en el New York Times

[2] El tráfico desde móviles sigue aumentando y es una tendencia que no va a cambiar.

[3] De hecho, hay tantas opciones que es difícil elegir bien. Mi consejo: no elijas lo que está de moda, sino lo que tus programadores sepan usar.

[4] La demanda de profesionales TIC capacitados presenta un problema interesante para el sector público. "It's a seller's market" dicen los gringos 😀

[5] Hay mucha gente que dice que sabe cómo.

---
layout: post
title: "UnaBanda.org: difundiendo música en vivo"
date: 2021-02-18T18:43:51.344Z
author: manuel
---

En los últimos meses de 2019 —cuando éramos felices y no lo sabíamos—, me encontraba entre dos trabajos. Había dejado mi puesto en [Airtable](https://airtable.com) y me estaba preparando para comenzar en [Coefficient](https://coefficient.io), la empresa para la que trabajo actualmente como ingeniero de software. La peste todavía no estaba entre nosotros y la escena de música en vivo en Bahía Blanca pasaba por un momento de bastante actividad. Como bajista estable de la [Orquesta Congo Bongo](https://www.youtube.com/watch?v=HsU64wzYlWs) y el trío [Metatron](https://www.youtube.com/watch?v=GjUDaoh2Irk), yo salía a tocar al menos una vez por semana; una frecuencia bastante alta para un músico amateur en una ciudad chica como Bahía Blanca.

En esa misma época descubrí en Facebook el laburo de Fabio, que publicaba todos los días en su muro una agenda minuciosa de todos los eventos de música en vivo de la ciudad y zona. Como tenía tiempo [^1], me puse en contacto con el y le propuse aunar esfuerzos para sistematizar su laburo e intentar llegar a más personas. 

## Primer intento

Construí la primer versión de la agenda usando [GatsbyJS](https://www.gatsbyjs.com/), un generador de sitios web que en ese entonces gozaba de cierta popularidad. Estuvo online unos días y me di cuenta que no funcionaba para una herramienta de este tipo. Por otro lado, me resultó un horror como sistema: complejo, lento, inestable.


## Segundo intento

Seguía teniendo tiempo, así que decidí empezar de vuelta. Esta vez, elegí hacer un sitio con una arquitectura convencional: un frontend implementando con [Next.JS](https://nextjs.org/) y un backend hecho en Python que expone los datos mediante [GraphQL](https://graphql.org).

Para el _data entry_ de los eventos construí un modelo de datos muy simple [^2] en Airtable. El backend obtiene los datos periódicamente y los almacena en su base de datos local.

Esta versión se hizo pública a mediados de octubre de 2019. La cosa empezó a tener un crecimiento de tráfico considerable, como se ve en el gráfico. La subida que empieza en Diciembre corresponde con la decisión de haber hecho una [Facebook Page](https://www.facebook.com/unabandaorg), en la que todos los días se publica un posteo que contiene la agenda del día. Esto sucede mediante [Zapier][https://zapier.com/], porque usar directamente las APIs de Facebook es exageradamente difícil.

![Grafico de usarios activos desde Octubre de 2019 hasta Marzo de 2020](/wp-content/uploads/unabanda-users.png "Veniamos bien y aparecio COVID")

## La Gran Cuarentena de 2020

El gráfico anterior es una gran foto del año pasado. A partir de la segunda semana de marzo, la música en vivo desapareció y se desplomó el tráfico a [unabanda.org](https://unabanda.org). No tenía sentido seguir trabajando en el sitio, así que pusimos un cartelito y desensillamamos hasta que aclarara.

## Tercer nacimiento

Con el fin del invierno 2020, la disminución de casos de COVID-19 y el hartazgo generalizado, volvió timidamente la música en vivo y el incansable Fabio volvió a la carga en su muro de Facebook. Yo tenía ganas de desempolvar a unabanda.org y aproveché para hacer algunos cambios en el sistema.

El frontend quedó igual, pero decidí sacarme de encima el backend que había hecho en Python. Como ya soy programador jovato, quiero escribir la menor cantidad de software posible. En otras palabras, maximizar la funcionalidad minimizando la cantidad de software de la que tengo que hacerme cargo.

Decidí usar [Datasette](https://datasette.io), un proyecto libre y abierto al que le venía siguiendo el rastro hace rato. [Simon Willison](https://simonwillison.net/) [^3], su autor, lo define como _una multi-herramienta abierta para publicar y explorar datos_. 

Datasette provee herramientas para acceder a información almacenada en bases de datos [SQLite](https://sqlite.org), una herramienta de software que todos usamos aunque no lo sepamos [^4]. Simon, además, está construyendo un _ecosistema_ de herramientas alrededor de Datasette. En particular, un _plugin_ que habilita una interfaz GraphQL. Con esto, tenía todas las partes necesarias para reemplazar mi backend con una instancia de Datasette.

Como no hay intenciones comerciales detrás de unabanda.org, el objetivo es usar todos los canales disponibles para difundir la información que compila Fabio. Estamos probando, a partir de este tercer lanzamiento, un [canal de Telegram](https://t.me/unabanda) en el que todos los días se publica la agenda.

El _frontend_ de unabanda.org es abierto, publicado bajo la licencia MIT, y su código fuente está disponible en GitHub: [https://github.com/jazzido/unabanda.org](https://github.com/jazzido/unabanda.org). El backend (es decir, la instancia de Datasette) también es de libre acceso y está disponible en [https://datasette.unabanda.org](https://datasette.unabanda.org).

## Necesitamos ayuda.

Hacemos unabanda.org porque tenemos ganas de que los recitales exploten de gente. Y para eso, necesitamos de tu ayuda:

  - Difundiendo el proyecto
  - Dándole like a la página en FB: [https://facebook.com/unabanda.org](https://facebook.com/unabanda.org)
  - Suscribiéndote al canal de Telegram: [https://t.me/unabanda](https://t.me/unabanda)

Si sos programador/programadora o diseñador/diseñadora, también nos sirve tu ayuda:

  - Mejorando el diseño del sitio: lo hice yo, que no soy diseñador. Le pongo la mejor de las ondas, pero está bastante fiero.
  - Mejorando el código: no pretendo demasiadas funcionalidad adicionales, pero si se te ocurre algo ponete en contacto conmigo o mandame un _pull request_.


[^1]: "Tener tiempo" a los 40 años era una oportunidad única, había que aprovecharla.
[^2]: Eventos, Lugares y Ciudades.
[^3]: Prócer indiscutible de las tecnologías abiertas de programación web. Entre otras cosas, es uno de los co-autores de [Django](https://www.djangoproject.com/), legendario _framework_ para construcción de sitios web.
[^4] Es parte fundamental de los sistemas operativos Android y iOS.

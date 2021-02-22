---
layout: blog
title: "UnaBanda.org: difundiendo música en vivo"
date: 2021-02-18T18:43:51.344Z
---
En los últimos meses de 2019 —cuando éramos felices y no lo sabíamos—, me encontraba entre dos trabajos. Había dejado mi puesto en [Airtable](https://airtable.com) y me estaba preparando para comenzar en [Coefficient](https://coefficient.io), la empresa para la que trabajo actualmente como ingeniero de software. La peste todavía no estaba entre nosotros y la música en vivo en Bahía Blanca gozaba de un buen momento. Como bajista estable de la [Orquesta Congo Bongo](https://www.youtube.com/watch?v=HsU64wzYlWs) y el trío [Metatron](https://www.youtube.com/watch?v=GjUDaoh2Irk), yo salía a tocar al menos una vez por semana: una frecuencia bastante alta para un músico semi-amateur en una ciudad chica como Bahía Blanca.

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



[^1]: "Tener tiempo" a los 40 años era una oportunidad única, había que aprovecharla.
[^2]: Eventos, Lugares y Ciudades.

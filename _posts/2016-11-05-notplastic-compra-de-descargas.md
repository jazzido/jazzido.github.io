---
title: Notplastic — intentando vender archivos
author: manuel
layout: post
permalink: /2016/11/05/notplastic-compra-de-descargas
categories:
  - Uncategorized
---

Hace unos años, cuando los músicos todavía tenían esperanzas de poder cobrar por su música grabada, mi amigo [Milton Amadeo](https://miltonamadeo.bandcamp.com/) había grabado su disco [Como dos Barcos](https://miltonamadeo.bandcamp.com/album/como-dos-barcos) y quería venderlo en los shows en vivo. Como no tenía sentido hacer una tirada de CDs (además de ser carísimos, ¿quién tiene una compactera a esta altura?), se nos ocurrió intentar vender *códigos de descarga*. Es decir, una tarjetita con un número que los compradores podían ingresar en un sitio web para bajar el disco en formato MP3. Programé una aplicación web muy simple para hacer eso, se vendieron varios códigos de descarga, todos contentos.

A otros amigos músicos les entusiasmó el sistema, pero querían que los *downloads* se puedan pagar con tarjeta, además de los códigos impresos. Pero el sistema que había hecho para el disco de Milton era una porquería hecha en una tarde, y no había manera de agregarle *features*.

En las semanas de desempleo que tuve justo antes de [mudarme a EE.UU. para estudiar](/2015/10/01/my-media-lab-statement-of-objectives/), para sacarme las ganas de programar, rehice la aplicación original y le agregué algunos *features*:

  - Integración con [Mercado Pago](http://mercadopago.com), a través de su sistema de notificación (parecido al [IPN de PayPal](https://developer.paypal.com/docs/classic/products/instant-payment-notification/).
  - Precio mínimo y precio sugerido, y la posibilidad de pagar más, como donación a la causa.
  - *Multi-tenancy*: el sistema original funcionaba con un proyecto solo, ahora podía vender más de uno.

Luego de un par de años de estar en línea, y de haber publicado algunos proyectos de amigos y familia como [El Abrigo del Viento](http://s.unabanda.cc/p/el-abrigo-del-viento) —película documental producida por mi esposa [Luisina Pozzo Ardizzi](http://lupaproductora.com/)— o el disco [LAS de Supernova Jazz Trío](http://s.unabanda.cc/p/supernova-jazz-trio-las), aprendí algunas cosas de este experimento:

  - Argentina es un país muy poco bancarizado, en especial los miembros del *target* de proyectos artísticos independientes. La idea de usar Mercado Pago era aprovechar los medios de pago diferidos como Pago Fácil. La lección es que no funcionan para este tipo de sistemas. Salvo un único caso, las 22 transacciones iniciadas con un cupón *nunca* fueron completadas. Es decir, los usuarios comenzaban el proceso, y nunca iban al Farmacity a pagar el cupón. El medio de pago más usado fue, por lejos, las tarjetas de crédito.
  - La enorme mayoría de los que compraron algo, pagaron el mínimo, pese a ser precios irrisorios (más barato que un kilo de milanesas, o una coca grande).
  - A pesar de la complejidad de su API, y lo deficiente de su documentación, Mercado Pago funciona bastante bien.

Este proyecto nunca tuvo desarrollo comercial, ni de producto (así lo demuestra su horrible diseño gráfico). Pero esta breve y superficial experiencia me sugiere que este modelo no funciona. Ya no bajamos música, ni ningún tipo de *bien digital*: los consumimos online, en plataformas como YouTube. 

De todas maneras, fue un experimento interesante, y me mantuvo entretenido los días previos a mudarnos a EE.UU.

El código fuente de notplastic está disponible bajo licencia AGPL3 en [http://github.com/jazzido/notplastic](http://github.com/jazzido/notplastic)

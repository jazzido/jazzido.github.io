---
title: Arreglando el sitio de AFIP
author: manuel
layout: post
permalink: /2012/02/06/arreglando-el-sitio-de-afip/
dsq_thread_id:
  - 566951643
categories:
  - Uncategorized
---
La <a title="Administración Federal de Ingresos Públicos" href="http://www.afip.gov.ar" target="_blank">Administración Federal de Ingresos Públicos</a> es frecuentemente citada como uno de los mejores ejemplos de uso de tecnología en el estado. Pero parece que andan flojos de *web developers* actualizados, porque hace años que la función de imprimir constancia de monotributo (que debe ser uno de los servicios más requeridos de todo el sitio) funciona solamente en Internet Explorer.

<a href="https://gist.github.com/raw/1756266/b7220fdfb427ecba7ec3209f0a7fb007f72edb92/afip.user.js" target="_blank">Este userscript para Firefox+Greasemonkey o Chrome soluciona el problemita</a>.

En caso que algún programador voluntarioso de AFIP esté leyendo esto y tenga ganas de arreglarlo: no anda porque usan `window.navigate.`. Agreguen esto en algún lado y todos contentos:

{% highlight javascript %}
window.navigate = function(u) { window.location.href = u };
{% endhighlight %}

Es *una línea de código*, AFIP. Media pila.

**Actualización** [Felipe Lerena][1] acaba de publicar una extensión para Firefox que usa este código: [Modernizador de la AFIP][2]

 [1]: https://twitter.com/#!/felipelerena
 [2]: https://addons.mozilla.org/en-US/firefox/addon/modernizador-de-la-afip/

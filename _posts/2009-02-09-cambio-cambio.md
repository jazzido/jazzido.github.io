---
title: Cambio, Cambio.
author: manuel
layout: post
permalink: /2009/02/09/cambio-cambio/
dsq_thread_id:
  - 68371945
categories:
  - Uncategorized
tags:
  - dolar
  - google
  - tipo de cambio
---
Reactivar este *güeblog* con esta clase de post después de más de medio año sin actualizarlo es, al menos, deprimente. Pero hoy empecé mis vacaciones; estoy contento y generoso, entonces publico esto que le puede servir a más de uno.

Estaba haciendo unas cuentas domésticas en un [Google Spreadsheet][1] y necesitaba saber la cotización del dólar. Los verdes se están yendo para arriba como flato de hombre rana, así que necesitaba alguna manera de mantener ese valor actualizado en mi planilla. Así de ordenado soy.

El [web service][2] de [Mi dolar][3] nos va servir para esto.

Entonces, estas son las fórmulas:

**Precio dólar comprador**: `=ImportXML("http://www.midolar.com.ar/dolar.xml", "/MIDOLAR/VALORCOMPRA[1]")/1000`

**Precio dólar vendedor**: `=ImportXML("http://www.midolar.com.ar/dolar.xml", "/MIDOLAR/VALORVENTA[1]")/1000`

 [1]: http://docs.google.com/
 [2]: http://www.midolar.com.ar/dolar.xml
 [3]: http://www.midolar.com.ar
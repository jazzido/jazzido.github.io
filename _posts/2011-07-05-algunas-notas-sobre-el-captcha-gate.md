---
title: Algunas notas sobre el Captcha-gate
author: manuel
layout: post
permalink: /2011/07/05/algunas-notas-sobre-el-captcha-gate/
dsq_thread_id:
  - 350718051
categories:
  - Uncategorized
tags:
  - datos abiertos
  - opendata
---
[Hace un año][1] hice público a [Gasto Público Bahiense][2]. El objetivo del proyecto es brindar *otra* manera de explorar los datos de compras a proveedores que publica la [Municipalidad de Bahía Blanca][3]. El día 4 de julio de 2011 la Municipalidad lanzó el rediseño de su site, introduciendo una restricción al acceso a los datos de compras, implementada mediante un [CAPTCHA][4]. El CAPTCHA es un mecanismo usado por los sistemas online para diferenciar a sus usuarios entre seres humanos y robots. Plantean un problema fácil de resolver para nosotros (tipear un texto que aparece deformado es el más frecuente), pero muy difícil o imposible para nuestros amigos los robots. [Google Bot][5] es el agente automático que usa Google para recorrer la Web e indexar su contenido. Más humildemente, Gasto Público Bahiense cuenta con un robot que periódicamente extraía los datos del sitio del gobierno municipal bahiense y los almacenaba en una base de datos para luego mostrarlos a través de una interfaz más clara y fácil de usar que la que provee el municipio. Desde ayer, la Municipalidad obliga a los usuarios de su site a resolver un CAPTCHA para acceder a las órdenes de compra publicadas, en efecto impidiendo el acceso del robot de GPB a los datos de los que se alimenta.

### Una pequeña crónica de los hechos

Me enteré de este rediseño ayer por la noche y ví que el acceso a la información de la sección [&#8220;Transparencia&#8221;][6] estaba protegida por un CAPTCHA. Me sorprendí: estos mecanismos no se utilizan en general para impedirle a un robot la *lectura* de información. Es más frecuente que estén ahí para impedir que, por ejemplo, un agente automático registre en un minuto mil cuentas de email gratuitas y luego las use para enviar spam. A raíz de estos cambios, el robot original de GPB quedó obsoleto luego de un año de fiel servicio. Construir (programar) uno nuevo lleva unos días de trabajo; tiempo muerto en el que GPB no va a actualizar la información. Informé acerca de esto en un anuncio que puse en el encabezado del sitio, [lo publiqué en Twitter][7] y pedí difusión. En el transcurso de la noche, muchos se hicieron eco del anuncio y propagaron la noticia. La difusión y discusión continuaron en Twitter durante todo el día de hoy. Eventualmente, la noticia fue levantada por [el medio bahiense SoloLocal.info][8] y [un rato más tarde por La Nueva Provincia][9], el diario local. Esta última nota incluye la grabación de una entrevista hecha por la radio local LU2 a Ramiro Villalba, actual secretario de hacienda de Bahía Blanca, responsable de la publicación en la web de los datos de compras municipales. Voy a responder a algunos de los conceptos vertidos por Villalba, empezando por lo bueno.

#### *&#8216;Ahora publicamos mucha más información que antes&#8217;*

Es cierto y me alegra muchísimo. Ahora hay información de [compras devengadas][10], un [listado de proveedores][11] actualizado, información más detallada sobre las operaciones. Con respecto a lo anterior, progresó mucho. Intuyo que la existencia de GPB influyó en estas decisiones.

#### *&#8220;La municipalidad no esconde nada, los datos pueden ser vistos por cualquiera&#8221;*

Cierto. Lo que critico es que se obstruyó adrede el acopio automático de los datos. La información (de toda clase) nos inunda: necesitamos ayuda para poder interpretarla. ¿Qué mejor que un robot que trabaje por nosotros?. Hacen muy rápido y a costo nulo, el trabajo que a un ser humano le llevaría días. Imposible, se les negó el acceso.

#### *&#8220;se hizo para evitar a los robots que hacen más lento el sistema&#8221;*

Existen maneras mucho más apropiadas de optimizar el consumo de recursos de un servidor ante la actividad intensa que  *podría* ocasionar la visita de un robot. Para sitios de &#8220;sólo lectura&#8221;, como el caso de los datos publicados por la MBB, la solución a este potencial problema es muy sencilla. Un enorme porcentaje de los servidores conectados a la Web reciben las visitas simultáneas de decenas de robots sin que ocasionen problema alguno. En particular, el robot de GPB efectuaba alrededor de *300 pedidos cada 3 días*, cifra insignificante incluso para un sitio de bajo tráfico.

#### *&#8220;si Gasto Público Bahiense publica la información, es porque la Municipalidad la provee&#8221;*

Poco que decir al respecto, es cierto. La [ordenanza Nº 11.162][12] así lo requiere.

#### *&#8216;Los robots vulneran la seguridad de la información&#8217;*

Alguien que &#8220;vulnera la seguridad de la información&#8221; almacenada en un sistema, está comprometiendo la integridad de los datos. No veo de qué manera **la mera lectura de datos públicos** está poniendo en peligro la seguridad del sistema de la Municipalidad. Vale aclarar que el robot de GPB no usa trucos ni subterfugios: hace lo que haría una persona que navega el sitio, pero varias veces más rápido.

#### *&#8220;A fin del año pasado intentamos algun contacto con [el responsable de GPB], pero vive en Bariloche y no pudimos concretar la reunión&#8221;*

Lamento que la discusión se desvíe hacia los detalles, pero Villalba no es preciso en su relato. Hubo dos intentos de coordinar encuentros, en octubre del año pasado y en marzo de este año. En octubre todavía vivía en Bahía Blanca, expresaron interés a través de un intermediario y hasta ahí llegamos, no supe más nada. En Marzo, cuando nuevamente manifestaron su interés de reunirse conmigo, yo ya estaba instalado en Bariloche. Propuse una reunión vía Skype, pero no se concretó porque preferían verme en persona. Luego, el mismo intermediario me dijo que Villalba iba a contactarme por email, todavía estoy esperando.

### Redondeando

Confío en que la MBB revisará la decisión de restringir el acceso a medios automáticos: así como GPB está transitoriamente imposibilitado de acceder, se prohibió permanentemente la entrada a los robots de los buscadores de propósito general (Google, Yahoo, Bing, etc). Tratándose de información pública, es prioritario **facilitar** en lugar de entorpecer el acceso.

 [1]: http://blog.jazzido.com/2010/07/18/un-experimento-sobre-tecnologia-y-transparencia-gubernamental/
 [2]: http://gastopublicobahiense.org
 [3]: http://www.bahiablanca.gov.ar
 [4]: http://es.wikipedia.org/wiki/Captcha
 [5]: http://es.wikipedia.org/wiki/Googlebot
 [6]: http://www.bahiablanca.gov.ar/transparencia/index.php
 [7]: http://twitter.com/#!/manuelaristaran/status/88052010099802114
 [8]: http://www.sololocal.info/noticias/1-de-bahia/2845-nueva-web-municipal-impide-actualizar-gasto-publico-bahiense-.html
 [9]: http://www.lanueva.com/laciudad/nota/e81bb23acd/1/102211.html
 [10]: http://www.bahiablanca.gov.ar/compras/devengado.aspx
 [11]: http://www.bahiablanca.gov.ar/compras/proveedores.aspx
 [12]: http://webcache.googleusercontent.com/search?q=cache:szZS_CP_QZAJ:mbamac.bahiablanca.gov.ar/digesto4/Ordenanza.html%3Ford%3D11162+mendoza+bahia+blanca+%22control+activo+%22&cd=1&hl=es-419&ct=clnk&gl=ar&source=www.google.com.ar

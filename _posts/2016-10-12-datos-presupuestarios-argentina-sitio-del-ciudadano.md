---
title: Extrayendo datos reutilizables del Sitio del Ciudadano
author: manuel
layout: post
permalink: /2016/10/12/datos-presupuestarios-argentina-sitio-del-ciudadano.md
categories:
  - Uncategorized
---

<style type="text/css">
  .gist-file
  .gist-data {max-height: 300px;}
</style>

Los presupuestos públicos son conjuntos de datos multidimensionlales, que reflejan
las estructuras burocráticas, contables y económicas de los gastos e
ingresos de la administración del estado. Su complejidad presenta
desafíos interesantes a la hora de construir herramientas que permitan
explorarlos.

Aunque los presupuestos de cualquier nivel del estado son muy
similares en estructura, casi todos los países publican en la web sus presupuestos
a través de herramientas *ad-hoc*. Este panorama fue
relevado en detalle por [Jonathan Stray](http://jonathanstray.com) en
el reporte
[Open Budget Data: Mapping the Landscape](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2654878). La
fundación [Open Knowledge International](https://okfn.org/), por su
parte, impulsa el proyecto
[Fiscal Data Package](http://specs.frictionlessdata.io/fiscal-data-package/),
que aprovecha esa similaridad estructural para estandarizar el formato
en que se publican los datos públicos fiscales.

Argentina hace su parte desde hace algunos años con el
[Sitio del Ciudadano](http://sitiodelciudadano.mecon.gov.ar),
dependiente del
[Ministerio de Hacienda y Finanzas Públicas](http://www.mecon.gov.ar). Pese
a su título, nos cuesta imaginar a un ciudadano común navegando con
éxito esta herramienta, debido a su mal diseño, complejidad y bajísima
performance.

Durante mi tesis de maestría trabajé en
[SpendView](https://spendview.media.mit.edu/), un
prototipo de herrramienta para visualizar información
presupuestaria. Uno de las premisas de su diseño es ser lo
suficientemente flexible para almacenar y permitir explorar
cualquier presupuesto público. Naturalmente, me interesaba mostrar [el
presupuesto argentino](https://spendview.media.mit.edu/cube/argentina). *SpendView*
requiere que los datos estén representados de manera desagregada. Es
decir, cada *línea* del presupuesto debe contener información sobre
todas las dimensiones en que se clasifica. En el caso del presupuesto asignado al
[presupuesto del CONICET](https://spendview.media.mit.edu/cube/argentina/Institucional/Ministerio%20de%20Ciencia%2C%20Tecnolog%C3%ADa%20e%20Innovaci%C3%B3n%20Productiva/Ministerio%20de%20Ciencia%2C%20Tecnolog%C3%ADa%20e%20Innovaci%C3%B3n%20Productiva/Consejo%20Nacional%20de%20Investigaciones%20Cient%C3%ADficas%20y%20T%C3%A9cnicas),
tomamos una línea del presupuesto bastante desagregada (actualizada a marzo de 2016):

  - Clasificación Administrativa (*¿quién gasta?*)
    - [Ministerio de Ciencia, Tecnología e Innovación Productiva](https://spendview.media.mit.edu/cube/argentina/Institucional/Ministerio%20de%20Ciencia%2C%20Tecnolog%C3%ADa%20e%20Innovaci%C3%B3n%20Productiva) (nivel Jurisdicción)
    - [Ministerio de Ciencia, Tecnología e Innovación Productiva](https://spendview.media.mit.edu/cube/argentina/Institucional/Ministerio%20de%20Ciencia%2C%20Tecnolog%C3%ADa%20e%20Innovaci%C3%B3n%20Productiva/Ministerio%20de%20Ciencia%2C%20Tecnolog%C3%ADa%20e%20Innovaci%C3%B3n%20Productiva) (nivel Subjurisdicción)
    - [CONICET](https://spendview.media.mit.edu/cube/argentina/Institucional/Ministerio%20de%20Ciencia%2C%20Tecnolog%C3%ADa%20e%20Innovaci%C3%B3n%20Productiva/Ministerio%20de%20Ciencia%2C%20Tecnolog%C3%ADa%20e%20Innovaci%C3%B3n%20Productiva/Consejo%20Nacional%20de%20Investigaciones%20Cient%C3%ADficas%20y%20T%C3%A9cnicas) (nivel Entidad)
  - Clasificación según Objeto del Gasto (*¿en qué se gasta?*)
    - Gastos en Personal (nivel Inciso)
    - Personal Permanente (nivel Partida Principal)
  - Clasificación Funcional (*¿para qué se gasta?*)
    - Servicios Sociales (nivel Finalidad)
    - Ciencia y Técnica (nivel Función)
  - Clasificación según Fuente de financiamiento
    - Tesoro Nacional
  - Medidas
    - Crédito Vigente: 4.65 miles de millones de pesos argentinos
    - Devengado: 1.21 miles de millones de pesos argentinos.

Desafortunadamente, el Sitio del Ciudadano sólo ofrece cuadros pre-agregados. Es decir, no es posible obtener líneas completas del presupuesto, que refieran a todos los criterios de clasificación. Pero la data está, y se puede extraer.

## Hurgando en el Sitio del Ciudadano (SiCi)

El SiCi está implementado en una versión antigua de [Oracle Business Intelligence](https://www.oracle.com/solutions/business-analytics/business-intelligence/index.html), un sistema muy poco apto para construir sitios web públicos. Por ejemplo, para obtener una tabla de gastos por jurisdicción, el browser hace 388 pedidos al servidor (!), transfiere 4.2 MB y (en mi computadora y con mi conexión a internet) tarda 27 segundos en mostrar el contenido.

![Lentísimo](/wp-content/uploads/2016/10/sici_lento.png)


Pero entremezclada en su verborragia, el SiCi emite información que nos permitirá obtener los datos que necesitamos. El primer indicio es un request que ocurre cuando se interactúa (*hover*, *click*, etc) sobre una tabla. El browser pide un recurso llamado `/saw.dll?getReportXmlFromSearchID`, que contiene la definición del reporte solicitado:

<script src="https://gist.github.com/jazzido/f933d4686742e34d843e7ce50825bea8.js"></script>

Confundidos dentro de semejante aberración, hay elementos interesantes que contienen las fórmulas para cada columna del reporte:

``` xml
<saw:columnFormula>
  <sawx:expr xsi:type="sawx:sqlExpression">Institucion."Cod. y Desc. Jurisdiccion"</sawx:expr>
</saw:columnFormula>
<!-- ... -->
<saw:columnFormula>
  <sawx:expr xsi:type="sawx:sqlExpression">CAST(Tiempo.Mes as VARCHAR(2))</sawx:expr>
</saw:columnFormula>
<!-- ... -->
<saw:columnFormula>
  <sawx:expr xsi:type="sawx:sqlExpression">"Indicadores Credito"."$ Cred. Vigente"</sawx:expr>
</saw:columnFormula>
```

Como uno de los elementos principales, al principio del archivo, aparece `<saw:criteria subjectArea="&quot;SITIO DEL CIUDADANO&quot;" >`, que no es otra cosa que la tabla/cubo sobre la que opera el reporte.

Luego de un exhaustivo proceso de investigación (busqué en Google), vi que el recurso HTTP `/saw.dll` es el punto de entrada a casi todas las operaciones que ofrece este sistema de *business intelligence*. Grande fue mi felicidad cuando vi en la documentación que [`saw.dll` acepta un parámetro llamado `SQL`](https://docs.oracle.com/cd/E14571_01/bi.1111/e16364/apiwebintegrate.htm#BIEIT357). Resulta que ese supuesto SQL es una extensión de Oracle, diseñada para hacer consultas OLAP (analíticas). En pocas palabras, es un SQL donde el `SUM()` sobre las medidas y el `GROUP BY` sobre las dimensiones están implícitos (me gustó la idea, ojalá hubiera una implementación open source).

Probamos el *endpoint* `saw.dll` con una consulta simple (averigüé los nombres de las columnas mirando la definición del reporte):

``` sql
SELECT "Ejercicio Presupuestario"."Cod. Ejercicio Presupuestario",
       "Sector Institucional"."Desc. Caracter",
       "Indicadores Credito"."$ Comprometido",
       "Indicadores Credito"."$ Devengado",
       "Indicadores Credito"."$ Pagado",
       "Indicadores Credito"."$ Cred. Vigente"
FROM "SITIO DEL CIUDADANO"
```

El URL completo es el siguiente (el nombre de usuario y el password están visibles en el HTML del SiCi)

```
http://sitiodelciudadano.mecon.gov.ar/analytics/saw.dll?Go
  &NQUser=usrsici_c
  &NQPassword=usrsici_c
  &SQL=SELECT%20%22Ejercicio%20Presupuestario%22.%22Cod.%20Ejercicio%20Presupuestario%22,%20%22Sector%20Institucional%22.%22Desc.%20Caracter%22,%20%22Indicadores%20Credito%22.%22$%20Comprometido%22,%20%22Indicadores%20Credito%22.%22$%20Devengado%22,%20%22Indicadores%20Credito%22.%22$%20Pagado%22,%20%22Indicadores%20Credito%22.%22$%20Cred.%20Vigente%22%20FROM%20%22SITIO%20DEL%20CIUDADANO%22
```
Boom. La historia de la ejecución presupuestaria desde 1998, desagregada por "Sector Institucional". Para mi sorpresa, funcionó a la perfección, dibujando una tabla que parece diseñada en 1999, y cuyo HTML parece escrito con Microsoft FrontPage '98:

![Consulta simple](/wp-content/uploads/2016/10/tabla_consulta_simple.png)

Pero queremos CSV, no una tabla horrible. Volvemos a la [documentación](https://docs.oracle.com/cd/E14571_01/bi.1111/e16364/apiwebintegrate.htm), y vemos un parámetro `Format`. Agregamos `Format=CSV` al URL, que nos devuelve un  hermoso archivo separado por comas.

Vamos a omitir muchos pasos intermedios, para pasar directamente al modelo terminado. La siguiente consulta obtiene la ejecución presupuestaria a la fecha en 2016, a una resolución más alta de la que podíamos esperar:

``` sql
SELECT "Ejercicio Presupuestario"."Cod. Ejercicio Presupuestario",
       "Sector Institucional"."Desc. Caracter",
       "Institucion"."Cod. Jurisdiccion",
       "Institucion"."Desc. Jurisdiccion",
       "Institucion"."Cod. Subjurisdiccion",
       "Institucion"."Desc. Subjurisdiccion",
       "Institucion"."Cod. Entidad",
       "Institucion"."Desc. Entidad",
       "Servicio"."Cod. Servicio",
       "Servicio"."Desc. Larga Servicio",
       "Apertura Programatica"."Cod. Programa",
       "Apertura Programatica"."Desc. Programa",
       "Finalidad Funcion"."Cod. Finalidad",
       "Finalidad Funcion"."Desc. Finalidad",
       "Finalidad Funcion"."Cod. Funcion",
       "Finalidad Funcion"."Desc. Funcion",
       "Objeto Gasto"."Cod. Inciso",
       "Objeto Gasto"."Desc. Inciso",
       "Objeto Gasto"."Cod. Principal",
       "Objeto Gasto"."Desc. Principal",
       "Objeto Gasto"."Cod. Parcial",
       "Objeto Gasto"."Desc. Parcial",
       "Objeto Gasto"."Cod. Subparcial",
       "Objeto Gasto"."Desc. Subparcial",
       "Clasificador Economico"."Cod. 2 Digitos",
       "Clasificador Economico"."Desc. 2 Digitos",
       "Clasificador Economico"."Cod. 3 Digitos",
       "Clasificador Economico"."Desc. 3 Digitos",
       "Fuente Financiamiento"."Cod Codigos",
       "Fuente Financiamiento"."Cod y Desc Codigos",
       "Indicadores Credito"."$ Comprometido",
       "Indicadores Credito"."$ Devengado",
       "Indicadores Credito"."$ Pagado",
       "Indicadores Credito"."$ Cred. Vigente"
FROM "SITIO DEL CIUDADANO"
WHERE "Ejercicio Presupuestario"."Cod. Ejercicio Presupuestario"=2016
  AND ("Objeto Gasto"."Cod. Inciso" BETWEEN 1 AND 8)
  AND "Clasificador Economico"."Cod. 2 Digitos" IN (21, 22)
```

(Los filtros que aparecen en la cláusula `WHERE` fueron tomados de los reportes)

La tabla/cubo `SITIO DEL CIUDADANO` contiene más dimensiones, que nos permitirían obtener la ejecución presupuestaria a una resolución de **días**, para cualquier fecha desde 1998.

Obtener esa información, y construir reportes o herramientas interesantes, queda como ejercicio para el lector entusiasta.

---
title: Un experimento sobre tecnología y transparencia gubernamental
author: manuel
layout: post
permalink: /2010/07/18/un-experimento-sobre-tecnologia-y-transparencia-gubernamental/
dsq_thread_id:
  - 119510834
categories:
  - Uncategorized
tags:
  - bahía blanca
  - gasto público
  - gobierno 2.0
  - transparencia
---
La *transparencia gubernamental* es un tema de moda. Fue uno de los pilares de la innovadora campaña electoral de Barack Obama, donde la tecnología jugó un papel central. Una de las formas en las que Obama materializó estas promesas de transparencia es el proyecto [data.gov][1], donde se brinda acceso a más de 40.000 conjuntos de datos &#8220;crudos&#8221; generados por agencias del gobierno federal. Hay [estadísticas de accidentes aéreos][2] o [la lista de quienes visitaron la Casa Blanca][3] en los últimos meses. Lo destacable de esta iniciativa, que está siendo [implementada por otros países][4], es que los datos se publican en formatos *legibles por computadoras*. Esto es, la información puede ser usada en herramientas de análisis de datos (una planilla de cálculo, por ejemplo) o usados para construir *mashups*, aplicaciones que combinan varias fuentes de información. Un buen ejemplo es [OilMoney][5], que muestra los aportes monetarios que hacen las compañías petroleras a las campañas electorales. En Argentina, el panorama es bastante desolador. [No hay ley de acceso a la información pública][6]; apenas tenemos [el decreto 1172/03][7] que tiene bastantes problemas en su aplicación (ver [acá][8] o [acá][9]). Algunos organismos que *sí* publican información en la web, lo hacen en formatos ilegibles para las máquinas. Una imagen que contiene un documento impreso escaneado, como las [declaraciones juradas de los funcionarios municipales de Bahía Blanca][10], es invisible para los buscadores. Esto también es un problema para la transparencia: en esta época de sobreabundancia de información, en la que los filtros son imprescindibles para encontrar lo que se busca, los documentos deben ser publicados en formatos apropiados. Por suerte, están apareciendo iniciativas particulares en la dirección correcta. La ONG [Poder Ciudadano][11] desarrolló dos herramientas interesantes. Una [base de datos sobre las pautas publicitarias oficiales][12] y el sitio [Dinero y Política][13]. Son un recurso para facilitar la búsqueda y el acceso a datos provenientes de organismos estatales, que no se destacan por producir sitios web de calidad.

### Gasto Público Bahiense

La [Municipalidad de Bahía Blanca][14] publica en su sitio web [las órdenes de compra adjudicadas a proveedores][15]. La herramienta de consulta dista del ideal, pero afortunadamente los datos están publicados en una página web común y silvestre; tienen cierta *estructura* y por lo tanto, un programa de computadora podrá interpretarlos. La Municipalidad sólo publica los listados de las órdenes de compra: cuándo y a quién se compró tal o cual cosa y a qué repartición fue destinada la compra. Vista así, poco es el aporte de esa información. Entonces, motivado por el interés de probar algunas herramientas informáticas y por la curiosidad acerca de la transparencia y la tecnología, construí [Gasto Público Bahiense][16]. [Gasto Público Bahiense][16] facilita el acceso a los datos de compras publicados por la MBB. Permite consultar la lista completa de proveedores y de dependencias municipales, ver las órdenes de compra por repartición o por proveedor, obtener montos totales por período y buscar en el contenido de las órdenes de compra. Puede verse [el gasto de la Secretaría de Gobierno en el primer semestre de este año][17] y comparar con [el mismo período de la Dirección de Fortalecimiento Humano][18].

### ¿Y para qué?

Los gobernantes son responsables de informar sus acciones a los ciudadanos, al fin de cuentas están ahí gracias al voto popular. No pueden desaprovechar la oportunidad que ofrece la web para brindar esa información y mostrarla de manera concisa y clara. Muchas veces, el supuesto costo de este tipo de proyectos es argumento para no implementarlos. La única inversión para construir GPB fue *1 semana de mi tiempo*. Tampoco hubo costos de licencias de software; todos los componentes que usé para el desarrollo son [software libre][19] o de uso gratuito. A su vez, [GPB también es software libre][20].

Los datos son materia prima para la discusión. Con GPB, tengo la esperanza de que algún bahiense se alarme o se alegre por el dinero invertido en una cosa u otra y que se produzca la discusión que motive acciones transformadoras.

 [1]: http://data.gov
 [2]: http://www.data.gov/raw/1576
 [3]: http://www.socrata.com/Government/White-House-Visitor-Records-Requests/644b-gaut
 [4]: http://www.data.gov/internationalDataWebsites
 [5]: http://oilmoney.priceofoil.org/
 [6]: http://www.periodismo-aip.org/noticia-detalle.php?id=115
 [7]: http://www.mejordemocracia.gov.ar/paginas.dhtml?pagina=57
 [8]: http://www.clarin.com/opinion/Mora-acceso-informacion_0_298170247.html
 [9]: http://www.perfil.com/contenidos/2009/10/09/noticia_0017.html
 [10]: http://www.bahiablanca.gov.ar/autoridades/list_ddjj.html
 [11]: http://www.poderciudadano.org/
 [12]: http://poderciudadano.org/?p=79
 [13]: http://www.dineroypolitica.org/
 [14]: http://www.bahiablanca.gov.ar
 [15]: http://www.bahiablanca.gov.ar/compras4/comprasrV2.asp
 [16]: http://gpb.nerdpower.org
 [17]: http://gpb.nerdpower.org/reparticion/secretaria-de-gobierno/2010/01/2010/06/
 [18]: http://gpb.nerdpower.org/reparticion/dir-de-fortalecimiento-humano/2010/01/2010/06/
 [19]: http://es.wikipedia.org/wiki/Software_libre
 [20]: http://github.com/jazzido/GPB

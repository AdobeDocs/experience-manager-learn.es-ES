---
title: Guía de implementación de búsqueda simple
description: La implementación de búsqueda simple son los materiales del laboratorio de la Cumbre 2017 AEM Search Demystified. Esta página contiene los materiales de este laboratorio. Para una visita guiada al laboratorio, por favor vea el libro de Lab en la sección Presentación de esta página.
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
feature: 'Búsqueda  '
topic: Desarrollo
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '899'
ht-degree: 2%

---


# Guía de implementación de búsqueda simple{#simple-search-implementation-guide}

La implementación de búsqueda simple son los materiales del **laboratorio de Adobe Summit AEM Search Demystified**. Esta página contiene los materiales de este laboratorio. Para una visita guiada al laboratorio, por favor vea el libro de Lab en la sección Presentación de esta página.

![Información general sobre la arquitectura de búsqueda](assets/l4080/simple-search-application.png)

## Materiales de presentación {#bookmarks}

* [Libro de trabajo de Lab](assets/l4080/l4080-lab-workbook.pdf)
* [Presentación](assets/l4080/l4080-presentation.pdf)

## Marcadores {#bookmarks-1}

### Herramientas {#tools}

* [Administrador de índices](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Explicar la consulta](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene)  > /oak:index/cqPageLucene
* [Administrador de paquetes CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [Depurador de QueryBuilder](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Generador de definiciones de índices Oak](https://oakutils.appspot.com/generate/index)

### Capítulos {#chapters}

*Los vínculos de capítulo siguientes suponen que los  [paquetes ](#initialpackages) iniciales están instalados en AEM Author en`http://localhost:4502`*

* [Capítulo 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Capítulo 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Capítulo 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Capítulo 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Capítulo 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Capítulo 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Capítulo 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Capítulo 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Capítulo 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Paquetes {#packages}

### Paquetes iniciales {#initial-packages}

* [Etiquetas](assets/l4080/summit-tags.zip)
* [Paquete de aplicación de búsqueda simple](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Paquetes de capítulo {#chapter-packages}

* [Solución del capítulo 1](assets/l4080/l4080-chapter1.zip)
* [Solución del capítulo 2](assets/l4080/l4080-chapter2.zip)
* [Solución del capítulo 3](assets/l4080/l4080-chapter3.zip)
* [Solución del capítulo 4](assets/l4080/l4080-chapter4.zip)
* [Configuración del capítulo 5](assets/l4080/l4080-chapter5-setup.zip)
* [Solución del capítulo 5](assets/l4080/l4080-chapter5-solution.zip)
* [Solución del capítulo 6](assets/l4080/l4080-chapter6.zip)
* [Solución del capítulo 9](assets/l4080/l4080-chapter9.zip)

## Materiales a los que se hace referencia {#reference-materials}

* [Repositorio de Github](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modelos Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Exportador de modelo Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API de QueryBuilder](https://experienceleague.adobe.com/docs/)
* [AEM complemento de Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode)  ([página de documentación](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correcciones y seguimiento {#corrections-and-follow-up}

Correcciones y aclaraciones de las discusiones de laboratorio y respuestas a preguntas de seguimiento de los asistentes.

1. **¿Cómo dejar de reindexar?**

   La reindexación se puede detener a través de IndexStats MBean disponible a través de [AEM Web Console > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Ejecute `abortAndPause()` para anular la reindexación. Esto bloqueará el índice para volver a indexarlo hasta que se invoque `resume()`.
      * Al ejecutar `resume()` se reiniciará el proceso de indexación.
   * Documentación: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **¿Cómo pueden los índices oak soportar múltiples inquilinos?**

   Oak admite la colocación de índices a través del árbol de contenido, y estos índices solo indexarán dentro de ese sub-árbol. Por ejemplo, **`/content/site-a/oak:index/cqPageLucene`** se puede crear para indexar contenido solo bajo **`/content/site-a`.**

   Un enfoque equivalente es usar las propiedades **`includePaths`** y **`queryPaths`** en un índice en **`/oak:index`**. Por ejemplo:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Las consideraciones con este enfoque son:

   * Las consultas DEBEN especificar una restricción de ruta que sea igual al alcance de la ruta de consulta del índice, o ser un descendiente de allí.
   * Los índices de ámbitos más amplios (por ejemplo `/oak:index/cqPageLucene`) TAMBIÉN indexarán los datos, lo que dará como resultado una ingesta duplicada y costo de uso del disco.
   * Puede requerir una administración de configuración duplicada (por ejemplo, agregar el mismo indexRules en varios índices de inquilino si deben satisfacer los mismos conjuntos de consultas)
   * Este enfoque se sirve mejor en el nivel de AEM Publish para la búsqueda de sitio personalizada, como en AEM Author, es común que las consultas se ejecuten en la parte superior del árbol de contenido para diferentes inquilinos (por ejemplo, a través de OmniSearch): diferentes definiciones de índice pueden resultar en un comportamiento diferente solo en función de la restricción de ruta.


3. **¿Dónde hay una lista de todos los analizadores disponibles?**

   Oak expone un conjunto de elementos de configuración del analizador de lucene que se utilizan en AEM.

   * [Documentación de Apache Oak Analyzers](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtros](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **¿Cómo se busca Páginas y Recursos en la misma consulta?**

   Una novedad en AEM 6.3 es la capacidad de consultar varios tipos de nodos en la misma consulta proporcionada. La siguiente consulta de QueryBuilder. Tenga en cuenta que cada &quot;subconsulta&quot; puede resolverse en su propio índice, por lo que en este ejemplo, la subconsulta `cq:Page` se resuelve en `/oak:index/cqPageLucene` y la subconsulta `dam:Asset` se resuelve en `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   tiene como resultado el siguiente plan de consulta y consulta:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Explore la consulta y los resultados a través de [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) y [AEM complemento de Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **¿Cómo buscar en varias rutas en la misma consulta?**

   Una novedad en AEM 6.3 es la capacidad de realizar consultas en varias rutas en la misma consulta proporcionada. La siguiente consulta de QueryBuilder. Tenga en cuenta que cada &quot;subconsulta&quot; puede resolverse en su propio índice.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   resultados en el siguiente plan de consulta y consulta

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Explore la consulta y los resultados a través de [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) y [AEM complemento de Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

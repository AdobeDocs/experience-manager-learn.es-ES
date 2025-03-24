---
title: Guía de implementación de búsqueda simple
description: La implementación de búsqueda simple es el material del laboratorio Summit 2017 AEM Search Demystified. Esta página contiene los materiales de este laboratorio. Para una visita guiada por el laboratorio, por favor vea el libro del laboratorio en la sección Presentación de esta página.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 138
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 2%

---

# Guía de implementación de búsqueda simple{#simple-search-implementation-guide}

La implementación de búsqueda simple es el material de **Adobe Summit lab AEM Search Demystified**. Esta página contiene los materiales de este laboratorio. Para una visita guiada por el laboratorio, por favor vea el libro del laboratorio en la sección Presentación de esta página.

![Descripción general de la arquitectura de búsqueda](assets/l4080/simple-search-application.png)

## Materiales de presentación {#bookmarks}

* [Libro de laboratorio](assets/l4080/l4080-lab-workbook.pdf)
* [Presentación](assets/l4080/l4080-presentation.pdf)

## Marcadores {#bookmarks-1}

### Herramientas {#tools}

* [Administrador de índices](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Explicar consulta](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [Administrador de paquetes CRX](http://localhost:4502/crx/packmgr/index.jsp)
* [Depurador de QueryBuilder](¿http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Generador de definiciones de índice de Oak](https://oakutils.appspot.com/generate/index)

### Capítulos {#chapters}

*Los vínculos de capítulo siguientes suponen que los [paquetes iniciales](#initialpackages) están instalados en AEM Author en`http://localhost:4502`*

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
* [Capítulo 3 Solución](assets/l4080/l4080-chapter3.zip)
* [Capítulo 4 Solución](assets/l4080/l4080-chapter4.zip)
* [Configuración del capítulo 5](assets/l4080/l4080-chapter5-setup.zip)
* [Solución del capítulo 5](assets/l4080/l4080-chapter5-solution.zip)
* [Capítulo 6 Solución](assets/l4080/l4080-chapter6.zip)
* [Solución del capítulo 9](assets/l4080/l4080-chapter9.zip)

## Materiales de referencia {#reference-materials}

* [Repositorio de GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Modelos de Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Exportador de modelos Sling](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [API de QueryBuilder](https://experienceleague.adobe.com/docs/)
* [Complemento de AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([Página de documentación](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correcciones y seguimiento {#corrections-and-follow-up}

Correcciones y aclaraciones de las discusiones del laboratorio y respuestas a las preguntas de seguimiento de los asistentes.

1. **¿Cómo detener la reindexación?**

   La reindexación se puede detener mediante el MBean IndexStats disponible a través de [Consola web de AEM > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Ejecute `abortAndPause()` para anular la reindexación. Esto bloqueará el índice para volver a indexarlo hasta que se invoque `resume()`.
      * Ejecutar `resume()` reiniciará el proceso de indización.
   * Documentación: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **¿Cómo pueden los índices de Oak admitir varios inquilinos?**

   Oak admite la colocación de índices en todo el árbol de contenido, y estos índices solo indexarán dentro de ese subárbol. Por ejemplo, **`/content/site-a/oak:index/cqPageLucene`** se pudo crear para indexar contenido solamente bajo **`/content/site-a`.**

   Un enfoque equivalente es usar las propiedades **`includePaths`** y **`queryPaths`** en un índice bajo **`/oak:index`**. Por ejemplo:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   Las consideraciones con este enfoque son las siguientes:

   * Las consultas DEBEN especificar una restricción de ruta de acceso igual al ámbito de la ruta de acceso de la consulta del índice o ser un descendiente de él.
   * Los índices de ámbito más amplio (por ejemplo `/oak:index/cqPageLucene`) TAMBIÉN indexarán los datos, lo que dará como resultado una ingesta duplicada y un costo de uso de disco.
   * Puede requerir la administración de configuraciones duplicadas (por ejemplo, agregar las mismas reglas de índice en varios índices de inquilinos si deben satisfacer los mismos conjuntos de consultas)
   * Este método se sirve mejor en el nivel de publicación de AEM para la búsqueda de sitios personalizados, ya que en AEM Author es común que las consultas se ejecuten en la parte superior del árbol de contenido para diferentes inquilinos (por ejemplo, a través de OmniSearch): diferentes definiciones de índice pueden dar como resultado un comportamiento diferente basado únicamente en la restricción de ruta.

3. **¿Dónde hay una lista de todos los analizadores disponibles?**

   Oak expone un conjunto de elementos de configuración del analizador que proporciona lucene para su uso en AEM.

   * [Documentación de los analizadores Apache Oak](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filtros](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **¿Cómo buscar páginas y Assets en la misma consulta?**

   Una novedad en AEM 6.3 es la capacidad de consultar varios tipos de nodos en la misma consulta proporcionada. La siguiente consulta de QueryBuilder. Tenga en cuenta que cada &quot;subconsulta&quot; puede resolver su propio índice, por lo que en este ejemplo, la subconsulta `cq:Page` se resuelve en `/oak:index/cqPageLucene` y la subconsulta `dam:Asset` se resuelve en `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   da como resultado la siguiente consulta y plan de consulta:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   Explore la consulta y los resultados a través de [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) y [complemento de AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **¿Cómo buscar en varias rutas de acceso en la misma consulta?**

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

   da como resultado la siguiente consulta y plan de consulta

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   Explore la consulta y los resultados a través de [QueryBuilder Debugger](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) y [complemento de AEM Chrome](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

---
title: Advertencias transitorias en AEM as a Cloud Service
description: Aprenda a mitigar las advertencias transversales en AEM as a Cloud Service.
topics: Migration
feature: Migration
role: Architect, Developer
level: Beginner
kt: 10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
source-git-commit: 48943df64d9793066f8f19497ef42f8aa80e5795
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 3%

---

# Advertencias transitorias

>[!TIP]
>Marque esta página para referencia futura.

_¿Qué son las advertencias transversales?_

Las advertencias transitorias son __aemerror__ las instrucciones de registro que indican que las consultas de bajo rendimiento se están ejecutando en el servicio AEM Publish. Las advertencias transversales se suelen manifestar de AEM de dos maneras:

1. __Consultas lentas__ que no utilizan índices, lo que resulta en tiempos de respuesta lentos.
1. __Consultas fallidas__, que generan un `RuntimeNodeTraversalException`, lo que provoca una experiencia rota.

Permitir que las advertencias transversales no estén seleccionadas ralentiza AEM rendimiento y puede provocar experiencias rotas para los usuarios.

## Cómo resolver advertencias transversales

La mitigación de las advertencias transversales se puede abordar mediante tres pasos sencillos: analice, ajuste y verifique. Espere varias iteraciones de ajuste y verifique antes de identificar los ajustes óptimos.

<div class="columns is-multiline">

<!-- Analyze -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Analyze" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#analyze" title="Analizar" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/1-analyze.png" alt="Analizar">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Analizar el problema</p>
               <p class="is-size-6">Identificar y comprender qué consultas están atravesando.</p>
               <a href="#analyze" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Analizar</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Adjust -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adjust" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#adjust" title="Ajustar" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/2-adjust.png" alt="Ajustar">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Ajustar el código o la configuración</p>
               <p class="is-size-6">Actualice las consultas y los índices para evitar los recorridos de las consultas.</p>
               <a href="#adjust" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ajustar</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Verify -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Verify" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#verify" title="Verificar" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="Verificar">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Verificar que los ajustes hayan funcionado</p>                       
               <p class="is-size-6">Compruebe que los cambios realizados en las consultas y los índices eliminen los cambios.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Verificar</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. Analizar{#analyze}

En primer lugar, identifique qué servicios de AEM Publish presentan advertencias transversales. Para ello, desde Cloud Manager, [descargar servicios de publicación&#39; `aemerror` logs](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html#cloud-manager){target=&quot;_blank&quot;} de todos los entornos (desarrollo, fase y producción) del pasado __tres días__.

![Descargar AEM registros as a Cloud Service](./assets/traversals/download-logs.jpg)

Abra los archivos de registro y busque la clase Java™ `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. El registro que contiene advertencias transversales contiene una serie de afirmaciones que tienen un aspecto similar a:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

Según el contexto de la ejecución de la consulta, las instrucciones de registro pueden contener información útil sobre el creador de la consulta:

+ URL de solicitud HTTP asociada a la ejecución de la consulta

   + Ejemplo: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Sintaxis de consulta Oak

   + Ejemplo: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ Consulta XPath

   + Ejemplo: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Código que ejecuta la consulta

   + Ejemplo:  `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__Consultas fallidas__ van seguidas de una `RuntimeNodeTraversalException` , similar a:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. Ajustar{#adjust}

Una vez descubiertas las consultas ofensivas y su código de invocación, deben realizarse ajustes. Se pueden realizar dos tipos de ajustes para mitigar las advertencias transversales:

### Ajuste de la consulta

__Cambiar la consulta__ para agregar nuevas restricciones de consulta que se resuelvan a restricciones de índice existentes. Cuando sea posible, prefiera cambiar la consulta a cambiar los índices.

+ [Aprenda a ajustar el rendimiento de las consultas](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}

### Ajustar el índice

__Cambiar (o crear) un índice de AEM__ de modo que las restricciones de consulta existentes se puedan resolver en las actualizaciones de índice.

+ [Aprenda a ajustar índices existentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#query-performance-tuning){target=&quot;_blank&quot;}
+ [Aprenda a crear índices](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html#create-a-new-index){target=&quot;_blank&quot;}

## 3. Verificar{#verify}

Los ajustes realizados en las consultas, los índices o ambos deben verificarse para garantizar que mitigan las advertencias transversales.

![Explicar consulta](./assets/traversals/verify.gif)

If only [ajustes a la consulta](#adjust-the-query) se realizan, la consulta se puede probar directamente en AEM as a Cloud Service mediante el [Explicar consulta](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;}. Explicar que Query se ejecuta con el servicio AEM Author, sin embargo, como las definiciones de índice son las mismas en los servicios Autor y Publicación, la validación de consultas con el servicio AEM Author es suficiente.

If [ajustes en el índice](#adjust-the-index) se realizan, el índice debe implementarse en AEM as a Cloud Service. Con los ajustes de índice implementados, Developer Console [Explicar consulta](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html#queries){target=&quot;_blank&quot;} se puede usar para ejecutar y ajustar la consulta aún más.

En última instancia, todos los cambios (consulta y código) se comprometen con Git y se implementan en AEM as a Cloud Service mediante Cloud Manager. Una vez implementadas, pruebe de nuevo las rutas de código asociadas con las advertencias transversales originales y verifique que ya no aparezcan advertencias transversales en la variable `aemerror` log.

## Otros recursos

Consulte estos otros recursos útiles para comprender AEM índices, búsquedas y advertencias transversales.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5: Búsqueda e indexación" tabindex="-1"><img class="is-bordered-r-small" src="../../../cloud-5/imgs/009-thumb.png" alt="Cloud 5: Búsqueda e indexación"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" title="Cloud 5: Búsqueda e indexación">Cloud 5: Búsqueda e indexación</a></p>
               <p class="is-size-6">El equipo de Cloud 5 muestra exploraciones de los entresijos de la búsqueda y la indexación en AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-5/cloud5-aem-search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Content Search and Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Content Search and Indexing
" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="Buscar contenido e indexar" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="Buscar contenido e indexar">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" title="Buscar contenido e indexar">Documentación de indexación y búsqueda de contenido</a></p>
               <p class="is-size-6">Aprenda a crear y administrar índices en AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Modernizing your Oak indexes -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modernizing your Oak indexes" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernización de los índices de Oak" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Modernización de los índices de Oak">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" title="Modernización de los índices de Oak">Modernización de los índices de Oak</a></p>
               <p class="is-size-6">Aprenda a convertir AEM 6 definiciones de índices Oak para que sean compatibles con AEM as a Cloud Service, y a mantener los índices en adelante.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Index definition documentation -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Index definition documentation" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentación de definición de índice" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="Documentación de definición de índice">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentación de definición de índice">Documentación del índice Lucene</a></p>
               <p class="has-ellipsis is-size-6">La referencia del índice Apache Oak Jackrabbit Lucene que documenta todas las configuraciones de índice de Lucene admitidas.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

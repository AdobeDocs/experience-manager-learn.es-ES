---
title: Advertencias transversales en AEM as a Cloud Service
description: Obtenga información sobre cómo mitigar las advertencias de recorrido en AEM as a Cloud Service.
feature: Migration
role: Developer
level: Beginner
jira: KT-10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
duration: 155
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 5%

---

# Advertencias transversales

>[!TIP]
>Añada esta página a marcadores para volver a ella en el futuro.

_¿Qué son las advertencias de recorrido?_

Las advertencias transversales son __aemerror__ instrucciones de registro que indican que se están ejecutando consultas de mal rendimiento en el servicio de publicación de AEM. Las advertencias transversales suelen manifestarse en AEM de dos maneras:

1. __Consultas lentas__ que no utilizan índices, lo que resulta en tiempos de respuesta lentos.
1. __Consultas con errores__, que generan un `RuntimeNodeTraversalException`, lo que da como resultado una experiencia rota.

Si se permiten las advertencias de recorrido sin marcar, se ralentiza el rendimiento de AEM y puede provocar que los usuarios se rompan las experiencias.

## Cómo resolver las advertencias de recorrido

La mitigación de las advertencias transversales se puede abordar mediante tres simples pasos: analizar, ajustar y verificar. Espere varias iteraciones de ajuste y verifique antes de identificar los ajustes óptimos.

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
                <p class="headline is-size-5 has-text-weight-bold">Analice el problema</p>
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
                <p class="headline is-size-5 has-text-weight-bold">Ajuste el código o la configuración</p>
               <p class="is-size-6">Actualice las consultas y los índices para evitar los recorridos por consultas.</p>
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
                <p class="headline is-size-5 has-text-weight-bold">Verifique que los ajustes funcionaron</p>                       
               <p class="is-size-6">Verifique los cambios en las consultas y los índices eliminen las travesías.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Verificar</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## &#x200B;1. Analizar{#analyze}

En primer lugar, identifique qué servicios de publicación de AEM muestran advertencias transversales. Para ello, desde Cloud Manager, [descargue los registros `aemerror` de los servicios de publicación](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html?lang=es#cloud-manager){target="_blank"} de todos los entornos (desarrollo, ensayo y producción) durante los últimos __tres días__.

![Descargar registros de AEM as a Cloud Service](./assets/traversals/download-logs.jpg)

Abra los archivos de registro y busque la clase Java™ `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor`. El registro que contiene advertencias de recorrido contiene una serie de instrucciones similares a las siguientes:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

Según el contexto de ejecución de la consulta, las instrucciones de registro pueden contener información útil sobre el creador de la consulta:

+ URL de solicitud HTTP asociada a la ejecución de consultas

   + Ejemplo: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Sintaxis de consultas de Oak

   + Ejemplo: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ Consulta XPath

   + Ejemplo: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Código que ejecuta la consulta

   + Ejemplo: `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__Las consultas con errores__ van seguidas de una instrucción `RuntimeNodeTraversalException`, similar a:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## &#x200B;2. Ajuste{#adjust}

Una vez descubiertas las consultas ofensivas y su código de invocación, se deben realizar ajustes. Se pueden realizar dos tipos de ajustes para mitigar las advertencias de recorrido:

### Ajuste de la consulta

__Cambie la consulta__ para agregar nuevas restricciones de consulta que se resuelvan en las restricciones de índice existentes. Cuando sea posible, prefiera cambiar la consulta a cambiar los índices.

+ [Aprenda a ajustar el rendimiento de la consulta](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=es#query-performance-tuning){target="_blank"}

### Ajuste del índice

__Cambie (o cree) un índice de AEM__ de forma que las restricciones de consultas existentes puedan resolverse en las actualizaciones de índice.

+ [Aprenda a ajustar los índices existentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=es#query-performance-tuning){target="_blank"}
+ [Aprenda a crear índices](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=es#create-a-new-index){target="_blank"}

## &#x200B;3. Verificar{#verify}

Los ajustes realizados en las consultas, los índices o ambos deben verificarse para garantizar que mitigan las advertencias de recorrido.

![Explicar consulta](./assets/traversals/verify.gif)

Si solo se realizan [ajustes en la consulta](#adjust-the-query), la consulta se puede probar directamente en AEM as a Cloud Service a través de [Explain Query](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=es#queries){target="_blank"} de Developer Console. Explicar La consulta se ejecuta con el servicio de AEM Author; sin embargo, dado que las definiciones de índice son las mismas en los servicios de creación y publicación, es suficiente validar las consultas con el servicio de AEM Author.

Si se realizan [ajustes en el índice](#adjust-the-index), el índice debe implementarse en AEM as a Cloud Service. Con los ajustes de índice implementados, se puede usar [Explain Query](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=es#queries){target="_blank"} de Developer Console para ejecutar y ajustar la consulta con más detalle.

En última instancia, todos los cambios (consulta y código) se comprometen con Git y se implementan en AEM as a Cloud Service mediante Cloud Manager. Una vez implementadas, pruebe las rutas de código asociadas con las advertencias de recorrido originales y compruebe que las advertencias de recorrido ya no aparecen en el registro `aemerror`.

## Otros recursos

Consulte estos otros recursos útiles para comprender los índices AEM, las búsquedas y las advertencias de recorrido.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=es" title="Cloud 5: Búsqueda e indexación" tabindex="-1"><img class="is-bordered-r-small" src="../../../expert-resources/cloud-5/imgs/009-thumb.png" alt="Cloud 5: Búsqueda e indexación"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=es" title="Cloud 5: Búsqueda e indexación">Cloud 5: Búsqueda e indexación</a></p>
               <p class="is-size-6">Los programas de equipo de Cloud 5 exploran los detalles de la búsqueda y la indexación en AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=es" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
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
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=es" title="Búsqueda de contenido e indexación" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="Búsqueda de contenido e indexación">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=es" title="Búsqueda de contenido e indexación">Documentación de indexación y búsqueda de contenido</a></p>
               <p class="is-size-6">Obtenga información sobre cómo crear y administrar índices en AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=es" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
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
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=es" title="Modernización de los índices de Oak" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Modernización de los índices de Oak">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=es" title="Modernización de los índices de Oak">Modernización de los índices de Oak</a></p>
               <p class="is-size-6">Aprenda a convertir definiciones de índice de Oak de AEM 6 para que sean compatibles con AEM as a Cloud Service y a mantener los índices en el futuro.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=es" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
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
               <p class="has-ellipsis is-size-6">La referencia del índice Apache Oak Jackrabbit Lucene que documenta todas las configuraciones de índice Lucene admitidas.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

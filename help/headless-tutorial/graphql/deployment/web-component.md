---
title: AEM implementaciones de componentes web sin encabezado
description: Obtenga información sobre las consideraciones de implementación para implementaciones sin encabezado basadas en componentes web o en JS puro.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---


# AEM implementaciones de componentes web sin encabezado

AEM sin encabezado [Componente web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)Las implementaciones de /JS son aplicaciones puras de JavaScript que se ejecutan en un explorador web y que consumen e interactúan con el contenido de AEM de una forma directa. Las implementaciones de componentes web/JS difieren de las implementaciones de [SPA implementaciones](./spa.md) en el sentido de que no utilizan un marco de SPA sólido, y se espera que estén incrustados en el contexto de cualquier sitio web, entregar, para mostrar contenido de AEM.


## Configuraciones de implementación

La siguiente configuración de implementación debe estar in situ para implementaciones de componentes web/JS.

| La aplicación de componente web/JS se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ü | š | š |
| [Uso compartido de recursos de origen diverso (CORS)](./configurations/cors.md) | š | š | š |
| [AEM hosts](./configurations/aem-hosts.md) | š | š | š |

## Ejemplo de componente web

Adobe proporciona un ejemplo de componente web.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Componente web" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Componente web">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Componente web">Componente web</a></p>
                   <p class="is-size-6">Componente web de ejemplo, escrito en JavaScript puro, que consume contenido de AEM API de GraphQL sin encabezado.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>

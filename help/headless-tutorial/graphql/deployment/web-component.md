---
title: AEM Implementaciones de componentes web sin encabezado
description: AEM Obtenga información acerca de las consideraciones de implementación para implementaciones sin encabezado de componentes web/basadas en JS puro.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---

# AEM Implementaciones de componentes web sin encabezado

AEM Sin encabezado [Componente web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)AEM Las implementaciones de /JS son aplicaciones JavaScript puras que se ejecutan en un explorador web, que consumen contenido e interactúan con él de manera sin encabezado, de manera que se puede ejecutar sin encabezado. Las implementaciones de componentes web/JS difieren de [SPA implementaciones de](./spa.md) SPA AEM en el sentido de que no utilizan un marco de trabajo de robusto y se espera que estén incrustados en el contexto de cualquier sitio web, entreguen o surjan contenidos de la red de manera que puedan ser mostrados por los usuarios.


## Configuraciones de implementación

La siguiente configuración de implementación debe estar implementada para implementaciones de componentes web/JS.

| La aplicación web Component/JS se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Uso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM anfitriones de](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Ejemplo de componente web

El Adobe proporciona un ejemplo de componente web.

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
                   <p class="is-size-6">AEM Componente web de ejemplo, escrito en JavaScript puro, que consume contenido de las API de GraphQL sin encabezado, que se utilizan para la creación de contenido sin encabezado.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>

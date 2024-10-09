---
title: AEM Implementaciones de componentes web sin encabezado
description: AEM Obtenga información acerca de las consideraciones de implementación para implementaciones sin encabezado de componentes web/basadas en JS puro.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 089bcf71f03bdbb6d21337cc23452afb33ce8098
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 2%

---

# AEM Implementaciones de componentes web sin encabezado

AEM Las implementaciones de [Web Component](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS sin encabezado son aplicaciones de JavaScript AEM puras que se ejecutan en un explorador web y que consumen contenido e interactúan con él de manera sin encabezado, de manera que no tienen encabezado. SPA SPA AEM Las implementaciones de componentes web/JS difieren de las [implementaciones de](./spa.md) en que no utilizan un marco de trabajo sólido y se espera que estén incrustadas en el contexto de cualquier sitio web, que entreguen y que surjan contenidos de las páginas de la página de la que se vaya a realizar el envío.


## Configuraciones de implementación

La siguiente configuración de implementación debe estar implementada para implementaciones de componentes web/JS.

| La aplicación web Component/JS se conecta a → | AEM Author | Publicación de AEM | AEM Previsualización de |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Uso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| AEM [hosts de la](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

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
                   <p class="is-size-6">Componente web de ejemplo, escrito en JavaScript AEM puro, que consume contenido de las API de GraphQL sin encabezado de la red de aplicaciones de la interfaz de usuario de.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>

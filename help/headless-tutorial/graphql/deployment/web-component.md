---
title: Implementaciones de componentes web sin encabezado de AEM
description: Obtenga información acerca de las consideraciones de implementación para implementaciones sin encabezado de AEM basadas en componentes web/JS puro.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 2%

---

# Implementaciones de componentes web sin encabezado de AEM

Las implementaciones de AEM sin encabezado [Web Component](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS son aplicaciones de JavaScript puras que se ejecutan en un explorador web, que consumen contenido en AEM e interactúan con él sin encabezado. Las implementaciones de componentes web/JS difieren de las [implementaciones de SPA](./spa.md) en que no utilizan un marco de trabajo de SPA robusto y se espera que estén incrustadas en el contexto de cualquier sitio web, envío, para que aparezcan contenidos de AEM.


## Configuraciones de implementación

La siguiente configuración de implementación debe estar implementada para implementaciones de componentes web/JS.

| La aplicación web Component/JS se conecta a → | AEM Author | Publicación de AEM | Previsualización de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Uso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hosts de AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

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
                   <p class="is-size-6">Componente web de ejemplo, escrito en JavaScript puro, que consume contenido de las API de GraphQL sin encabezado de AEM.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>

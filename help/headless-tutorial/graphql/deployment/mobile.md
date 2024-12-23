---
title: AEM Implementaciones móviles sin encabezado
description: AEM Obtenga información acerca de las consideraciones de implementación para implementaciones móviles sin encabezado
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
duration: 31
source-git-commit: 23ea95cfdf7e4c9fde4b53e9f68079b4d267ca20
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 2%

---

# AEM Implementaciones móviles sin encabezado

AEM Las implementaciones móviles sin encabezado son aplicaciones móviles nativas para iOS, Android™, etc. AEM que consumen contenido e interactúan con él de manera sin encabezado, de manera que se pueden usar en el proceso de.

AEM Las implementaciones móviles requieren una configuración mínima, ya que las conexiones HTTP a las API sin encabezado de la aplicación no se inician en el contexto de un explorador.

## Configuraciones de implementación

La siguiente configuración de implementación debe estar implementada para implementaciones de aplicaciones móviles.

| La aplicación móvil se conecta al → | AEM Author | Publicación de AEM | AEM Previsualización de |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Uso compartido de recursos de origen cruzado (CORS) | ✘ | ✘ | ✘ |
| AEM [hosts de la](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Ejemplo de aplicaciones móviles

El Adobe de proporciona aplicaciones móviles de ejemplo de iOS y Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="aplicación de iOS" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="aplicación de iOS">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="aplicación de iOS">aplicación de iOS</a></p>
                   <p class="is-size-6">Una aplicación de iOS AEM de ejemplo, escrita en SwiftUI, que consume contenido de las API de GraphQL sin encabezado, que se utilizan para el uso de la aplicación sin encabezado.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="aplicación Android™" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="aplicación de Android">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="aplicación Android™">aplicación Android™</a></p>
                   <p class="is-size-6">Aplicación de ejemplo de Java™ AndroidAEM ™ que consume contenido de las API de GraphQL sin encabezado de la aplicación de la aplicación de la interfaz de usuario de la aplicación.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>

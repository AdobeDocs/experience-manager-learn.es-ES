---
title: AEM implementaciones móviles sin encabezado
description: Obtenga información sobre las consideraciones de implementación para implementaciones móviles AEM sin encabezado.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---


# AEM implementaciones móviles sin encabezado

AEM implementaciones móviles sin encabezado son aplicaciones móviles nativas para iOS, Android™, etc. que consumen e interactúan con el contenido de AEM de una manera sin encabezado.

Las implementaciones móviles requieren una configuración mínima, ya que las conexiones HTTP a API sin encabezado AEM no se inician en el contexto de un explorador.

## Configuraciones de implementación

La siguiente configuración de implementación debe estar en su lugar para implementaciones de aplicaciones móviles.

| La aplicación móvil se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ü | š | š |
| Uso compartido de recursos de origen diverso (CORS) | ü | ü | ü |
| [AEM hosts](./configurations/aem-hosts.md) | š | š | š |

## Ejemplos de aplicaciones móviles

Adobe proporciona ejemplos de aplicaciones móviles iOS y Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="aplicación iOS" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="aplicación iOS">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="aplicación iOS">aplicación iOS</a></p>
                   <p class="is-size-6">Una aplicación de iOS de ejemplo, escrita en SwiftUI, que consume contenido de AEM API de GraphQL sin encabezado.</p>
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
                   <a href="../example-apps/android-app.md" title="Aplicación Android™" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Aplicación de Android">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Aplicación Android™">Aplicación Android™</a></p>
                   <p class="is-size-6">Ejemplo de aplicación Java™ Android™ que consume contenido de las API de GraphQL sin encabezado AEM.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>



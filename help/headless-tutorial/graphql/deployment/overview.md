---
title: AEM Implementaciones sin encabezado de
description: AEM Obtenga información acerca de las distintas consideraciones de implementación para aplicaciones sin encabezado de.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 120
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# AEM Implementaciones sin encabezado de

AEM AEM SPA SPA Las implementaciones de cliente sin encabezado toman muchos formularios; las implementaciones de cliente alojadas en la interfaz de usuario, las de servidor a servidor, las de sitio web, las de aplicación móvil o incluso las de servidor a servidor, entre otras, las de externas.

AEM En función del cliente y de cómo se implemente, las implementaciones sin encabezado de la tienen diferentes consideraciones.

## AEM arquitectura de servicio de

AEM AEM Antes de explorar las consideraciones de implementación, es imperativo comprender la arquitectura lógica de la implementación, así como la separación y las funciones de los niveles de servicio de los as a Cloud Service de la implementación, y las características de los mismos. AEM El as a Cloud Service consta de dos servicios lógicos:

+ __AEM Autor de__ es el servicio donde los equipos crean, colaboran y publican fragmentos de contenido (y otros recursos).
+ __AEM Publicación de__ es el servicio que se publicó. Los fragmentos de contenido (y otros recursos) se replican para el consumo general.
+ __AEM Previsualización de__ AEM es el servicio que imita el comportamiento de Publicación, pero que tiene contenido publicado para vista previa o revisión. AEM La vista previa de la versión se ha diseñado para audiencias internas y no para la publicación general de contenido. AEM El uso de la vista previa de la es opcional, según el flujo de trabajo deseado.

![AEM arquitectura de servicio de](./assets/overview/aem-service-architecture.png)

AEM Arquitectura de implementación típica sin encabezado as a Cloud Service de la

AEM AEM Los clientes sin encabezado que operan en una capacidad de producción suelen interactuar con Publish, que contiene el contenido publicado y aprobado, y que no tiene encabezado. AEM AEM Los clientes que interactúen con el autor de la comunicación deben tener especial cuidado, ya que el autor de la comunicación es seguro de forma predeterminada, requiere autorización para todas las solicitudes y también puede incluir trabajo en curso o contenido no aprobado.

## Implementaciones de cliente sin encabezado

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="SPA Aplicación de una sola página ()" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="SPA Aplicaciones de una sola página ()">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="SPA Aplicación de una sola página ()">SPA Aplicación de una sola página ()</a></p>
                   <p class="is-size-6">SPA Obtenga información acerca de las consideraciones de implementación para aplicaciones de una sola página ().</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Componente web/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Componente web/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Componente web/JS">Componente web/JS</a></p>
               <p class="is-size-6">Obtenga información acerca de las consideraciones de implementación para componentes web y consumidores sin encabezado JavaScript basados en explorador.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="Aplicaciones móviles" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="Aplicaciones móviles">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="Aplicaciones móviles">aplicación móvil</a></p>
               <p class="is-size-6">Obtenga información sobre las consideraciones de implementación para aplicaciones móviles.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="Aplicaciones de servidor a servidor" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="Aplicaciones de servidor a servidor">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="Aplicaciones de servidor a servidor">Aplicación de servidor a servidor</a></p>
               <p class="is-size-6">Obtenga información acerca de las consideraciones de implementación para aplicaciones de servidor a servidor</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aprender</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>

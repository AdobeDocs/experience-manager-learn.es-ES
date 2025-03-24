---
title: Implementaciones sin encabezado de AEM
description: Obtenga información acerca de las distintas consideraciones de implementación para aplicaciones sin encabezado de AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 59
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# Implementaciones sin encabezado de AEM

Las implementaciones de cliente sin encabezado de AEM toman muchos formularios; SPA alojada en AEM, SPA externa, sitio web, aplicación móvil o incluso proceso de servidor a servidor.

Según el cliente y cómo se implemente, las implementaciones sin encabezado de AEM tienen diferentes consideraciones.

## Arquitectura de servicio de AEM

Antes de explorar las consideraciones de implementación, es imperativo comprender la arquitectura lógica de AEM, y la separación y las funciones de los niveles de servicio de AEM as a Cloud Service. AEM as a Cloud Service consta de dos servicios lógicos:

+ __Autor de AEM__ es el servicio donde los equipos crean, colaboran y publican fragmentos de contenido (y otros recursos).
+ __Publicación de AEM__ es el servicio en el que se publicaron Los fragmentos de contenido (y otros recursos) se replican para uso general.
+ __Vista previa de AEM__ es el servicio que imita la publicación de AEM en comportamiento, pero tiene contenido publicado para vista previa o revisión. La vista previa de AEM está destinada a audiencias internas y no a la entrega general de contenido. El uso de la vista previa de AEM es opcional, según el flujo de trabajo deseado.

![Arquitectura de servicio de AEM](./assets/overview/aem-service-architecture.png)

Arquitectura de implementación típica sin encabezado de AEM as a Cloud Service

Los clientes sin encabezado de AEM que operan en una capacidad de producción suelen interactuar con AEM Publish, que contiene el contenido aprobado y publicado. Los clientes que interactúen con AEM Author deben tener especial cuidado, ya que AEM Author es seguro de forma predeterminada, y requiere autorización para todas las solicitudes. También puede incluir trabajo en curso o contenido no aprobado.

## Implementaciones de cliente sin encabezado

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="Aplicación de una sola página (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Aplicaciones de una sola página (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="Aplicación de una sola página (SPA)">Aplicación de una sola página (SPA)</a></p>
                   <p class="is-size-6">Obtenga información acerca de las consideraciones de implementación para aplicaciones de una sola página (SPA).</p>
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
               <p class="is-size-6">Obtenga información acerca de las consideraciones de implementación para componentes web y consumidores sin encabezado de JavaScript basados en explorador.</p>
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
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="Aplicaciones móviles">Aplicación móvil</a></p>
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

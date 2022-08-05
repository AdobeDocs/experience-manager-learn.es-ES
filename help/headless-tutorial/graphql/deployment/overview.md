---
title: AEM implementaciones sin encabezado
description: Obtenga información sobre las distintas consideraciones de implementación para AEM aplicaciones sin encabezado.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# AEM implementaciones sin encabezado

AEM implementaciones de cliente sin encabezado adoptan muchas formas; SPA alojadas en AEM, SPA externa, sitio web, aplicación móvil o incluso proceso de servidor a servidor.

Según el cliente y cómo se implemente, AEM implementaciones sin encabezado tienen diferentes consideraciones.

## arquitectura de servicio AEM

Antes de explorar las consideraciones de implementación, es imperativo comprender AEM arquitectura lógica y la separación y las funciones de los niveles de servicio de AEM as a Cloud Service. AEM as a Cloud Service consta de dos servicios lógicos:

+ __Autor de AEM__ es el servicio en el que los equipos crean, colaboran y publican fragmentos de contenido (y otros recursos).
+ __AEM Publish__ es el servicio que se publicó en los fragmentos de contenido (y otros recursos) y que se replica para el consumo general.
+ __Vista previa de AEM__ es el servicio que imita el comportamiento de AEM Publish, pero que tiene contenido publicado para fines de vista previa o revisión. AEM Vista previa está diseñada para audiencias internas y no para la entrega general de contenido. El uso de AEM Vista previa es opcional, en función del flujo de trabajo deseado.

![arquitectura de servicio AEM](./assets/overview/aem-service-architecture.png)

Arquitectura de implementación típica AEM as a Cloud Service sin encabezado

AEM clientes sin encabezado que operan con capacidad de producción normalmente interactúan con AEM Publish, que contiene el contenido aprobado y publicado. Los clientes que interactúan con AEM Author deben tener especial cuidado, ya que AEM Author es seguro de forma predeterminada, y requiere autorización para todas las solicitudes, y también puede contener trabajo en curso o contenido no aprobado.

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
                   <p class="is-size-6">Obtenga información sobre las consideraciones de implementación para aplicaciones de una sola página (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
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
               <p class="is-size-6">Obtenga información sobre las consideraciones de implementación para componentes web y consumidores JavaScript sin encabezado basados en explorador.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
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
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
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
               <p class="is-size-6">Obtenga información sobre las consideraciones de implementación para aplicaciones de servidor a servidor</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
---
title: Implementaciones de servidor a servidor sin encabezado de AEM
description: Obtenga información acerca de las consideraciones de implementación para implementaciones sin encabezado de servidor a servidor de AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---

# Implementaciones de servidor a servidor sin encabezado de AEM

Las implementaciones de servidor a servidor sin encabezado de AEM implican aplicaciones o procesos del lado del servidor que consumen contenido en AEM e interactúan con él de forma directa.

Las implementaciones de servidor a servidor requieren una configuración mínima, ya que las conexiones HTTP a las API de AEM sin encabezado no se inician en el contexto de un explorador.

## Configuraciones de implementación

La siguiente configuración de implementación debe estar implementada para las implementaciones de aplicaciones de servidor a servidor.

| La aplicación de servidor a servidor se conecta a → | AEM Author | Publicación de AEM | Previsualización de AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Uso compartido de recursos de origen cruzado (CORS) | ✘ | ✘ | ✘ |
| [Hosts de AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Requisitos de autorización

Las solicitudes autorizadas a las API de GraphQL de AEM suelen producirse en el contexto de las aplicaciones servidor a servidor, ya que otros tipos de aplicaciones, como [aplicaciones de una sola página](./spa.md), [mobile](./mobile.md) o [Web Components](./web-component.md), suelen utilizar la autorización, ya que es difícil proteger las credenciales

Al autorizar solicitudes a AEM as a Cloud Service, use [autenticación de token basada en credenciales de servicio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=es). Para obtener más información acerca de la autenticación de solicitudes en AEM as a Cloud Service, consulte el [tutorial de autenticación basado en tokens](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=es). El tutorial explora la autenticación basada en tokens usando [API HTTP de AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html?lang=es), pero los mismos conceptos y enfoques son aplicables a las aplicaciones que interactúan con las API de GraphQL sin encabezado de AEM.

## Ejemplo de aplicación de servidor a servidor

Adobe proporciona un ejemplo de aplicación de servidor a servidor codificada en Node.js.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="Aplicación de servidor a servidor" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="Aplicación de servidor a servidor">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="Aplicación de servidor a servidor">Aplicación de servidor a servidor</a></p>
                   <p class="is-size-6">Aplicación de servidor a servidor de ejemplo, escrita en Node.js, que consume contenido de las API de GraphQL sin encabezado de AEM.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>

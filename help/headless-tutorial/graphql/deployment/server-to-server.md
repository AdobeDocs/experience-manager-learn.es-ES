---
title: AEM implementaciones indirectas de servidor a servidor
description: Obtenga información sobre las consideraciones de implementación para implementaciones sin encabezado de AEM servidor a servidor.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# AEM implementaciones indirectas de servidor a servidor

AEM implementaciones indirectas de servidor a servidor implican aplicaciones o procesos del lado del servidor que consumen e interactúan con el contenido de manera AEM y sin objetivos.

Las implementaciones servidor a servidor requieren una configuración mínima, ya que las conexiones HTTP a API sin AEM no se inician en el contexto de un explorador.

## Configuraciones de implementación

La siguiente configuración de implementación debe estar en su lugar para implementaciones de aplicaciones servidor a servidor.

| La aplicación servidor a servidor se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ü | š | š |
| Uso compartido de recursos de origen diverso (CORS) | ü | ü | ü |
| [AEM hosts](./configurations/aem-hosts.md) | š | š | š |

## Requisitos de autorización

Las solicitudes autorizadas para AEM las API de GraphQL suelen producirse en el contexto de aplicaciones de servidor a servidor, ya que otros tipos de aplicaciones, como [aplicaciones de una sola página](./spa.md), [mobile](./mobile.md)o [Componentes web](./web-component.md), normalmente utilizan autorización porque es difícil proteger las credenciales .

Al autorizar solicitudes para AEM as a Cloud Service, utilice [autenticación de token basada en credenciales de servicio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). Para obtener más información sobre la autenticación de solicitudes a AEM as a Cloud Service, consulte la [tutorial de autenticación basado en token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). El tutorial explora la autenticación basada en token mediante [API HTTP de AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) pero los mismos conceptos y enfoques se aplican a las aplicaciones que interactúan con AEM API de GraphQL sin encabezado.

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
                   <p class="is-size-6">Una aplicación de servidor a servidor de ejemplo, escrita en Node.js, que consume contenido de las API de GraphQL sin encabezado AEM.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
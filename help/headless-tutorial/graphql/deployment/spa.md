---
title: Implementación de SPA para AEM GraphQL
description: Obtenga información sobre las consideraciones de implementación para implementaciones sin encabezado de aplicaciones de una sola página (SPA) AEM.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 1%

---


# AEM implementaciones de SPA sin encabezado


AEM implementaciones de aplicaciones de una sola página (SPA) sin encabezado implican aplicaciones basadas en JavaScript creadas mediante marcos como React o Vue, que consumen contenido e interactúan con él de forma AEM y sin encabezado.

La implementación de una SPA que interactúa con los AEM de una manera incompleta implica alojar el SPA y hacer que sea accesible a través de un explorador web.

## Aloje el SPA

Un SPA consta de una colección de recursos web nativos: **HTML, CSS y JavaScript**. Estos recursos se generan durante la _versión_ proceso (por ejemplo, `npm run build`) e implementados en un host para su consumo por los usuarios finales.

Hay varias **hosting** según los requisitos de su organización:

1. **Proveedores de Cloud** como **Azure** o **AWS**.

2. **On-Premise** alojamiento en una empresa **centro de datos**

3. **Plataformas de alojamiento front-end** como **Amplificación de AWS**, **Servicio de aplicaciones de Azure**, **Netlify**, **Heroku**, **Vercel**, etc.

## Configuraciones de implementación

La consideración principal al alojar un SPA que interactúa con AEM sin encabezado es si se accede al SPA a través de AEM dominio (o host) o en un dominio diferente.  El motivo es SPA las aplicaciones web se ejecutan en exploradores web y, por lo tanto, están sujetas a las políticas de seguridad de los exploradores web.

### Dominio compartido

Un SPA y AEM comparten dominios cuando los usuarios finales del mismo dominio tienen acceso a ambos. Por ejemplo:

+ AEM se accede a través de: `https://wknd.site/`
+ Se accede a SPA a través de `https://wknd.site/spa`

Dado que se accede a AEM y a la SPA desde el mismo dominio, los navegadores web permiten que la SPA realice XHR para AEM puntos finales sin encabezado sin necesidad de CORS y permiten compartir cookies HTTP (como AEM `login-token` ).

La forma en que SPA y AEM tráfico se enrutan en el dominio compartido depende de usted: CDN con varios orígenes, servidor HTTP con proxy inverso, alojamiento del SPA directamente en AEM, etc.

A continuación se indican las configuraciones de implementación necesarias para SPA implementaciones de producción, cuando se alojan en el mismo dominio que AEM.

| SPA se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ü | š | š |
| Uso compartido de recursos de origen diverso (CORS) | ü | ü | ü |
| AEM hosts | ü | ü | ü |

### Dominios diferentes

Una SPA y AEM tienen dominios diferentes cuando los usuarios finales acceden a ellos desde los distintos dominios. Por ejemplo:

+ AEM se accede a través de: `https://wknd.site/`
+ Se accede a SPA a través de `https://wknd-app.site/`

Dado que se accede a AEM y a los SPA desde distintos dominios, los navegadores web aplican políticas de seguridad como [uso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md), e impida el uso compartido de cookies HTTP (como AEM `login-token` ).

A continuación se indican las configuraciones de implementación necesarias para SPA implementaciones de producción, cuando se alojan en un dominio diferente al AEM.

| SPA se conecta a | Autor de AEM | AEM Publish | Vista previa de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ü | š | š |
| [Uso compartido de recursos de origen diverso (CORS)](./configurations/cors.md) | š | š | š |
| [AEM hosts](./configurations/aem-hosts.md) | š | š | š |

#### Ejemplo SPA implementación en dominios diferentes

En este ejemplo, el SPA se implementa en un dominio Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) y el SPA consume AEM API de GraphQL del dominio de publicación de AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). Las siguientes capturas de pantalla resaltan el requisito de CORS.

1. El SPA se suministra desde un dominio Netlify, pero realiza una llamada XHR a las API de GraphQL AEM en un dominio diferente. Esta solicitud entre sitios requiere [CORS](./configurations/cors.md) que se debe configurar en AEM para permitir que la solicitud del dominio Netlify acceda a su contenido.

   ![SPA solicitud servida desde hosts SPA y AEM ](assets/spa/cors-requirement.png)

2. Inspeccionando la solicitud XHR a la API de AEM GraphQL, la variable `Access-Control-Allow-Origin` está presente, indicando al navegador web que AEM permite solicitar a este dominio Netlify el acceso a su contenido.

   Si el AEM [CORS](./configurations/cors.md) faltaba o no incluía el dominio Netlify , el explorador web fallaba en la solicitud XHR e informaba de un error CORS.

   ![Encabezado de respuesta CORS AEM API de GraphQL](assets/spa/cors-response-headers.png)

## Ejemplo de aplicación de una sola página

Adobe proporciona un ejemplo de aplicación de una sola página codificada en React.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="React app" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="React app">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="React app">React app</a></p>
               <p class="is-size-6">Una aplicación de una sola página de ejemplo, escrita en React, que consume contenido de AEM API de GraphQL sin encabezado.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>

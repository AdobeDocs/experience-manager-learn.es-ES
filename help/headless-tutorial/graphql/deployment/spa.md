---
title: SPA AEM Implementación de la implementación de para la implementación de GraphQL
description: SPA AEM Obtenga información sobre las consideraciones de implementación para implementaciones de una sola página () sin encabezado.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 1%

---

# AEM SPA Implementaciones de Headless

AEM SPA Las implementaciones de aplicación de una sola página () sin encabezado implican aplicaciones basadas en JavaScript AEM creadas con marcos de trabajo como React o Vue, que consumen contenido e interactúan con él de manera sin encabezado, y lo hacen de manera sin encabezado.

SPA AEM SPA La implementación de un que interactúe de manera sin encabezado implica alojar el y hacer que sea accesible a través de un explorador web.

## SPA Host de la

SPA Un recurso está formado por una colección de recursos web nativos: **HTML, CSS y JavaScript**. Estos recursos se generan durante el proceso _build_ (por ejemplo, `npm run build`) y se implementan en un host para que los consuman los usuarios finales.

Hay varias opciones de **alojamiento** según los requisitos de su organización:

1. **Proveedores de nube** como **Azure** o **AWS**.

2. **Alojamiento local** en un **centro de datos** corporativo

3. **Plataformas de alojamiento front-end** como **AWS Amplify**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel**, etc.

## Configuraciones de implementación

SPA AEM SPA AEM La consideración principal al alojar una que interactúa con sin encabezado es si se accede a la a través de un dominio (o host) de la aplicación o en un dominio diferente.  SPA El motivo es que las aplicaciones web se ejecutan en exploradores web y, por lo tanto, están sujetas a las políticas de seguridad de los exploradores web.

### Dominio compartido

SPA AEM Un dominio compartido de y cuando ambos son acceso para usuarios finales del mismo dominio. Por ejemplo:

+ AEM Se tiene acceso a la a través de: `https://wknd.site/`
+ SPA Se accede a la a través de `https://wknd.site/spa`

AEM SPA SPA AEM AEM Dado que se accede a las dos, la y la, desde el mismo dominio, los exploradores web permiten al usuario realizar XHR en puntos finales sin encabezado sin necesidad de CORS y permiten el uso compartido de cookies HTTP (como, por ejemplo, el uso compartido de cookies `login-token`), lo que permite a los exploradores web crear puntos finales sin encabezado, sin necesidad de usar CORS. El uso compartido de estas cookies se realiza de forma conjunta con los navegadores web, lo que permite el uso compartido de cookies HTTP (como, por ejemplo, el uso compartido de cookies de , de la).

SPA AEM SPA AEM Depende de usted cómo se enrute el tráfico de la y la en el dominio compartido: CDN con varios orígenes, servidor HTTP con proxy inverso, alojamiento de la directamente en el dominio compartido, etc.

SPA AEM A continuación, se indican las configuraciones de implementación necesarias para implementaciones de producción de la, cuando se hospedan en el mismo dominio que las implementaciones de.

| SPA se conecta a | AEM Author | Publicación de AEM | AEM Previsualización de |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Uso compartido de recursos de origen cruzado (CORS) | ✘ | ✘ | ✘ |
| AEM anfitriones de | ✘ | ✘ | ✘ |

### Dominios diferentes

SPA AEM Un y un dominio tienen dominios diferentes cuando los usuarios finales de un dominio diferente acceden a ellos. Por ejemplo:

+ AEM Se tiene acceso a la a través de: `https://wknd.site/`
+ SPA Se accede a la a través de `https://wknd-app.site/`

AEM SPA AEM Dado que se accede a los recursos de origen cruzado (CORS) [y a los dominios, los exploradores web aplican directivas de seguridad tales como el acceso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md), y evitan el uso compartido de cookies HTTP (como el uso compartido de cookies `login-token` por parte de los navegadores web).

SPA AEM A continuación, se indican las configuraciones de implementación necesarias para implementaciones de producción de la, cuando se aloja en un dominio diferente al de la ubicación de la aplicación.

| SPA se conecta a | AEM Author | Publicación de AEM | AEM Previsualización de |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Uso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| AEM [hosts de la](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### SPA Implementación de ejemplo en dominios diferentes

SPA SPA AEM En este ejemplo, el se implementa en un dominio de Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) y el consume las API de GraphQL AEM de los dominios de Publish (`https://publish-p65804-e666805.adobeaemcloud.com`) de la. Las siguientes capturas de pantalla resaltan el requisito CORS.

1. SPA AEM El se sirve desde un dominio de Netlify, pero realiza una llamada XHR a las API de GraphQL de la en un dominio diferente. AEM Esta solicitud entre sitios requiere que [CORS](./configurations/cors.md) se configure en el sitio para permitir que la solicitud del dominio de Netlify acceda a su contenido.

   SPA SPA AEM ![solicitud de servicio atendida desde los anfitriones de ](assets/spa/cors-requirement.png) de los hosts de y

2. AEM Al inspeccionar la solicitud XHR a la API de GraphQL AEM de la, está presente `Access-Control-Allow-Origin`, lo que indica al explorador web que la solicitud de este dominio de Netlify se permite para acceder a su contenido.

   AEM Si falta el elemento [CORS](./configurations/cors.md) o no incluye el dominio Netlify, el explorador web no responderá a la solicitud XHR e informará de un error CORS.

   AEM ![CORS Response Header (Encabezado de respuesta CORS), API de GraphQL](assets/spa/cors-response-headers.png)

## Ejemplo de aplicación de una sola página

El Adobe proporciona un ejemplo de aplicación de una sola página codificada en React.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="Aplicación React" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="Aplicación React">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="Aplicación React">Aplicación React</a></p>
               <p class="is-size-6">AEM Aplicación de ejemplo de una sola página, escrita en React, que consume contenido de las API de GraphQL sin encabezado, sin encabezado, que se utilizan para la creación de contenido.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
               </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="Aplicación Next.js" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="Aplicación Next.js">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="Aplicación Next.js">Aplicación Next.js</a></p>
               <p class="is-size-6">AEM Aplicación de ejemplo de una sola página, escrita en Next.js, que consume contenido de las API de GraphQL sin encabezado de la aplicación de la interfaz de usuario sin encabezado.</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>

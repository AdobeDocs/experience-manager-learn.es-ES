---
title: Implementación de SPA para AEM GraphQL
description: Obtenga información sobre las consideraciones de implementación para implementaciones sin encabezado de AEM de la aplicación de una sola página (SPA).
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 1%

---

# Implementaciones de SPA sin encabezado de AEM

Las implementaciones de aplicaciones de una sola página (SPA) sin encabezado de AEM implican aplicaciones basadas en JavaScript creadas con marcos de trabajo como React o Vue, que consumen contenido en AEM e interactúan con él sin encabezado.

La implementación de una SPA que interactúe con AEM de forma descentralizada implica alojar la SPA y hacerla accesible a través de un explorador web.

## Alojar la SPA

Un SPA consta de una colección de recursos web nativos: **HTML, CSS y JavaScript**. Estos recursos se generan durante el proceso _build_ (por ejemplo, `npm run build`) y se implementan en un host para que los consuman los usuarios finales.

Hay varias opciones de **alojamiento** según los requisitos de su organización:

1. **Proveedores de nube** como **Azure** o **AWS**.

2. **Alojamiento local** en un **centro de datos** corporativo

3. **Plataformas de alojamiento front-end** como **AWS Amplify**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel**, etc.

## Configuraciones de implementación

La consideración principal al alojar un SPA que interactúa con AEM sin encabezado es si se accede al SPA a través del dominio (o host) de AEM o en un dominio diferente.  El motivo es que las SPA son aplicaciones web que se ejecutan en exploradores web y, por lo tanto, están sujetas a las políticas de seguridad de los exploradores web.

### Dominio compartido

Una SPA y AEM comparten dominios cuando ambos son accesibles para los usuarios finales desde el mismo dominio. Por ejemplo:

+ Se tiene acceso a AEM a través de: `https://wknd.site/`
+ Se accede a la SPA a través de `https://wknd.site/spa`

Dado que se accede a AEM y a la SPA desde el mismo dominio, los exploradores web permiten a la SPA realizar XHR en puntos finales sin encabezado de AEM sin necesidad de CORS y permiten el uso compartido de cookies HTTP (como la cookie `login-token` de AEM).

Depende de usted cómo se enrute el tráfico de SPA y AEM en el dominio compartido: CDN con varios orígenes, servidor HTTP con proxy inverso, alojamiento del SPA directamente en AEM, etc.

A continuación se indican las configuraciones de implementación necesarias para las implementaciones de producción de SPA, cuando se alojan en el mismo dominio que AEM.

| SPA se conecta a → | AEM Author | Publicación de AEM | Previsualización de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Uso compartido de recursos de origen cruzado (CORS) | ✘ | ✘ | ✘ |
| Hosts de AEM | ✘ | ✘ | ✘ |

### Dominios diferentes

Una SPA y AEM tienen dominios diferentes cuando los usuarios finales de los distintos dominios acceden a ellos. Por ejemplo:

+ Se tiene acceso a AEM a través de: `https://wknd.site/`
+ Se accede a la SPA a través de `https://wknd-app.site/`

Dado que se accede a AEM y a la SPA desde dominios diferentes, los exploradores web aplican directivas de seguridad como [uso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md) e impiden el uso compartido de cookies HTTP (como la cookie `login-token` de AEM).

A continuación se indican las configuraciones de implementación necesarias para las implementaciones de producción de SPA, cuando se alojan en un dominio diferente al de AEM.

| SPA se conecta a → | AEM Author | Publicación de AEM | Previsualización de AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtros de Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Uso compartido de recursos de origen cruzado (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hosts de AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Ejemplo de implementación de SPA en dominios diferentes

En este ejemplo, la SPA se implementa en un dominio de Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) y la SPA consume las API de AEM GraphQL desde el dominio de publicación de AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). Las siguientes capturas de pantalla resaltan el requisito CORS.

1. La SPA se sirve desde un dominio de Netlify, pero realiza una llamada XHR a las API de AEM GraphQL en un dominio diferente. Esta solicitud entre sitios requiere que [CORS](./configurations/cors.md) se configure en AEM para permitir que la solicitud del dominio de Netlify acceda a su contenido.

   ![Solicitud de SPA atendida desde los hosts de SPA y AEM ](assets/spa/cors-requirement.png)

2. Al inspeccionar la solicitud XHR a la API de AEM GraphQL, está presente `Access-Control-Allow-Origin`, lo que indica al explorador web que AEM permite que la solicitud de este dominio de Netlify acceda a su contenido.

   Si faltaba el AEM [CORS](./configurations/cors.md) o no incluía el dominio Netlify, el navegador web daría error a la solicitud XHR e informaría de un error CORS.

   ![API GraphQL de AEM de encabezado de respuesta CORS](assets/spa/cors-response-headers.png)

## Ejemplo de aplicación de una sola página

Adobe proporciona una aplicación de una sola página de ejemplo codificada en React.

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
               <p class="is-size-6">Aplicación de ejemplo de una sola página, escrita en React, que consume contenido de las API de GraphQL sin encabezado de AEM.</p>
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
               <p class="is-size-6">Aplicación de ejemplo de una sola página, escrita en Next.js, que consume contenido de las API de GraphQL sin encabezado de AEM.</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ver ejemplo</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>

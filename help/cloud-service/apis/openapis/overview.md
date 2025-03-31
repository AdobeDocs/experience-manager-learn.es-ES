---
title: API de AEM basadas en OpenAPI
description: Obtenga información acerca de las API de AEM basadas en OpenAPI, incluida la compatibilidad con la autenticación, los conceptos clave y cómo acceder a las API de Adobe.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 34aaecb7b82d7fae068549fad3ec9a4895fb9ec7
workflow-type: tm+mt
source-wordcount: '1015'
ht-degree: 0%

---

# API de AEM basadas en OpenAPI

>[!IMPORTANT]
>
>Las API de AEM basadas en OpenAPI solo están disponibles en AEM as a Cloud Service y no son compatibles con AEM 6.X.

Obtenga información acerca de las API de AEM basadas en OpenAPI, incluida la compatibilidad con la autenticación, los conceptos clave y cómo acceder a las API de Adobe.

[Especificación de OpenAPI](https://swagger.io/specification/) (anteriormente conocida como Swagger) es un estándar ampliamente utilizado para definir las API de RESTful. AEM as a Cloud Service proporciona varias API basadas en la especificación OpenAPI (o simplemente API de AEM basadas en OpenAPI), lo que facilita la creación de aplicaciones personalizadas que interactúen con los tipos de servicio de autor o publicación de AEM. A continuación se muestran algunos ejemplos:

**Sitios**

- [API de sitios](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/): API para trabajar con fragmentos de contenido.

**Assets**

- [API de carpetas](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): API para trabajar con carpetas, como crear, mostrar y eliminar carpetas.

- [API de autor de Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): API para trabajar con recursos y sus metadatos.

**Forms**

- [API de comunicaciones de Forms](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): API para trabajar con formularios y documentos.

En futuras versiones, se agregarán más API de AEM basadas en OpenAPI para admitir casos de uso adicionales.

>[!AVAILABILITY]
>
>Las API de AEM basadas en API abiertas están disponibles como parte de un programa de acceso anticipado. Si está interesado en acceder a ellos, le recomendamos que envíe un correo electrónico a [aem-apis@adobe.com](mailto:aem-apis@adobe.com) con una descripción de su caso de uso.

## Compatibilidad con autenticación{#authentication-support}

Las API de AEM basadas en OpenAPI admiten la autenticación OAuth 2.0, incluidos los siguientes tipos de concesión:

- **Credencial de servidor a servidor OAuth**: ideal para servicios back-end que necesitan acceso a API sin interacción del usuario. Utiliza el tipo de concesión _client_credentials_, lo que permite la administración segura del acceso en el nivel de servidor. Para obtener más información, consulte [Credencial de servidor a servidor de OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Credencial de la aplicación web OAuth**: Adecuado para aplicaciones web con componentes de front-end y back-end _3} que acceden a las API de AEM en nombre de los usuarios._ Utiliza el tipo de concesión _authorization_code_, donde el servidor backend administra secretos y tokens de forma segura. Para obtener más información, consulte [Credencial de la aplicación web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Credencial de aplicación de una sola página de OAuth**: diseñada para las SPA que se ejecutan en el explorador y que necesitan acceder a las API en nombre de un usuario sin un servidor back-end. Utiliza el tipo de concesión _authorization_code_ y se basa en los mecanismos de seguridad del lado del cliente mediante PKCE (clave de prueba para intercambio de código) para proteger el flujo del código de autorización. Para obtener más información, consulte [Credencial de aplicación de una sola página de OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Diferencia entre OAuth servidor a servidor frente a aplicación web frente a credenciales de aplicación de una sola página{#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials}

La siguiente tabla resume las diferencias entre los tres métodos de autenticación OAuth admitidos por las API de AEM basadas en OpenAPI:

|  | Servidor a servidor OAuth | Aplicación web de OAuth | Aplicación de una sola página (SPA) de OAuth |
| --- | --- | --- | --- |
| **Propósito de autenticación** | Diseñado para interacciones máquina a máquina. | Diseñado para interacciones impulsadas por el usuario en una aplicación web con _backend_. | Diseñado para interacciones impulsadas por el usuario en una _aplicación JavaScript del lado del cliente_. |
| **Comportamiento del token** | Emite tokens de acceso que representan la propia aplicación cliente. | Emite tokens de acceso en nombre de un usuario autenticado _a través de un backend_. | Emite tokens de acceso en nombre de un usuario autenticado _a través de un flujo solo de front-end_. |
| **Casos de uso** | Servicios back-end que necesitan acceso a API sin interacción del usuario. | Las aplicaciones web con componentes de front-end y back-end acceden a las API en nombre de los usuarios. | Aplicaciones de front-end puras (JavaScript) que acceden a las API en nombre de usuarios sin back-end. |
| **Consideraciones de seguridad** | Almacene de forma segura credenciales confidenciales (`client_id`, `client_secret`) en sistemas back-end. | Después de la autenticación del usuario, se le concede su propio _token de acceso temporal a través de una llamada back-end_. Almacene de forma segura credenciales confidenciales (`client_id`, `client_secret`) en sistemas back-end para intercambiar código de autorización por token de acceso. | Después de la autenticación del usuario, se le concede su propio _token de acceso temporal a través de una llamada de front-end_. No utiliza `client_secret`, ya que no es seguro almacenarlo en aplicaciones de front-end. Se basa en PKCE para intercambiar el código de autorización por el token de acceso. |
| **Tipo de concesión** | _client_credentials_ | _código de autorización_ | _authorization_code_ con **PKCE** |
| **Tipo de credencial de Adobe Developer Console** | Servidor a servidor OAuth | Aplicación web de OAuth | Aplicación de una sola página OAuth |

## Acceso a las API de Adobe y conceptos relacionados{#accessing-adobe-apis-and-related-concepts}

Antes de acceder a las API de Adobe, es esencial comprender estas construcciones clave:

- **[Adobe Developer Console](https://developer.adobe.com/)**: el centro para desarrolladores para acceder a las API de Adobe, SDK, eventos en tiempo real, funciones sin servidor y mucho más. Tenga en cuenta que es diferente del Developer Console _AEM_, que se usa para depurar aplicaciones de AEM.

- **[Proyecto Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)**: Lugar central para administrar integraciones de API, eventos y funciones de tiempo de ejecución. Aquí puede configurar las API, establecer la autenticación y generar las credenciales necesarias.

- **[Perfiles de producto](https://helpx.adobe.com/es/enterprise/using/support-for-experience-cloud.html?lang=es)**: los perfiles de producto proporcionan un ajuste preestablecido de permisos que le permite controlar el acceso de usuarios o aplicaciones a productos de Adobe como AEM, Adobe Target, Adobe Analytics y otros. Todos los productos de Adobe tienen perfiles de producto predefinidos asociados a ellos.

- **Servicios**: los servicios definen los permisos reales y están asociados al perfil del producto. Para reducir o aumentar el ajuste preestablecido de permisos, puede anular la selección de los servicios asociados con el perfil del producto o seleccionarlos. Por lo tanto, le permite controlar el nivel de acceso al producto y a sus API. En AEM as a Cloud Service, los servicios representan grupos de usuarios con Listas de control de acceso (ACL) predefinidas para nodos del repositorio, lo que permite la administración granular de permisos.

## Introducción

Obtenga información sobre cómo configurar su entorno de AEM as a Cloud Service y un proyecto de Adobe Developer Console para habilitar el acceso a las API de AEM basadas en OpenAPI. También puede acceder a la API de AEM mediante el explorador para comprobar la configuración y revisar la solicitud y la respuesta.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Configurar las API de AEM basadas en OpenAPI" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="Configurar las API de AEM basadas en OpenAPI"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Configurar las API de AEM basadas en OpenAPI">Configurar las API de AEM basadas en OpenAPI</a>
                    </p>
                    <p class="is-size-6">Aprenda a configurar su entorno de AEM as a Cloud Service para habilitar el acceso a las API de AEM basadas en OpenAPI.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Tutoriales de API

Aprenda a utilizar las API de AEM basadas en OpenAPI utilizando diferentes métodos de autenticación de OAuth:

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth Single Page App authentication.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="Invocar la API mediante la autenticación de servidor a servidor" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="Invocar la API mediante la autenticación de servidor a servidor"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Invocar la API mediante la autenticación de servidor a servidor">Invocar API mediante autenticación de servidor a servidor</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo invocar las API de AEM basadas en OpenAPI desde una aplicación NodeJS personalizada mediante la autenticación de servidor a servidor OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="Invocar la API mediante la autenticación de aplicación web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="Invocar la API mediante la autenticación de aplicación web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Invocar la API mediante la autenticación de aplicación web">Invocar API mediante autenticación de aplicación web</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo invocar las API de AEM basadas en OpenAPI desde una aplicación web personalizada mediante la autenticación de aplicación web de OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="Invocar la API mediante la autenticación de aplicación de una sola página" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="Invocar la API mediante la autenticación de aplicación de una sola página"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="Invocar la API mediante la autenticación de aplicación de una sola página">Invocar API mediante autenticación de aplicación de una sola página</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo invocar las API de AEM basadas en OpenAPI desde una aplicación de una sola página (SPA) personalizada mediante la autenticación de aplicación de una sola página de OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

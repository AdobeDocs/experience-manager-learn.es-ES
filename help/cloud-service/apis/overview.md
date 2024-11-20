---
title: AEM Información general de APIs
description: Obtenga información acerca de los distintos tipos de API en Adobe Experience Manager AEM AEM () y obtenga una descripción general de las API basadas en la especificación OpenAPI, comúnmente conocidas como API basadas en OpenAPI.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 316e08e6647d6fd731cd49ae1bc139ce57c3a7f4
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 1%

---

# AEM Información general de APIs{#aem-apis-overview}

Obtenga información acerca de los diferentes tipos de API en Adobe Experience Manager AEM () as a Cloud Service AEM AEM y obtenga información general sobre las [API de OpenAPI Specification (OAS)](https://swagger.io/specification/) basadas en API de, comúnmente conocidas como API de API basadas en OpenAPI.

AEM as a Cloud Service proporciona una amplia gama de API para crear, leer, actualizar y eliminar contenido, recursos y formularios. AEM Estas API permiten a los desarrolladores crear aplicaciones personalizadas que interactúen con las API de.

Exploremos los diferentes tipos de API en el ámbito de las API de Adobe y comprendamos los conceptos clave de acceso a las API de AEM.

## AEM Tipos de API de{#types-of-aem-apis}

AEM ofrece API heredadas y modernas para interactuar con sus tipos de servicio de autor y publicación.

- AEM **API heredadas**: introducidas en versiones anteriores de la aplicación, las API heredadas siguen siendo compatibles con versiones anteriores por motivos de compatibilidad con versiones anteriores.

- **API modernas**: Estas API siguen las prácticas recomendadas actuales de diseño de API y se recomiendan para nuevas integraciones, según las especificaciones de REST y OpenAPI.


| AEM Tipo de API de | Especificaciones | Disponibilidad | Caso práctico | Ejemplos |
| --- | --- | --- | --- | --- |
| API tradicionales (no RESTful) | Sling Servlets | AEM.X, AEM as a Cloud Service | Integraciones heredadas, compatibilidad con versiones anteriores | [API de Query Builder](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) y otras |
| API de RESTful | HTTP, JSON | AEM.X, AEM as a Cloud Service | Operaciones CRUD, aplicaciones modernas | [API HTTP de Assets](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [API REST de flujo de trabajo](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [exportador JSON para servicios de contenido](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) y otros |
| API de GraphQL | GraphQL | AEM.X, AEM as a Cloud Service | CMS SPA sin encabezado, aplicaciones móviles, aplicaciones para móviles | [API de GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| AEM API de datos basadas en API de | REST, OpenAPI | **Solo AEM as a Cloud Service** | Desarrollo con API en primer lugar, aplicaciones modernas | [API de autor de Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [API de carpetas](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [API de AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Servicios de Forms Acrobat](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) y otros |

>[!IMPORTANT]
>
>AEM Las API de basadas en OpenAPI solo están disponibles en AEM as a Cloud Service AEM y no son compatibles con la versión 6.X de.

AEM Para obtener más información sobre las API de, consulte las [API de Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

AEM Veamos con más detalle las API de API de basadas en OpenAPI y los conceptos importantes de acceso a las API de Adobe.

## AEM API de datos basadas en API de{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>AEM Las API de basadas en OpenAPI están disponibles como parte de un programa de acceso anticipado. Si está interesado en acceder a ellos, le recomendamos que envíe un correo electrónico a [aem-apis@adobe.com](mailto:aem-apis@adobe.com) con una descripción de su caso de uso.

[Especificación de OpenAPI](https://swagger.io/specification/) (anteriormente conocida como Swagger) es un estándar ampliamente utilizado para definir las API de RESTful. AEM as a Cloud Service proporciona varias API basadas en la especificación OpenAPI (o simplemente API basadas en OpenAPI), lo que facilita la creación de aplicaciones personalizadas que interactúen con los tipos de servicio de autor o publicación de la publicación de los que se dispone en la documentación de AEM AEM. A continuación se muestran algunos ejemplos:

**Sitios**

- [API de sitios](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/): API para trabajar con fragmentos de contenido.

**Assets**

- [API de carpetas](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): API para trabajar con carpetas, como crear, mostrar y eliminar carpetas.

- [API de autor de Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): API para trabajar con recursos y sus metadatos.

**Forms**

- [API de comunicaciones de Forms](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): API para trabajar con formularios y documentos.

AEM En futuras versiones, se añadirán más API de API de basadas en OpenAPI para admitir casos de uso adicionales.

## Compatibilidad con autenticación{#authentication-support}

AEM Las API de basadas en OpenAPI admiten los siguientes métodos de autenticación:

- **Credencial de servidor a servidor OAuth**: ideal para servicios back-end que necesitan acceso a API sin interacción del usuario. Utiliza el tipo de concesión _client_credentials_, lo que permite la administración segura del acceso en el nivel de servidor. Para obtener más información, consulte [Credencial de servidor a servidor de OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- AEM **Credencial de la aplicación web OAuth**: Adecuado para aplicaciones web con componentes de front-end y _back-end_ que acceden a las API de en nombre de los usuarios. Utiliza el tipo de concesión _authorization_code_, donde el servidor backend administra secretos y tokens de forma segura. Para obtener más información, consulte [Credencial de la aplicación web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- SPA **Credencial de aplicación de una sola página de OAuth**: diseñada para ejecutarse en el explorador, que necesita acceder a las API en nombre de un usuario sin un servidor back-end. Utiliza el tipo de concesión _authorization_code_ y se basa en los mecanismos de seguridad del lado del cliente mediante PKCE (clave de prueba para intercambio de código) para proteger el flujo del código de autorización. Para obtener más información, consulte [Credencial de aplicación de una sola página de OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Acceso a las API de Adobe y conceptos relacionados{#accessing-adobe-apis-and-related-concepts}

Antes de acceder a las API de Adobe, es esencial comprender estos conceptos clave:

- **[Adobe Developer Console](https://developer.adobe.com/)**: el centro para desarrolladores para acceder a las API de Adobe, SDK, eventos en tiempo real, funciones sin servidor y mucho más. AEM Tenga en cuenta que es diferente del Developer Console AEM __, que se usa para depurar aplicaciones de la.

- **[Proyecto Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)**: Lugar central para administrar integraciones de API, eventos y funciones de tiempo de ejecución. Aquí puede configurar las API, establecer la autenticación y generar las credenciales necesarias.

- **[Perfiles de producto](https://helpx.adobe.com/es/enterprise/using/support-for-experience-cloud.html?lang=es)**: los perfiles de producto proporcionan un ajuste preestablecido de permisos que le permite controlar el acceso de usuarios o aplicaciones a productos de Adobe AEM como, por ejemplo,, Adobe Target, Adobe Analytics y otros. Cada producto de Adobe tiene perfiles de producto predefinidos asociados a él.

- **Servicios**: los servicios definen los permisos reales y están asociados al perfil del producto. Para reducir o aumentar el ajuste preestablecido de permisos, puede anular la selección de los servicios asociados con el perfil del producto o seleccionarlos. Por lo tanto, le permite controlar el nivel de acceso al producto y a sus API. En AEM as a Cloud Service, los servicios representan grupos de usuarios con Listas de control de acceso (ACL) predefinidas para nodos del repositorio, lo que permite la administración granular de permisos.

## Pasos siguientes{#next-steps}

AEM Con una comprensión de los diferentes tipos de API de, que incluyen
AEM Las API basadas en API abiertas de, y los conceptos clave para acceder a las API de Adobe AEM de, ya están listos para empezar a crear aplicaciones personalizadas que interactúen con las API de.

AEM Empecemos con el tutorial [Cómo invocar las API basadas en OpenAPIs](invoke-openapi-based-aem-apis.md) de las API de la aplicación de código abierto.

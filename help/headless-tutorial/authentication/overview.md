---
title: Autenticación en AEM as a Cloud Service desde una aplicación externa
description: Explore cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM as a Cloud Service a través de HTTP mediante tókenes de acceso de desarrollo local y credenciales de servicio.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: ht
source-wordcount: '621'
ht-degree: 100%

---

# Autenticación basada en tókenes en AEM as a Cloud Service

AEM expone una variedad de puntos finales HTTP con los que se puede interactuar sin encabezado, desde GraphQL, AEM Content Services o la API HTTP de Assets. A menudo, estos consumidores sin encabezado pueden necesitar autenticarse en AEM para acceder a contenido o acciones protegidos. Para facilitar esto, AEM admite la autenticación basada en tókenes de solicitudes HTTP desde aplicaciones, servicios o sistemas externos.

En este tutorial, exploraremos cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM as a Cloud Service a través de HTTP mediante tókenes de acceso.

>[!VIDEO](https://video.tv.adobe.com/v/3410079?quality=12&learn=on&captions=spa)

## Requisitos previos

Asegúrese de que cumple los siguientes requisitos antes de seguir este tutorial:

1. Acceso a un entorno de AEM as a Cloud Service (preferiblemente, un entorno de desarrollo o un programa de zona protegida)
1. Abono en el perfil de producto del administrador de AEM a los servicios de Author del entorno de AEM as a Cloud Service. 
1. Ser miembro o tener acceso a su administrador de IMS de Adobe Org (deberá realizar una inicialización única de las [Credenciales de servicio](./service-credentials.md))
1. El [sitio WKND]( https://github.com/adobe/aem-guides-wknd) más reciente implementado en su entorno Cloud Service

## Información general sobre la aplicación externa

Este tutorial utiliza una [aplicación Node.js simple](./assets/aem-guides_token-authentication-external-application.zip) que se ejecuta desde la línea de comandos para actualizar los metadatos de los recursos en AEM as a Cloud Service mediante la [API HTTP de Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=es).

El flujo de ejecución de la aplicación Node.js es el siguiente:

![Aplicación externa](./assets/overview/external-application.png)

1. La aplicación Node.js se invoca desde la línea de comandos
1. Los parámetros de línea de comandos definen lo siguiente:
   + El host del servicio de Author de AEM as a Cloud Service al que conectarse (`aem`)
   + La carpeta de recursos de AEM cuyos recursos se han actualizado (`folder`)
   + La propiedad y el valor de los metadatos que se van a actualizar (`propertyName` y `propertyValue`)
   + La ruta de acceso local al archivo que proporciona las credenciales necesarias para obtener acceso a AEM as a Cloud Service (`file`)
1. El token de acceso que se utiliza para autenticarse en AEM deriva del archivo JSON proporcionado mediante el parámetro de línea de comandos `file`

   a. Si las credenciales de servicio utilizadas para el desarrollo no local se proporcionan en el archivo JSON (`file`), el token de acceso se recupera de las API de IMS de Adobe
1. La aplicación utiliza el token de acceso para acceder a AEM y enumerar todos los recursos de la carpeta especificada en el parámetro de línea de comandos `folder`
1. Para cada recurso de la carpeta, la aplicación actualiza sus metadatos en función del nombre de propiedad y el valor especificados en los parámetros de línea de comandos `propertyName` y `propertyValue`

Aunque esta aplicación de ejemplo es Node.js, estas interacciones se pueden desarrollar utilizando diferentes lenguajes de programación y ejecutarse desde otros sistemas externos.

## Token de acceso de desarrollo local

Los tókenes de acceso de desarrollo local se generan para un entorno de AEM as a Cloud Service específico y proporcionan acceso a los servicios de creación y publicación.  Estos tókenes de acceso son temporales y solo se utilizan durante el desarrollo de aplicaciones o sistemas externos que interactúan con AEM a través de HTTP. En lugar de tener que obtener y administrar credenciales de servicio auténticas, un desarrollador puede generar de forma rápida y sencilla un token de acceso temporal que le permita desarrollar su integración.

+ [Cómo utilizar el token de acceso de desarrollo local](./local-development-access-token.md)

## Credenciales de servicio

Las credenciales de servicio son credenciales auténticas que se utilizan en cualquier escenario que no sea de desarrollo (el más obvio es el de producción) y facilitan la posibilidad de que una aplicación o un sistema externo se autentique e interactúe con AEM as a Cloud Service a través de HTTP. Las credenciales del servicio en sí no se envían a AEM para la autenticación, sino que la aplicación externa las utiliza para generar un JWT, que luego se intercambia con las API de IMS de Adobe _por_ un token de acceso, y se puede utilizar para autenticar solicitudes HTTP a AEM as a Cloud Service.

+ [Uso de las credenciales de servicio](./service-credentials.md)

## Recursos adicionales

+ [Descargar la aplicación de ejemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Otros ejemplos de código de creación e intercambio de JWT
   + [Ejemplos de código de Node.js, Java, Python, C#.NET y PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)
   + [Ejemplo de código basado en JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)

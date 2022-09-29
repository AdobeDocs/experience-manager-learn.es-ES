---
title: Autenticar con AEM as a Cloud Service desde una aplicación externa
description: Explore cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM as a Cloud Service a través de HTTP mediante Tokens de acceso de desarrollo local y credenciales de servicio.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 1%

---

# Autenticación basada en token para AEM as a Cloud Service

AEM expone una variedad de extremos HTTP con los que se puede interactuar de forma directa, desde GraphQL, AEM Content Services a la API HTTP de Assets. A menudo, estos consumidores sin encabezado pueden tener que autenticarse en AEM para acceder a contenido o acciones protegidos. Para facilitarlo, AEM admite la autenticación basada en token de solicitudes HTTP de aplicaciones, servicios o sistemas externos.

En este tutorial, explore bien cómo una aplicación externa puede autenticarse mediante programación e interactuar con para AEM as a Cloud Service a través de HTTP mediante tokens de acceso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Requisitos previos

Asegúrese de que las siguientes opciones están implementadas antes de seguir este tutorial junto con este tutorial:

1. Acceso a AEM entorno as a Cloud Service (preferiblemente un entorno de desarrollo o un programa de espacio aislado)
1. Pertenencia a los servicios de creación del entorno as a Cloud Service AEM AEM Perfil de producto del administrador
1. Pertenencia a su administrador de organización de Adobe IMS o acceso a él (deberán realizar una inicialización única de la variable [Credenciales de servicio](./service-credentials.md))
1. La última [Sitio WKND](https://github.com/adobe/aem-guides-wknd) implementado en su entorno de Cloud Service

## Información general de aplicaciones externas

Este tutorial utiliza un [aplicación Node.js simple](./assets/aem-guides_token-authentication-external-application.zip) ejecute desde la línea de comandos para actualizar los metadatos de recursos en AEM as a Cloud Service mediante [API HTTP de recursos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

El flujo de ejecución de la aplicación Node.js es el siguiente:

![Aplicación externa](./assets/overview/external-application.png)

1. La aplicación Node.js se invoca desde la línea de comandos
1. Los parámetros de la línea de comandos definen:
   + El host del servicio de creación as a Cloud Service AEM para conectarse a (`aem`)
   + La carpeta de recursos AEM cuyos recursos se actualizan (`folder`)
   + La propiedad de metadatos y el valor que se va a actualizar (`propertyName` y `propertyValue`)
   + La ruta local al archivo que proporciona las credenciales necesarias para acceder a AEM as a Cloud Service (`file`)
1. El token de acceso utilizado para autenticarse en AEM se deriva del archivo JSON proporcionado mediante el parámetro de línea de comandos `file`

   a. Si las credenciales de servicio utilizadas para el desarrollo no local se proporcionan en el archivo JSON (`file`), el token de acceso se recupera de las API de Adobe IMS
1. La aplicación utiliza el token de acceso para acceder a AEM y mostrar todos los recursos de la carpeta especificada en el parámetro de línea de comandos `folder`
1. Para cada recurso de la carpeta, la aplicación actualiza sus metadatos en función del nombre de propiedad y el valor especificados en los parámetros de la línea de comandos `propertyName` y `propertyValue`

Aunque esta aplicación de ejemplo es Node.js, estas interacciones pueden desarrollarse con diferentes lenguajes de programación y ejecutarse desde otros sistemas externos.

## Token de acceso de desarrollo local

Los tokens de acceso de desarrollo local se generan para un entorno as a Cloud Service AEM específico y proporcionan acceso a los servicios de autor y publicación.  Estos tokens de acceso son temporales y solo deben utilizarse durante el desarrollo de aplicaciones o sistemas externos que interactúan con AEM a través de HTTP. En lugar de que un desarrollador tenga que obtener y administrar credenciales de servicio de bonafide, puede generar rápida y fácilmente un token de acceso temporal que le permita desarrollar su integración.

+ [Cómo usar el token de acceso de desarrollo local](./local-development-access-token.md)

## Credenciales de servicio

Las Credenciales de Servicio son las credenciales valiosas utilizadas en cualquier escenario que no sea de desarrollo (lo que es más obvio es la producción) que facilitan la capacidad de una aplicación externa o del sistema para autenticarse e interactuar con AEM as a Cloud Service a través de HTTP. Las propias credenciales de servicio no se envían a AEM para su autenticación, sino que la aplicación externa las utiliza para generar un JWT, que se intercambia con las API de Adobe IMS _para_ un token de acceso, que se puede utilizar para autenticar solicitudes HTTP AEM as a Cloud Service.

+ [Cómo utilizar las credenciales del servicio](./service-credentials.md)

## Recursos adicionales

+ [Descargue la aplicación de ejemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Otras muestras de código de creación e intercambio de JWT
   + [Ejemplos de código de Node.js, Java, Python, C#.NET y PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Ejemplo de código basado en JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)

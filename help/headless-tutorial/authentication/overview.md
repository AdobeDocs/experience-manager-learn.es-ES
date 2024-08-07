---
title: Autenticación en AEM as a Cloud Service desde una aplicación externa
description: Explore cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM as a Cloud Service a través de HTTP mediante tokens de acceso de desarrollo local y credenciales de servicio.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 0%

---

# Autenticación basada en tokens en AEM as a Cloud Service

AEM EXPONE una variedad de puntos de conexión HTTP con los que se puede interactuar sin encabezado, desde GraphQL AEM, Servicios de contenido de API HTTP de Assets. AEM A menudo, estos consumidores sin encabezado pueden necesitar autenticarse para acceder a las acciones o al contenido protegido con el fin de obtener acceso a las acciones de los que se puede acceder. AEM Para facilitar esto, admite la autenticación basada en token de solicitudes HTTP de aplicaciones, servicios o sistemas externos.

En este tutorial, exploraremos cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM as a Cloud Service a través de HTTP mediante tokens de acceso.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Requisitos previos

Asegúrese de que las siguientes opciones estén implementadas antes de seguir este tutorial:

1. Acceso al entorno de AEM AEM as a Cloud Service (preferiblemente, un entorno de desarrollo o un programa de zona protegida)
1. Pertenencia a los servicios de autor del entorno de AEM as a Cloud ServiceAEM Perfil de producto del administrador
1. Pertenencia a su administrador de organización de IMS de Adobe o acceso a él (tendrán que realizar una inicialización única de [Credenciales de servicio](./service-credentials.md))
1. El [sitio WKND](https://github.com/adobe/aem-guides-wknd) más reciente implementado en su entorno de Cloud Service

## Introducción a la aplicación externa

Este tutorial utiliza una [aplicación Node.js simple](./assets/aem-guides_token-authentication-external-application.zip) que se ejecuta desde la línea de comandos para actualizar los metadatos de los recursos en AEM as a Cloud Service mediante la [API HTTP de Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

El flujo de ejecución de la aplicación Node.js es el siguiente:

![Aplicación externa](./assets/overview/external-application.png)

1. La aplicación Node.js se invoca desde la línea de comandos
1. Los parámetros de línea de comandos definen:
   + El host del servicio de autor de AEM as a Cloud Service al que conectarse (`aem`)
   + AEM La carpeta de recursos de la carpeta de recursos de la que se actualizaron (`folder`)
   + La propiedad y el valor de los metadatos que se van a actualizar (`propertyName` y `propertyValue`)
   + Ruta de acceso local al archivo que proporciona las credenciales necesarias para obtener acceso a AEM as a Cloud Service (`file`)
1. AEM El token de acceso utilizado para autenticarse en la autenticación se deriva del archivo JSON proporcionado mediante el parámetro de línea de comandos `file`

   a. Si las credenciales de servicio utilizadas para el desarrollo no local se proporcionan en el archivo JSON (`file`), el token de acceso se recupera de las API de IMS de Adobe
1. AEM La aplicación utiliza el token de acceso para acceder a todos los recursos de la carpeta especificada en el parámetro de línea de comandos `folder` y obtener acceso a ellos, así como para realizar una lista de los mismos
1. Para cada recurso de la carpeta, la aplicación actualiza sus metadatos en función del nombre de propiedad y el valor especificados en los parámetros de línea de comandos `propertyName` y `propertyValue`

Aunque esta aplicación de ejemplo es Node.js, estas interacciones se pueden desarrollar utilizando diferentes lenguajes de programación y ejecutarse desde otros sistemas externos.

## Token de acceso de desarrollo local

Los tokens de acceso de desarrollo local se generan para un entorno de AEM as a Cloud Service específico y proporcionan acceso a los servicios de Author y Publish.  AEM Estos tokens de acceso son temporales y solo se utilizan durante el desarrollo de aplicaciones o sistemas externos que interactúan con el servidor de correo electrónico a través de HTTP. Los tokens de acceso son temporales y solo se utilizan para el desarrollo de aplicaciones o sistemas externos que interactúan con el servidor de correo electrónico a través de HTTP. En lugar de que un desarrollador tenga que obtener y administrar credenciales de servicio de confianza, puede generar de forma rápida y sencilla un token de acceso temporal que les permita desarrollar su integración.

+ [Cómo utilizar el token de acceso de desarrollo local](./local-development-access-token.md)

## Credenciales del servicio

Las credenciales de servicio son las credenciales de confianza utilizadas en cualquier escenario sin desarrollo (y, obviamente, en la producción) que facilitan la capacidad de una aplicación externa o del sistema para autenticarse en AEM as a Cloud Service a través de HTTP e interactuar con él. AEM Las credenciales del servicio en sí no se envían a los usuarios para su autenticación, sino que la aplicación externa las utiliza para generar un JWT, que se intercambia con las API de IMS de Adobe _for_, y un token de acceso, que luego se puede utilizar para autenticar solicitudes HTTP a AEM as a Cloud Service.

+ [Cómo usar las credenciales del servicio](./service-credentials.md)

## Recursos adicionales

+ [Descargue la aplicación de ejemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Otros ejemplos de código de creación e intercambio de JWT
   + [Ejemplos de código de Node.js, Java, Python, C#.NET y PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [Ejemplo de código basado en JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)

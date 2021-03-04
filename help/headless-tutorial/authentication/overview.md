---
title: Autenticación a AEM as a Cloud Service desde una aplicación externa
description: Explore cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM as a Cloud Service a través de HTTP mediante los tokens de acceso de desarrollo local y las credenciales de servicio.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---


# Autenticación basada en tokens para AEM as a Cloud Service

En este tutorial, explore bien cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM as a Cloud Service a través de HTTP mediante tokens de acceso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Requisitos previos

Asegúrese de que las siguientes opciones están implementadas antes de seguir este tutorial junto con este tutorial:

1. Acceso a AEM as a Cloud Service environment (preferiblemente un entorno de desarrollo o un programa de espacio aislado)
1. Pertenencia a los servicios de autor del entorno de AEM as a Cloud Service Perfil de producto del administrador de AEM
1. Membresía o acceso a su administrador de organización de IMS de Adobe (deberán realizar una inicialización única de las [Credenciales de servicio](./service-credentials.md))
1. El [sitio WKND](https://github.com/adobe/aem-guides-wknd) más reciente implementado en el entorno de Cloud Service

## Información general de aplicaciones externas

Este tutorial utiliza una [aplicación Node.js simple](./assets/aem-guides_token-authentication-external-application.zip) ejecutada desde la línea de comandos para actualizar los metadatos de recursos en AEM as a Cloud Service utilizando la [API HTTP de recursos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

El flujo de ejecución de la aplicación Node.js es el siguiente:

![Aplicación externa](./assets/overview/external-application.png)

1. La aplicación Node.js se invoca desde la línea de comandos
1. Los parámetros de la línea de comandos definen:
   + El host del servicio AEM as a Cloud Service Author al que se va a conectar (`aem`)
   + La carpeta de recursos de AEM cuyos recursos se actualizarán (`folder`)
   + La propiedad de metadatos y el valor que se va a actualizar (`propertyName` y `propertyValue`)
   + La ruta local al archivo que proporciona las credenciales necesarias para acceder a AEM as a Cloud Service (`file`)
1. El token de acceso utilizado para autenticarse en AEM se deriva del archivo JSON proporcionado mediante el parámetro de línea de comandos `file`

   a. Si las credenciales de servicio utilizadas para el desarrollo no local se proporcionan en el archivo JSON (`file`), el token de acceso se recupera de las API de Adobe IMS
1. La aplicación utiliza el token de acceso para acceder a AEM y enumera todos los recursos de la carpeta especificada en el parámetro de línea de comandos `folder`
1. Para cada recurso de la carpeta, la aplicación actualiza sus metadatos en función del nombre y valor de la propiedad especificados en los parámetros de línea de comandos `propertyName` y `propertyValue`

Aunque esta aplicación de ejemplo es Node.js, estas interacciones pueden desarrollarse con diferentes lenguajes de programación y ejecutarse desde otros sistemas externos.

## Token de acceso de desarrollo local

Los tokens de acceso de desarrollo local se generan para un entorno específico de AEM as a Cloud Service y proporcionan acceso a los servicios de creación y publicación.  Estos tokens de acceso son temporales y solo deben utilizarse durante el desarrollo de aplicaciones o sistemas externos que interactúen con AEM a través de HTTP. En lugar de que un desarrollador tenga que obtener y administrar credenciales de servicio de bonafide, puede generar rápida y fácilmente un token de acceso temporal que le permita desarrollar su integración.

+ [Cómo usar el token de acceso de desarrollo local](./local-development-access-token.md)

## Credenciales de servicio

Las credenciales de servicio son las credenciales valiosas utilizadas en cualquier escenario que no sea de desarrollo (lo que es más obvio es la producción) que faciliten la capacidad de una aplicación externa o del sistema para autenticarse en AEM as a Cloud Service a través de HTTP e interactuar con él. Las propias credenciales de servicio no se envían a AEM para su autenticación, sino que la aplicación externa las utiliza para generar un JWT, que se intercambia con las API de Adobe IMS _para_ un token de acceso, que se puede utilizar para autenticar solicitudes HTTP a AEM as a Cloud Service.

+ [Cómo utilizar las credenciales del servicio](./service-credentials.md)

## Recursos adicionales

+ [Descargue la aplicación de ejemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Otras muestras de código de creación e intercambio de JWT
   + [Ejemplos de código de Node.js, Java, Python, C#.NET y PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Ejemplo de código basado en JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)

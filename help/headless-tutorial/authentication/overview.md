---
title: Autentificación para AEM como Cloud Service desde una aplicación externa
description: Explore cómo una aplicación externa puede autenticarse mediante programación e interactuar con AEM como Cloud Service mediante HTTP mediante Tokenes de acceso de desarrollo local y credenciales de servicio.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
translation-type: tm+mt
source-git-commit: c4f3d437b5ecfe6cb97314076cd3a5e31b184c79
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 0%

---


# Autenticación basada en tokens para AEM como Cloud Service

En este tutorial, explore bien cómo una aplicación externa puede autenticarse e interactuar mediante programación para AEM como Cloud Service sobre HTTP mediante tokenes de acceso.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Requisitos previos

Antes de seguir este tutorial, asegúrese de que dispone de los siguientes elementos:

1. Acceso a un AEM como entorno Cloud Service (preferiblemente un entorno de desarrollo o un programa de Simulador para pruebas)
1. Pertenencia a la AEM como autor de un entorno de Cloud Service o servicios AEM Perfil de producto administrador
1. Membresía o acceso al administrador de organización de IMS de Adobe (deberá realizar una inicialización única de las [Credenciales de servicio](./service-credentials.md))
1. El [sitio WKND](https://github.com/adobe/aem-guides-wknd) más reciente implementado en el entorno del Cloud Service

## Visión general de la aplicación externa

Este tutorial utiliza una [aplicación Node.js simple](./assets/aem-guides_token-authentication-external-application.zip) ejecutada desde la línea de comandos para actualizar los metadatos de los recursos en AEM como Cloud Service mediante [API HTTP de recursos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

El flujo de ejecución de la aplicación Node.js es el siguiente:

![Aplicación externa](./assets/overview/external-application.png)

1. La aplicación Node.js se invoca desde la línea de comandos
1. Los parámetros de línea de comandos definen:
   + El AEM como host de servicio Cloud Service Author al que conectarse (`aem`)
   + La carpeta de recursos AEM cuyos recursos se actualizarán (`folder`)
   + La propiedad de metadatos y el valor que se va a actualizar (`propertyName` y `propertyValue`)
   + Ruta de acceso local al archivo que proporciona las credenciales necesarias para acceder a AEM como Cloud Service (`file`)
1. El token de acceso utilizado para autenticarse en AEM se deriva del archivo JSON proporcionado a través del parámetro de línea de comandos `file`

   a. Si las credenciales de servicio utilizadas para el desarrollo no local se proporcionan en el archivo JSON (`file`), el token de acceso se recupera de las API de IMS de Adobe
1. La aplicación utiliza el token de acceso para acceder a AEM y lista de todos los recursos de la carpeta especificada en el parámetro de línea de comandos `folder`
1. Para cada recurso de la carpeta, la aplicación actualiza sus metadatos en función del nombre de propiedad y el valor especificados en los parámetros de línea de comandos `propertyName` y `propertyValue`

Aunque esta aplicación de ejemplo es Node.js, estas interacciones pueden desarrollarse con diferentes lenguajes de programación y ejecutarse desde otros sistemas externos.

## Token de acceso de desarrollo local

Los Tokenes de acceso de desarrollo local se generan para un AEM específico como entorno de Cloud Service y proporcionan acceso a los servicios de creación y publicación.  Estos tokenes de acceso son temporales y solo se utilizan durante el desarrollo de aplicaciones o sistemas externos que interactúan con AEM a través de HTTP. En lugar de que un programador tenga que obtener y administrar credenciales de servicio de bonafide, puede generar rápida y fácilmente un token de acceso temporal que le permita desarrollar su integración.

+ [Cómo usar el Token de acceso de desarrollo local](./local-development-access-token.md)

## Credenciales de servicio

Credenciales de servicio son las credenciales fehacientes utilizadas en cualquier escenario que no sea de desarrollo (principalmente producción) que facilitan la capacidad de una aplicación externa o del sistema para autenticarse e interactuar con AEM como Cloud Service sobre HTTP. Las credenciales de servicio no se envían a AEM para la autenticación, sino que la aplicación externa las utiliza para generar un JWT, que se intercambia con las API de Adobe IMS _para_ un token de acceso, que luego se puede utilizar para autenticar solicitudes HTTP para AEM como Cloud Service.

+ [Cómo utilizar las credenciales de servicio](./service-credentials.md)

## Recursos adicionales

+ [Descargar la aplicación de ejemplo](./assets/aem-guides_token-authentication-external-application.zip)
+ Otras muestras de código de creación e intercambio de JWT
   + [Ejemplos de código de Node.js, Java, Python, C#.NET y PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [Ejemplo de código basado en JavaScript/Axios](https://github.com/adobe/aemcs-api-client-lib)

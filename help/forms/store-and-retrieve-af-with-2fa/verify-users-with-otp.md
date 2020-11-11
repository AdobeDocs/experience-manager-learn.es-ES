---
title: Verificar usuarios con OTP
description: Compruebe el número de móvil asociado al número de aplicación mediante OTP.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---



# Verificar usuarios con OTP

SMS Two Factor Authentication (Dual Factor Authentication) es un procedimiento de verificación de seguridad que se activa a través del inicio de sesión de un usuario en un sitio web, software o aplicación. En el proceso de inicio de sesión, se envía automáticamente un mensaje de texto al usuario a su número de teléfono móvil que contiene un código numérico único.

Existen varias organizaciones que proporcionan este servicio y siempre que tengan API de REST bien documentadas, podrá integrar fácilmente AEM Forms mediante las funciones de integración de datos de AEM Forms. A los efectos de este tutorial, he utilizado [Nexmo](https://developer.nexmo.com/verify/overview) para demostrar el caso de uso de SMS 2FA.

Se siguieron los siguientes pasos para implementar SMS 2FA con AEM Forms mediante el servicio Nexmo Verify.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Nexmo](https://dashboard.nexmo.com/sign-in). Anote la clave de API y la clave secreta de API. Estas claves serán necesarias para invocar las API de REST del servicio de Nexmo.

## Crear archivo Swagger/OpenAPI

La especificación OpenAPI (anteriormente la especificación Swagger) es un formato de descripción de API para las API de REST. Un archivo OpenAPI le permite describir toda la API, incluyendo:

* Extremos disponibles (/usuarios) y operaciones en cada extremo (GET/usuarios, POST/usuarios)
* Parámetros de operación Entrada y salida para cada método operationAuthentication
* Información de contacto, licencia, condiciones de uso y otra información.
* Las especificaciones de API se pueden escribir en YAML o JSON. El formato es fácil de aprender y legible tanto para los seres humanos como para las máquinas.

Para crear su primer archivo swagger/OpenAPI, siga la documentación de [OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms admite OpenAPI Specification versión 2.0 (fka Swagger).

Utilice el editor [de](https://editor.swagger.io/) swagger para crear el archivo de swagger y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo swagger se puede crear en formato JSON o YAML. El archivo de swagger completado se puede descargar desde [aquí](assets/two-factore-authentication-swagger.zip)

## Crear fuente de datos

Para integrar AEM/AEM Forms con aplicaciones de terceros, necesitamos un origen de datos basado en [REST que utilice el archivo](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) swagger en la configuración de servicios en la nube. El origen de datos completado se proporciona como parte de los recursos de este curso.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con modelos [de datos de](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)formularios. Un modelo de datos de formulario depende de los orígenes de datos para el intercambio de datos.
El modelo de datos de formulario completado se puede [descargar desde aquí](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

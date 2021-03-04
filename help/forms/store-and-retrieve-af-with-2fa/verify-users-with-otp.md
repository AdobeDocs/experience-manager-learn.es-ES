---
title: Verificar usuarios con OTP
description: Compruebe el número de móvil asociado al número de aplicación mediante OTP.
feature: Formularios adaptables
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 1%

---



# Verificar usuarios con OTP

SMS Two Factor Authentication (Dual Factor Authentication) es un procedimiento de verificación de seguridad que se activa mediante el inicio de sesión de un usuario en un sitio web, software o aplicación. En el proceso de inicio de sesión, se envía automáticamente al usuario un SMS a su número de móvil que contiene un código numérico único.

Existen varias organizaciones que proporcionan este servicio y, siempre y cuando tengan API de REST bien documentadas, puede integrar fácilmente AEM Forms mediante las funciones de integración de datos de AEM Forms. A los efectos de este tutorial, he utilizado [Nexmo](https://developer.nexmo.com/verify/overview) para demostrar el caso de uso de SMS 2FA.

Se siguieron los siguientes pasos para implementar el SMS 2FA con AEM Forms mediante el servicio de verificación de Nexmo.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Nexmo](https://dashboard.nexmo.com/sign-in). Anote la clave de API y la clave secreta de API. Estas claves serán necesarias para invocar las API de REST del servicio de Nexmo.

## Crear archivo Swagger/OpenAPI

La especificación OpenAPI (anteriormente la especificación Swagger) es un formato de descripción de API para las API de REST. Un archivo OpenAPI le permite describir toda la API, lo que incluye:

* Puntos finales disponibles (/usuarios) y operaciones en cada punto final (GET /usuarios, POST/usuarios)
* Parámetros de operación Entrada y salida para cada operación
Métodos de autenticación
* Información de contacto, licencia, términos de uso y otra información.
* Las especificaciones de API se pueden escribir en YAML o JSON. El formato es fácil de aprender y de leer tanto para humanos como para máquinas.

Para crear su primer archivo swagger/OpenAPI, siga la [documentación de OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms es compatible con OpenAPI Specification versión 2.0 (fka Swagger).

Utilice el [editor de swagger](https://editor.swagger.io/) para crear el archivo de intercambio y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo de intercambio se puede crear en formato JSON o YAML. El archivo de cambio completado se puede descargar desde [aquí](assets/two-factore-authentication-swagger.zip)

## Crear fuente de datos

Para integrar AEM/AEM Forms con aplicaciones de terceros, necesitamos [fuentes de datos basadas en REST utilizando el archivo de intercambio](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de los servicios de nube. La fuente de datos completa se proporciona como parte de los recursos de este curso.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modelo de datos de formulario se basa en fuentes de datos para el intercambio de datos.
El modelo de datos de formulario completado se puede [descargar desde aquí](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

---
title: Verificar usuarios con OTP
description: Compruebe el número de móvil asociado al número de aplicación mediante OTP.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 2%

---

# Verificar usuarios con OTP

La autenticación de dos factores (autenticación de doble factor) es un procedimiento de verificación de seguridad que se activa mediante el inicio de sesión del usuario en un sitio web, software o aplicación. En el proceso de inicio de sesión, se envía automáticamente un SMS al usuario a su número de móvil que contiene un código numérico único.

Existen varias organizaciones que proporcionan este servicio y, siempre y cuando tengan API de REST bien documentadas, puede integrar fácilmente AEM Forms utilizando las funcionalidades de integración de datos de AEM Forms. Para los fines de este tutorial, he utilizado [Siguiente](https://developer.nexmo.com/verify/overview) para mostrar el caso de uso de SMS 2FA.

Se siguieron los siguientes pasos para implementar el SMS 2FA con AEM Forms mediante el servicio Nexmo Verify.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Siguiente](https://dashboard.nexmo.com/sign-in). Anote la clave de API y la clave secreta de API. Estas claves son necesarias para invocar las API de REST del servicio de Nexmo.

## Crear archivo Swagger/OpenAPI

La especificación OpenAPI (anteriormente Especificación de Swagger) es un formato de descripción de API para las API de REST. Un archivo OpenAPI permite describir toda la API, lo que incluye:

* Puntos finales (/users) y operaciones disponibles en cada punto final (GET /users, POST /users)
* Parámetros de operación Entrada y salida para cada operación Métodos de autenticación
* Información de contacto, licencia, condiciones de uso y otra información.
* Las especificaciones de API se pueden escribir en YAML o JSON. El formato es fácil de aprender y de leer tanto para humanos como para máquinas.

Para crear su primer archivo swagger/OpenAPI, siga las [Documentación de OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms admite la especificación OpenAPI versión 2.0 (fka Swagger).

Utilice el [editor de swagger](https://editor.swagger.io/) para crear el archivo swagger y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo swagger se puede crear en formato JSON o YAML. El archivo Swagger completado se puede descargar desde [aquí](assets/two-factore-authentication-swagger.zip)

## Crear fuente de datos

AEM Para integrar la integración de/AEM Forms con aplicaciones de terceros, es necesario [Fuente de datos basada en REST que utiliza el archivo swagger](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de cloud services. La fuente de datos completada se le proporciona como parte de estos recursos del curso.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=es). Un modelo de datos de formulario se basa en fuentes de datos para el intercambio de datos.
El modelo de datos de formulario completado puede ser [descargado desde aquí](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

[Creación del formulario principal](./create-the-main-adaptive-form.md)

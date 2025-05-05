---
title: Autenticación de dos factores SMS
description: Añada un nivel adicional de seguridad para ayudar a confirmar la identidad de un usuario cuando desee realizar determinadas actividades
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 1%

---

# Verificar usuarios con sus números de teléfono móvil

La autenticación de dos factores (autenticación de doble factor) es un procedimiento de verificación de seguridad que se activa mediante el inicio de sesión del usuario en un sitio web, software o aplicación. En el proceso de inicio de sesión, se envía automáticamente un SMS al usuario a su número de móvil que contiene un código numérico único.

Existen varias organizaciones que proporcionan este servicio y, siempre y cuando tengan API de REST bien documentadas, puede integrar fácilmente AEM Forms utilizando las funcionalidades de integración de datos de AEM Forms. Para los fines de este tutorial, he usado [Nexmo](https://developer.nexmo.com/verify/overview) para mostrar el caso de uso de SMS 2FA.

Se siguieron los siguientes pasos para implementar el SMS 2FA con AEM Forms mediante el servicio Nexmo Verify.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Nexmo](https://dashboard.nexmo.com/sign-in). Anote la clave de API y la clave secreta de API. Estas claves son necesarias para invocar las API de REST del servicio de Nexmo.

## Crear archivo Swagger/OpenAPI

La especificación OpenAPI (anteriormente Especificación de Swagger) es un formato de descripción de API para las API de REST. Un archivo OpenAPI permite describir toda la API, lo que incluye:

* Extremos disponibles (/users) y operaciones en cada extremo (GET /users, POST /users)
* Parámetros de operación Entrada y salida para cada operación
Métodos de autenticación
* Información de contacto, licencia, condiciones de uso y otra información.
* Las especificaciones de API se pueden escribir en YAML o JSON. El formato es fácil de aprender y de leer tanto para humanos como para máquinas.

Para crear su primer archivo swagger/OpenAPI, siga la [documentación de OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms admite la especificación OpenAPI versión 2.0 (fka Swagger).

Use el [editor swagger](https://editor.swagger.io/) para crear su archivo swagger y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo swagger se puede crear en formato JSON o YAML. El archivo Swagger completado se puede descargar desde [aquí](assets/two-factore-authentication-swagger.zip)

## Crear Source de datos

Para integrar AEM/AEM Forms con aplicaciones de terceros, necesitamos [crear una fuente de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=es) en la configuración de los servicios en la nube.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=es). Un modelo de datos de formulario se basa en fuentes de datos para el intercambio de datos.
El modelo de datos de formulario completado se puede [descargar desde aquí](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Crear formulario adaptable

Integre las invocaciones POST del modelo de datos de formulario con el formulario adaptable para comprobar el número de teléfono móvil introducido por el usuario en el formulario. Puede crear su propio formulario adaptable y utilizar la invocación POST del modelo de datos de formulario para enviar y comprobar el código OTP según sus necesidades.

Si desea utilizar los recursos de ejemplo con sus claves API, siga los siguientes pasos:

* [Descargue el modelo de datos de formulario](assets/sms-2fa-fdm.zip) e impórtelo a AEM mediante [el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Descargar el formulario adaptable de ejemplo [descargado desde aquí](assets/sms-2fa-verification-af.zip). Este formulario de ejemplo utiliza las invocaciones de servicio del modelo de datos de formulario que se proporciona como parte de este artículo.
* Importe el formulario en AEM desde [Forms y la interfaz de usuario del documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra el formulario en modo de edición. Abra el editor de reglas para el siguiente campo

![sms-send](assets/check-sms.PNG)

* Edite la regla asociada al campo. Proporcione las claves API adecuadas
* Guarde el formulario
* [Previsualizar el formulario](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) y probar la funcionalidad

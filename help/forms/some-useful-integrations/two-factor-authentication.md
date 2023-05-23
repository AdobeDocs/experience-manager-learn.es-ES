---
title: Autenticación de dos factores SMS
description: Añada un nivel adicional de seguridad para ayudar a confirmar la identidad de un usuario cuando desee realizar determinadas actividades
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 4%

---

# Verificar usuarios con sus números de teléfono móvil

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

AEM Para integrar la integración de/AEM Forms con aplicaciones de terceros, es necesario [crear fuente de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de cloud services.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=es). Un modelo de datos de formulario se basa en fuentes de datos para el intercambio de datos.
El modelo de datos de formulario completado puede ser [descargado desde aquí](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Crear formulario adaptable

Integre las invocaciones del POST al modelo de datos de formulario con el formulario adaptable para comprobar el número de teléfono móvil introducido por el usuario en el formulario. Puede crear su propio formulario adaptable y utilizar la invocación del POST del modelo de datos de formulario para enviar y comprobar el código OTP según sus necesidades.

Si desea utilizar los recursos de ejemplo con sus claves API, siga los siguientes pasos:

* [Descargar el modelo de datos de formulario](assets/sms-2fa-fdm.zip) AEM e importar en la lista de usuarios de [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Descargue el ejemplo de formulario adaptable que puede ser [descargado desde aquí](assets/sms-2fa-verification-af.zip). Este formulario de ejemplo utiliza las invocaciones de servicio del modelo de datos de formulario que se proporciona como parte de este artículo.
* AEM Importe el formulario a desde la página de inicio de la página de [IU de Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra el formulario en modo de edición. Abra el editor de reglas para el siguiente campo

![sms-send](assets/check-sms.PNG)

* Edite la regla asociada al campo. Proporcione las claves API adecuadas
* Guarde el formulario
* [Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) y probar la funcionalidad

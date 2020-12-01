---
title: Autenticación de dos factores SMS
description: Añada un nivel de seguridad adicional para confirmar la identidad del usuario cuando desee realizar determinadas actividades
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: 4c08b09f59be0eb6644aaec729807b92bc339e82
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---



# Verificar usuarios con sus números de teléfono móvil

SMS Two Factor Authentication (Dual Factor Authentication) es un procedimiento de verificación de seguridad que se activa a través del inicio de sesión de un usuario en un sitio web, software o aplicación. En el proceso de inicio de sesión, se envía automáticamente un mensaje de texto al usuario a su número de teléfono móvil que contiene un código numérico único.

Existen varias organizaciones que proporcionan este servicio y siempre que tengan API de REST bien documentadas, podrá integrar fácilmente AEM Forms mediante las funciones de integración de datos de AEM Forms. A los efectos de este tutorial, he utilizado [Nexmo](https://developer.nexmo.com/verify/overview) para demostrar el caso de uso de SMS 2FA.

Se siguieron los siguientes pasos para implementar SMS 2FA con AEM Forms mediante el servicio Nexmo Verify.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Nexmo](https://dashboard.nexmo.com/sign-in). Anote la clave de API y la clave secreta de API. Estas claves serán necesarias para invocar las API de REST del servicio de Nexmo.

## Crear archivo Swagger/OpenAPI

La especificación OpenAPI (anteriormente la especificación Swagger) es un formato de descripción de API para las API de REST. Un archivo OpenAPI le permite describir toda la API, incluyendo:

* Extremos disponibles (/usuarios) y operaciones en cada extremo (GET/usuarios, POST/usuarios)
* Parámetros de operación Entrada y salida para cada operación
Métodos de autenticación
* Información de contacto, licencia, condiciones de uso y otra información.
* Las especificaciones de API se pueden escribir en YAML o JSON. El formato es fácil de aprender y legible tanto para los seres humanos como para las máquinas.

Para crear su primer archivo swagger/OpenAPI, siga la [documentación de OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms admite OpenAPI Specification versión 2.0 (fka Swagger).

Utilice el [editor de swagger](https://editor.swagger.io/) para crear el archivo de swagger y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo swagger se puede crear en formato JSON o YAML. El archivo de swagger completado se puede descargar desde [aquí](assets/two-factore-authentication-swagger.zip)

## Crear fuente de datos

Para integrar AEM/AEM Forms con aplicaciones de terceros, necesitamos [crear fuentes de datos](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de servicios en la nube.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modelo de datos de formulario depende de los orígenes de datos para el intercambio de datos.
El modelo de datos de formulario completado se puede [descargar desde aquí](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Crear formulario adaptable

Integre las invocaciones del POST del modelo de datos de formulario con el formulario adaptable para comprobar el número de teléfono móvil introducido por el usuario en el formulario. Puede crear su propio formulario adaptable y utilizar la invocación del POST del modelo de datos de formulario para enviar y comprobar el código OTP según sus necesidades.

Si desea utilizar los recursos de muestra con sus claves de API, siga los siguientes pasos:

* [Descargar la importación de ](assets/sms-2fa-fdm.zip) modelado de datos de formulario en AEM mediante  [el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Descargue el formulario adaptable de ejemplo que puede [descargarse desde aquí](assets/sms-2fa-verification-af.zip). Este formulario de ejemplo utiliza las invocaciones de servicio del modelo de datos de formulario que se proporciona como parte de este artículo.
* Importar el formulario en AEM desde la [IU de Forms y Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra el formulario en modo de edición. Abra el editor de reglas para el siguiente campo

![sms-send](assets/check-sms.PNG)

* Edite la regla asociada al campo. Proporcione las claves de API adecuadas
* Guardar el formulario
* [Previsualización del ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) formulario y prueba la funcionalidad



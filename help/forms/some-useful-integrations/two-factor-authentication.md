---
title: SMS Dos factores de autenticación
description: Agregue una capa adicional de seguridad para ayudar a confirmar la identidad de un usuario cuando desee realizar determinadas actividades
feature: Formularios adaptables
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 2%

---



# Verificar usuarios con sus números de teléfono móvil

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

Para integrar AEM/AEM Forms con aplicaciones de terceros, es necesario [crear la fuente de datos](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de los servicios en la nube.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Un modelo de datos de formulario se basa en fuentes de datos para el intercambio de datos.
El modelo de datos de formulario completado se puede [descargar desde aquí](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Crear formulario adaptable

Integre las invocaciones POST del Modelo de datos de formulario con el formulario adaptable para comprobar el número de teléfono móvil introducido por el usuario en el formulario. Puede crear su propio formulario adaptable y utilizar la invocación POST del modelo de datos de formulario para enviar y verificar el código OTP según sus necesidades.

Si desea utilizar los recursos de ejemplo con las claves de API, siga los siguientes pasos:

* [Descargue la importación de modelos de datos de formulario ](assets/sms-2fa-fdm.zip) en AEM mediante el administrador de  [paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Descargue el formulario adaptable de ejemplo que puede [descargarse desde aquí](assets/sms-2fa-verification-af.zip). Este formulario de ejemplo utiliza las invocaciones de servicio del modelo de datos de formulario que se proporciona como parte de este artículo.
* Importar el formulario en AEM desde la [IU de formularios y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra el formulario en modo de edición. Abra el editor de reglas para el siguiente campo

![sms-send](assets/check-sms.PNG)

* Edite la regla asociada al campo . Proporcione las claves de API adecuadas
* Guarde el formulario
* [Obtenga una vista previa del ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) formulario y pruebe la funcionalidad



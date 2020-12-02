---
title: Extracción de datos OCR
description: Extraer datos de documentos emitidos por el gobierno para rellenar formularios.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
translation-type: tm+mt
source-git-commit: c0db84f25334106c798d555c754d550113e91eac
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---



# Extracción de datos OCR

Extraer automáticamente datos de una amplia variedad de documentos emitidos por el gobierno para rellenar los formularios adaptables.

Existen varias organizaciones que proporcionan este servicio y siempre que tengan API de REST bien documentadas, podrá integrarlas fácilmente con AEM Forms mediante la función de integración de datos. Para este tutorial, he utilizado [Analizador de ID](https://www.idanalyzer.com/) para mostrar la extracción de datos de OCR de documentos cargados.

Se siguieron los pasos siguientes para implementar la extracción de datos OCR con AEM Forms mediante el servicio de ID Analyzer.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Analizador de ID](https://portal.idanalyzer.com/signin.html). Anote la clave de API. Esta clave será necesaria para invocar las API de REST del servicio del analizador de ID.

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

Utilice el [editor de swagger](https://editor.swagger.io/) para crear el archivo de swagger y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo swagger se puede crear en formato JSON o YAML. El archivo de swagger completado se puede descargar desde [aquí](assets/drivers-license-swagger.zip)

## Crear fuente de datos

Para integrar AEM/AEM Forms con aplicaciones de terceros, necesitamos [crear fuentes de datos](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de servicios en la nube. Utilice el [archivo de swagger](assets/drivers-license-swagger.zip) para crear la fuente de datos.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Basar el modelo de datos de formulario en el origen de datos creado en el paso anterior.

![fdm](assets/test-dl-fdm.PNG)

## Crear biblioteca de cliente

Necesitamos obtener la cadena codificada base64 del documento cargado. Esta cadena codificada base64 se pasa como uno de los parámetros de nuestra invocación REST.
La biblioteca del cliente se puede descargar [desde aquí.](assets/drivers-license-client-lib.zip)

## Crear formulario adaptable

Integre las invocaciones del POST del modelo de datos de formulario con el formulario adaptable para extraer datos del documento cargado por el usuario en el formulario. Puede crear su propio formulario adaptable y utilizar la invocación del POST del modelo de datos de formulario para enviar la cadena codificada base64 del documento cargado.

## Implementar en el servidor

Si desea utilizar los recursos de muestra con su clave de API, siga los siguientes pasos:

* [Descargue el ](assets/drivers-license-source.zip) origen de datos e impórtelos en AEM mediante  [el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Descargar la importación de ](assets/drivers-license-fdm.zip) modelado de datos de formulario en AEM mediante  [el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Descargar la biblioteca de cliente](assets/drivers-license-client-lib.zip)
* Descargue el formulario adaptable de ejemplo que puede [descargarse desde aquí](assets/adaptive-form-dl.zip). Este formulario de ejemplo utiliza las invocaciones de servicio del modelo de datos de formulario que se proporciona como parte de este artículo.
* Importar el formulario en AEM desde la [IU de Forms y Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra el formulario en el modo [de edición.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Especifique la clave de API como valor predeterminado en el campo apikey y guarde los cambios
* Abra el editor de reglas para el campo Cadena base 64. Observe la invocación del servicio cuando se cambia el valor de este campo.
* Guardar el formulario
* [Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), carga la imagen principal de la licencia de controladores



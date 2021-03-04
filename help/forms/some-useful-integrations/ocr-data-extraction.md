---
title: Extracción de datos de OCR
description: Extraiga datos de documentos emitidos por el gobierno para rellenar formularios.
feature: Formularios codificados por barras
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 2%

---



# Extracción de datos de OCR

Extraer automáticamente datos de una amplia variedad de documentos emitidos por el gobierno para rellenar sus formularios adaptables.

Existen varias organizaciones que proporcionan este servicio y, siempre que tengan API de REST bien documentadas, podrá integrarlas fácilmente con AEM Forms mediante la capacidad de integración de datos. A los efectos de este tutorial, he utilizado [ID Analyzer](https://www.idanalyzer.com/) para demostrar la extracción de datos OCR de los documentos cargados.

Se siguieron los siguientes pasos para implementar la extracción de datos OCR con AEM Forms mediante el servicio de ID Analyzer.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [ID Analyzer](https://portal.idanalyzer.com/signin.html). Anote la clave de API. Esta clave será necesaria para invocar las API de REST del servicio de ID Analyzer.

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

Utilice el [editor de swagger](https://editor.swagger.io/) para crear el archivo de intercambio y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo de intercambio se puede crear en formato JSON o YAML. El archivo de cambio completado se puede descargar desde [aquí](assets/drivers-license-swagger.zip)

## Crear fuente de datos

Para integrar AEM/AEM Forms con aplicaciones de terceros, es necesario [crear la fuente de datos](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de los servicios en la nube. Utilice el [archivo de intercambio](assets/drivers-license-swagger.zip) para crear la fuente de datos.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Base el modelo de datos de formulario en el origen de datos creado en el paso anterior.

![fdm](assets/test-dl-fdm.PNG)

## Crear lista de clientes

Necesitaríamos obtener la cadena codificada base64 del documento cargado. Esta cadena codificada base64 se pasa como uno de los parámetros de nuestra invocación REST.
La biblioteca de cliente se puede descargar [desde aquí.](assets/drivers-license-client-lib.zip)

## Crear formulario adaptable

Integre las invocaciones POST del Modelo de datos de formulario con su formulario adaptable para extraer datos del documento cargado por el usuario en el formulario. Puede crear su propio formulario adaptable y utilizar la invocación POST del modelo de datos de formulario para enviar la cadena codificada base64 del documento cargado.

## Implementar en el servidor

Si desea utilizar los recursos de ejemplo con su clave de API, siga los siguientes pasos:

* [Descargue la ](assets/drivers-license-source.zip) fuente de datos e impórtelos en AEM mediante el administrador de  [paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Descargue la importación de modelos de datos de formulario ](assets/drivers-license-fdm.zip) en AEM mediante el administrador de  [paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Descargar la biblioteca del cliente](assets/drivers-license-client-lib.zip)
* Descargue el formulario adaptable de ejemplo que puede [descargarse desde aquí](assets/adaptive-form-dl.zip). Este formulario de ejemplo utiliza las invocaciones de servicio del modelo de datos de formulario que se proporciona como parte de este artículo.
* Importar el formulario en AEM desde la [IU de formularios y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra el formulario en [modo de edición.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Especifique la clave de API como valor predeterminado en el campo apikey y guarde los cambios
* Abra el editor de reglas para el campo Base 64 String . Observe la invocación del servicio cuando se cambia el valor de este campo.
* Guarde el formulario
* [Previsualice el formulario](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), cargue la imagen principal de la licencia de controladores



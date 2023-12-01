---
title: Extracción de datos de OCR
description: Extraer datos de documentos emitidos por el gobierno para rellenar formularios.
feature: Barcoded Forms
version: 6.4,6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 4%

---

# Extracción de datos de OCR

Extraiga automáticamente datos de una amplia variedad de documentos emitidos por el gobierno para rellenar los formularios adaptables.

Existen varias organizaciones que proporcionan este servicio y, siempre y cuando tengan API de REST bien documentadas, puede integrarse fácilmente con AEM Forms mediante la capacidad de integración de datos. Para los fines de este tutorial, he utilizado [Analizador de ID](https://www.idanalyzer.com/) para demostrar la extracción de datos de OCR de los documentos cargados.

Se siguieron los siguientes pasos para implementar la extracción de datos de OCR con AEM Forms mediante el servicio Analizador de ID.

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Analizador de ID](https://portal.idanalyzer.com/signin.html). Tome nota de la clave de API. Esta clave es necesaria para invocar las API de REST del servicio del Analizador de ID.

## Crear archivo Swagger/OpenAPI

La especificación OpenAPI (anteriormente Especificación de Swagger) es un formato de descripción de API para las API de REST. Un archivo OpenAPI permite describir toda la API, lo que incluye:

* Puntos finales (/users) y operaciones disponibles en cada punto final (GET /users, POST /users)
* Parámetros de operación Entrada y salida para cada operación Métodos de autenticación
* Información de contacto, licencia, condiciones de uso y otra información.
* Las especificaciones de API se pueden escribir en YAML o JSON. El formato es fácil de aprender y de leer tanto para humanos como para máquinas.

Para crear su primer archivo swagger/OpenAPI, siga las [Documentación de OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms admite la especificación OpenAPI versión 2.0 (fka Swagger).

Utilice el [editor de swagger](https://editor.swagger.io/) para crear el archivo swagger y describir las operaciones que envían y verifican el código OTP enviado mediante SMS. El archivo swagger se puede crear en formato JSON o YAML. El archivo Swagger completado se puede descargar desde [aquí](assets/drivers-license-swagger.zip)

## Consideraciones al definir el archivo swagger

* Se requieren definiciones
* Es necesario utilizar $ref para las definiciones de métodos
* Prefiere tener secciones de consumo y producción definidas
* No defina parámetros de cuerpo de solicitud en línea ni parámetros de respuesta. Trate de modular tanto como sea posible. Por ejemplo, no se admite la siguiente definición

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

Se admite lo siguiente con una referencia a la definición de requestBody

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Archivo Swagger de muestra para su referencia](assets/sample-swagger.json)

## Crear fuente de datos

AEM Para integrar la integración de/AEM Forms con aplicaciones de terceros, es necesario [crear fuente de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de cloud services. Utilice el [archivo swagger](assets/drivers-license-swagger.zip) para crear la fuente de datos.

## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=es). Base el modelo de datos de formulario en la fuente de datos creada en el paso anterior.

![fdm](assets/test-dl-fdm.PNG)

## Crear biblioteca de cliente

Necesitaríamos obtener la cadena codificada en base64 del documento cargado. Esta cadena codificada en base64 se pasa como uno de los parámetros de nuestra invocación REST.
La biblioteca de cliente se puede descargar [desde aquí.](assets/drivers-license-client-lib.zip)

## Crear formulario adaptable

Integre las invocaciones del POST al modelo de datos de formulario con el formulario adaptable para extraer datos del documento cargado por el usuario en el formulario. Puede crear su propio formulario adaptable y utilizar la invocación del POST del modelo de datos de formulario para enviar la cadena codificada en base64 del documento cargado.

## Implementación en el servidor

Si desea utilizar los recursos de ejemplo con la clave de API, siga estos pasos:

* [Descargar la fuente de datos](assets/drivers-license-source.zip) AEM e importar en la lista de usuarios de [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Descargar el modelo de datos de formulario](assets/drivers-license-fdm.zip) AEM e importar en la lista de usuarios de [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Descargar la biblioteca del cliente](assets/drivers-license-client-lib.zip)
* Descargue el ejemplo de formulario adaptable que puede ser [descargado desde aquí](assets/adaptive-form-dl.zip). Este formulario de ejemplo utiliza las invocaciones de servicio del modelo de datos de formulario que se proporciona como parte de este artículo.
* AEM Importe el formulario a desde la página de inicio de la página de [IU de Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra el formulario en [modo de edición.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Especifique la clave de API como valor predeterminado en el campo apikey y guarde los cambios
* Abra el editor de reglas para el campo Base 64 String. Observe la invocación del servicio cuando se cambia el valor de este campo.
* Guarde el formulario
* [Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), sube una foto de tu licencia de conducir

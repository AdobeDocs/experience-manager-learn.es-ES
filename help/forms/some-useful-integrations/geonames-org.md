---
title: Listas desplegables en cascada
description: Rellene listas desplegables basadas en una selección de lista desplegable anterior.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
duration: 218
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 1%

---

# Listas desplegables en cascada

Una lista desplegable en cascada es una serie de controles DropDownList dependientes en los que un control DropDownList depende del control DropDownList primario o anterior. Los elementos del control DropDownList se rellenan basándose en un elemento seleccionado por el usuario de otro control DropDownList.

## Muestra del caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

Para los fines de este tutorial, he utilizado [API de REST de Geonames](https://www.geonames.org/export/web-services.html) para demostrar esta capacidad.
Existen varias organizaciones que proporcionan este tipo de servicio y, siempre y cuando tengan API de REST bien documentadas, puede integrarse fácilmente con AEM Forms mediante la capacidad de integración de datos

Se siguieron los siguientes pasos para implementar listas desplegables en cascada en AEM Forms

## Crear cuenta de desarrollador

Cree una cuenta de desarrollador con [Geonames](https://www.geonames.org/login). Anote el nombre de usuario. Este nombre de usuario es necesario para invocar las API de REST de geonames.org.

## Crear archivo Swagger/OpenAPI

La especificación OpenAPI (anteriormente Especificación de Swagger) es un formato de descripción de API para las API de REST. Un archivo OpenAPI permite describir toda la API, lo que incluye:

* Puntos finales (/users) y operaciones disponibles en cada punto final (GET /users, POST /users)
* Parámetros de operación Entrada y salida para cada operación Métodos de autenticación
* Información de contacto, licencia, condiciones de uso y otra información.
* Las especificaciones de API se pueden escribir en YAML o JSON. El formato es fácil de aprender y de leer tanto para humanos como para máquinas.

Para crear su primer archivo swagger/OpenAPI, siga las [Documentación de OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms admite la especificación OpenAPI versión 2.0 (FKA Swagger).

Utilice el [editor de swagger](https://editor.swagger.io/) para crear el archivo swagger y describir las operaciones que recuperan todos los países y elementos secundarios del país o estado. El archivo swagger se puede crear en formato JSON o YAML.

## Creación de fuentes de datos

AEM Para integrar la integración de/AEM Forms con aplicaciones de terceros, es necesario [crear fuente de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) en la configuración de cloud services. Utilice el [archivos swagger](assets/geonames-swagger-files.zip) para crear las fuentes de datos.
Deberá crear 2 fuentes de datos (una para recuperar todos los países y otra para obtener los elementos secundarios)


## Crear modelo de datos de formulario

La integración de datos de AEM Forms proporciona una interfaz de usuario intuitiva para crear y trabajar con [modelos de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=es). Base el modelo de datos de formulario en las fuentes de datos creadas en el paso anterior. Modelo de datos de formulario con 2 fuentes de datos

![fdm](assets/geonames-fdm.png)


## Crear formulario adaptable

GET Integre las invocaciones del modelo de datos de formulario con el formulario adaptable para rellenar las listas desplegables.
Cree un formulario adaptable con 2 listas desplegables. Uno para enumerar los países y otro para enumerar los estados o provincias según el país seleccionado.

### Rellenar lista desplegable de países

La lista de países se rellena cuando se inicializa el formulario por primera vez. La siguiente captura de pantalla muestra el editor de reglas configurado para rellenar las opciones de la lista desplegable de país. Tendrá que proporcionar su nombre de usuario con la cuenta de geonames para que esto funcione.
![get-countries](assets/get-countries-rule-editor.png)

#### Rellene la lista desplegable Estado o provincia

Es necesario rellenar la lista desplegable Estado/Provincia en función del país seleccionado. La siguiente captura de pantalla muestra la configuración del editor de reglas
![state-Province-options](assets/state-province-options.png)

### Ejercicio

Agregue 2 listas desplegables denominadas condados y ciudades en el formulario para enumerar los condados y ciudades en función del país y el estado/provincia seleccionados.
![ejercitarse](assets/cascading-drop-down-exercise.png)


### Recursos de muestra

Puede descargar los siguientes recursos para empezar a crear el ejemplo de lista desplegable en cascada. Los archivos swagger completados se pueden descargar desde [aquí](assets/geonames-swagger-files.zip)
Los archivos swagger describen la siguiente API de REST
* [Obtener todos los países](https://secure.geonames.org/countryInfoJSON?username=yourusername)
* [Obtener elementos secundarios del objeto Geoname](https://secure.geonames.org/children?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

El completado [El modelo de datos de formulario se puede descargar desde aquí](assets/geonames-api-form-data-model.zip)

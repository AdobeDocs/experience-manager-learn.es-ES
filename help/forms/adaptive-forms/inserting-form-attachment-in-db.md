---
title: 'Insertar datos adjuntos de formulario en la base de datos '
description: Inserte datos adjuntos de formulario en la base de datos mediante AEM flujo de trabajo.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 10488
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# Inserción de datos adjuntos de formulario en la base de datos

Este artículo explicará el caso de uso del almacenamiento de datos adjuntos de formularios en la base de datos MySQL.

Una solicitud habitual de los clientes es almacenar los datos de formulario capturados y el archivo adjunto de formulario en una tabla de la base de datos.
Para lograr este caso de uso, se siguieron los siguientes pasos

## Crear tabla de base de datos para guardar los datos del formulario y el archivo adjunto

Se ha creado una tabla denominada en todo el sitio para albergar los datos del formulario. Observe la imagen de tipo del nombre de columna **LONGBLOB** para almacenar el adjunto del formulario
![table-schema](assets/insert-picture-table.png)

## Crear modelo de datos de formulario

Se creó un modelo de datos de formulario para comunicarse con la base de datos MySQL. Deberá crear lo siguiente

* [Fuente de datos JDBC en AEM](./data-integration-technical-video-setup.md)
* [Modelo de datos de formulario basado en la fuente de datos JDBC](./jdbc-data-model-technical-video-use.md)

## Crear flujo de trabajo

Si configura el formulario adaptable para que se envíe a un flujo de trabajo AEM, tiene la opción de guardar los archivos adjuntos en un flujo de trabajo o guardar los archivos adjuntos en una carpeta específica de la carga útil. Para este caso de uso, es necesario guardar los archivos adjuntos en una variable de flujo de trabajo de tipo ArrayList of Document. Desde esta ArrayList necesitamos extraer el primer elemento e inicializar una variable de documento. Las variables de flujo de trabajo denominadas **listOfDocuments** y **employeePhoto** se han creado.
Cuando se envía el formulario adaptable para almacenar en déclencheur el flujo de trabajo, un paso del flujo de trabajo inicializa la variable employeePhoto mediante la secuencia de comandos ECMA. El siguiente es el código de script ECMA

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

El siguiente paso en el flujo de trabajo es insertar datos y el archivo adjunto de formulario en la tabla mediante el componente de servicio Invocar modelo de datos de formulario .
![insert-pic](assets/fdm-insert-pic.png)
[El flujo de trabajo completo con el script ecma de ejemplo se puede descargar desde aquí](assets/add-new-employee.zip).

>[!NOTE]
> Deberá crear un nuevo modelo de datos de formulario basado en JDBC y utilizar ese modelo de datos de formulario en el flujo de trabajo

## Crear formulario adaptable

Cree el formulario adaptable basado en el modelo de datos de formulario creado en el paso anterior. Arrastre y suelte los elementos del modelo de datos de formulario en el formulario. Configure el envío del formulario para almacenar en déclencheur el flujo de trabajo y especifique las siguientes propiedades, como se muestra en la captura de pantalla siguiente
![archivos adjuntos de formulario](assets/form-attachments.png)
---
title: Insertar archivo adjunto de formulario en base de datos
description: AEM Insertar archivo adjunto de formulario en la base de datos mediante flujo de trabajo de.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# Insertar datos adjuntos de formulario en la base de datos

Este artículo explicará el caso de uso de almacenar datos adjuntos de formularios en la base de datos MySQL.

Una tarea común de los clientes es almacenar los datos de formulario capturados y los datos adjuntos del formulario en una tabla de la base de datos.
Para llevar a cabo este caso de uso, se siguieron los siguientes pasos

## Crear una tabla de base de datos para guardar los datos del formulario y los datos adjuntos

Se ha creado una tabla denominada nueva para contener los datos del formulario. Observe la imagen del nombre de la columna de tipo **LONGBLOB** para almacenar el archivo adjunto del formulario
![table-schema](assets/insert-picture-table.png)

## Crear modelo de datos de formulario

Se creó un modelo de datos de formulario para comunicarse con la base de datos MySQL. Debe crear lo siguiente

* [AEM Fuente de datos JDBC en el](./data-integration-technical-video-setup.md)
* [Modelo de datos de formulario basado en la fuente de datos JDBC](./jdbc-data-model-technical-video-use.md)

## Crear flujo de trabajo

AEM Al configurar el formulario adaptable para que se envíe a un flujo de trabajo de, tiene la opción de guardar los archivos adjuntos del formulario en una variable de flujo de trabajo o de guardarlos en una carpeta especificada debajo de la carga útil. Para este caso de uso, es necesario guardar los archivos adjuntos en una variable de flujo de trabajo de tipo ArrayList of Document. De esta ArrayList necesitamos extraer el primer elemento e inicializar una variable de documento. Las variables de flujo de trabajo denominadas **listOfDocuments** y **employeePhoto** se han creado.
Cuando se envía el formulario adaptable al déclencheur del flujo de trabajo, un paso en el flujo de trabajo inicializa la variable employeePhoto mediante el script ECMA. El siguiente es el código de script ECMA

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

El siguiente paso del flujo de trabajo es insertar datos y el archivo adjunto del formulario en la tabla mediante el componente de servicio Invocar modelo de datos de formulario.
![insert-pic](assets/fdm-insert-pic.png)
[El flujo de trabajo completo con el script ecma de ejemplo se puede descargar desde aquí](assets/add-new-employee.zip).

>[!NOTE]
> Deberá crear un nuevo modelo de datos de formulario basado en JDBC y utilizar ese modelo de datos de formulario en el flujo de trabajo

## Crear formulario adaptable

Cree el formulario adaptable en función del modelo de datos de formulario creado en el paso anterior. Arrastre y suelte los elementos del modelo de datos de formulario en el formulario. Configure el envío del formulario para almacenar en déclencheur el flujo de trabajo y especifique las siguientes propiedades, como se muestra en la captura de pantalla siguiente
![form-attachments](assets/form-attachments.png)

---
title: Configuración de Data Source
description: Crear DataSource apuntando a la base de datos MySQL
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 64
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 5%

---

# Configuración de Data Source

AEM permite de muchas maneras la integración con una base de datos externa. Una de las prácticas más comunes y estándar de la integración de bases de datos es usar las propiedades de configuración de la fuente de datos obtenida de una conexión Apache Sling a través de [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar los [controladores MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) apropiados en AEM.
A continuación, establezca las propiedades del origen de datos agrupado de la conexión de Sling específicas de la base de datos. La siguiente captura de pantalla muestra la configuración utilizada para este tutorial. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

>[!NOTE]
>Asegúrese de asignar un nombre a la fuente de datos `StoreAndRetrieveAfData`, ya que es el nombre que se usa en el servicio OSGi.


![origen de datos](assets/data-source.JPG)

| Nombre de la propiedad | Valor de propiedad |   |
|---------------------|------------------------------------------------------------------------------------|---|
| Nombre de la fuente de datos | `StoreAndRetrieveAfData` |   |
| Clase de unidad JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |   |
| URI de conexión JDBC | `jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&autoReconnect=true` |   |
|                     |                                                                                    |   |


## Crear base de datos


La siguiente base de datos se utilizó para los fines de este caso de uso. La base de datos tiene una tabla llamada `formdatawithattachments` con las 4 columnas como se muestra en la captura de pantalla siguiente.
![base de datos](assets/table-schema.JPG)

* La columna **afdata** contendrá los datos del formulario adaptable.
* La columna **attachmentInfo** contendrá la información sobre los archivos adjuntos del formulario.
* Las columnas **telephoneNumber** contendrán el número de teléfono móvil de la persona que rellena el formulario.

Cree la base de datos importando el [esquema de base de datos](assets/data-base-schema.sql)
uso de MySQL workbench.

## Crear modelo de datos de formulario

Cree el modelo de datos de formulario y baselo en la fuente de datos creada en el paso anterior.
Configure el servicio **get** de este modelo de datos de formulario como se muestra en la captura de pantalla siguiente.
Asegúrese de que no devuelve una matriz en el servicio **get**.

El propósito de este servicio **get** es obtener el número de teléfono asociado con el identificador de la aplicación.

![get-service](assets/get-service.JPG)

Este modelo de datos de formulario se utilizará en **MyAccountForm** para obtener el número de teléfono asociado con el identificador de la aplicación.

## Siguientes pasos

[Escribir código para guardar archivos adjuntos en formularios](./store-form-attachments.md)

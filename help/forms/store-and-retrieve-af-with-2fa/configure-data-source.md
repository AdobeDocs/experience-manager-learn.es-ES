---
title: Configurar fuente de datos
description: Crear DataSource que apunta a la base de datos MySQL
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---


# Configurar fuente de datos

Existen muchas maneras en que AEM permite la integración con bases de datos externas. Una de las prácticas más comunes y estándar de integración de bases de datos es utilizar las propiedades de configuración de Apache Sling Connection Pooled DataSource a través de [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar los [controladores MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) adecuados para AEM.
A continuación, establezca las propiedades de Sling Connection Pooled DataSource específicas de la base de datos. La siguiente captura de pantalla muestra la configuración utilizada para este tutorial. El esquema de la base de datos se proporciona como parte de este tutorial.

![data-source](assets/data-source.JPG)


* Clase de controlador JDBC: `com.mysql.cj.jdbc.Driver`
* URI de conexión JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Asegúrese de asignar un nombre al origen de datos `StoreAndRetrieveAfData`, ya que éste es el nombre utilizado en el servicio OSGi.


## Crear base de datos


Se utilizó la siguiente base de datos para este caso de uso. La base de datos tiene una tabla denominada `formdatawithattachments` con las 4 columnas como se muestra en la captura de pantalla siguiente.
![data-base](assets/table-schema.JPG)

* La columna **afdata** contendrá los datos del formulario adaptable.
* La columna **attachmentInfo** contendrá la información sobre los datos adjuntos del formulario.
* Las columnas **phoneNumber** contendrán el número móvil de la persona que rellene el formulario.

Cree la base de datos importando el esquema [de base de datos](assets/data-base-schema.sql)
usando el área de trabajo de MySQL.

## Crear modelo de datos de formulario

Cree un modelo de datos de formulario y ciónelo en el origen de datos creado en el paso anterior.
Configure el servicio **get** de este modelo de datos de formulario como se muestra en la captura de pantalla siguiente.
Asegúrese de que no devuelve la matriz en el servicio **get**.

Este servicio **get** se utiliza para recuperar el número de teléfono asociado con la identificación de la aplicación.

![get-service](assets/get-service.JPG)

Este modelo de datos de formulario se utilizará en el **MyAccountForm** para recuperar el número de teléfono asociado con la ID de la aplicación.

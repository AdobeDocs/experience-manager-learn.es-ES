---
title: Almacenamiento y recuperación de datos de formulario de la base de datos MySQL
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 1%

---

# Configurar fuente de datos

Existen muchas maneras en que AEM permite la integración con bases de datos externas. Una de las prácticas más comunes y estándar de integración de bases de datos es utilizar las propiedades de configuración de Apache Sling Connection Pooled DataSource a través de [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar los [controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) adecuados en AEM.
Cree una fuente de datos de conexión compartida de Apache Sling y proporcione las propiedades especificadas en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de este tutorial.

![data-source](assets/save-continue.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![data-base](assets/data-base-tables.PNG)

El archivo sql para crear el esquema puede [descargarse desde aquí](assets/form-data-db.sql). Deberá importar este archivo mediante el área de trabajo de MySql para crear el esquema y la tabla.

>[!NOTE]
>Asegúrese de asignar un nombre al origen de datos **SaveAndContinue**. El código de muestra utiliza el nombre para conectarse a la base de datos.

| Nombre de propiedad | Value |
------------------------|---------------------------------------
| Nombre del origen de datos | SaveAndContinue |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |



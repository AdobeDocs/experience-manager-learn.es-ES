---
title: 'Almacenamiento y recuperación de datos de formulario de la base de datos MySQL: configuración de fuentes de datos'
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
version: 6.3,6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 1%

---

# Configurar fuentes de datos

Existen muchas maneras en que AEM permite la integración con bases de datos externas. Una de las prácticas más comunes y estándar de integración de bases de datos es usar las propiedades de configuración de fuentes de datos agrupadas de la conexión Apache Sling a través del [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar el [Controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) en AEM.
Cree una fuente de datos agrupada de conexión Apache Sling y proporcione las propiedades especificadas en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![fuente de datos](assets/save-continue.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![data-base](assets/data-base-tables.PNG)

El archivo sql para crear el esquema puede ser [descargado desde aquí](assets/form-data-db.sql). Deberá importar este archivo utilizando MySql Workbench para crear el esquema y la tabla.

>[!NOTE]
>Asegúrese de asignar un nombre a la fuente de datos **SaveAndContinue**. El código de ejemplo utiliza el nombre para conectarse a la base de datos.

| Nombre de propiedad | Value |
| ------------------------|---------------------------------------|
| Nombre del origen de datos | SaveAndContinue |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

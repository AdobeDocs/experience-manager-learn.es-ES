---
title: 'Almacenar y recuperar datos de formulario de la base de datos MySQL: configurar fuente de datos'
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 43
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 2%

---

# Configurar fuente de datos

AEM Existen muchas maneras de habilitar la integración con bases de datos externas mediante el uso de la interfaz de usuario de. Una de las prácticas más comunes y estándar de la integración de bases de datos es utilizar las propiedades de configuración de la fuente de datos obtenida de una conexión Apache Sling a través de la [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar el adecuado [Controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEM en la.
Cree la fuente de datos obtenida de una conexión Apache Sling y proporcione las propiedades que se especifican en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![data-source](assets/save-continue.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![data-base](assets/data-base-tables.PNG)

El archivo SQL para crear el esquema puede ser [descargado desde aquí](assets/form-data-db.sql). Deberá importar este archivo mediante MySql Workbench para crear el esquema y la tabla.

>[!NOTE]
>Asegúrese de nombrar la fuente de datos **Guardar y continuar**. El código de ejemplo utiliza el nombre para conectarse a la base de datos.

| Nombre de la propiedad | Valor |
| ------------------------|---------------------------------------|
| Nombre de Datasource | Guardar y continuar |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

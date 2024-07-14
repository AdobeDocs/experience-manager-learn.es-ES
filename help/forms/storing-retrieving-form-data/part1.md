---
title: 'Almacenar y recuperar datos de formulario de la base de datos MySQL: configuración de Data Source'
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 36
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 2%

---

# Configuración de Data Source

AEM Existen muchas maneras de habilitar la integración con bases de datos externas mediante el uso de la interfaz de usuario de. Una de las prácticas más comunes y estándar de la integración de bases de datos es usar las propiedades de configuración de la fuente de datos obtenida de una conexión Apache Sling a través de [configMgr](http://localhost:4502/system/console/configMgr).
AEM El primer paso es descargar e implementar los [controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) adecuados en el servidor de correo de.
Cree la fuente de datos obtenida de una conexión Apache Sling y proporcione las propiedades que se especifican en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![origen de datos](assets/save-continue.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![base de datos](assets/data-base-tables.PNG)

El archivo sql para crear el esquema se puede [descargar desde aquí](assets/form-data-db.sql). Deberá importar este archivo mediante MySql Workbench para crear el esquema y la tabla.

>[!NOTE]
>Asegúrese de asignar un nombre a la fuente de datos **Guardar y continuar**. El código de ejemplo utiliza el nombre para conectarse a la base de datos.

| Nombre de la propiedad | Valor |
| ------------------------|---------------------------------------|
| Nombre de Datasource | `SaveAndContinue` |
| Clase de controlador JDBC | `com.mysql.cj.jdbc.Driver` |
| URI de conexión JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |

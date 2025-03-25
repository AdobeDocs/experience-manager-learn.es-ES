---
title: Configuración de AEM Data Source
description: Configurar la fuente de datos respaldada por MySQL para almacenar y recuperar datos de formulario
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 39
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 3%

---

# Configurar fuente de datos

Existen muchas maneras en que AEM permite la integración con una base de datos externa. Una de las formas más comunes de integrar una base de datos es usando las propiedades de configuración de la fuente de datos obtenida de una conexión Apache Sling a través de [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar los [controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) adecuados en AEM.
Cree la fuente de datos obtenida de una conexión Apache Sling y proporcione las propiedades que se especifican en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![origen de datos](assets/data-source.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![base de datos](assets/data-base.PNG)


>[!NOTE]
>Asegúrese de asignar un nombre a la fuente de datos **aemformstutorial**. El código de ejemplo utiliza el nombre para conectarse a la base de datos.

| Nombre de la propiedad | Valor |
| ------------------------|--------------------------------------- |
| Nombre de Datasource | `SaveAndContinue` |
| Clase de controlador JDBC | `com.mysql.cj.jdbc.Driver` |
| URI de conexión JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |

## Recursos

El archivo SQL para crear el esquema se puede [descargar desde aquí](assets/sign-multiple-forms.sql). Deberá importar este archivo mediante MySql Workbench para crear el esquema y la tabla.

## Siguientes pasos

[Crear un servicio OSGi para almacenar y recuperar datos en la base de datos](./create-osgi-service.md)

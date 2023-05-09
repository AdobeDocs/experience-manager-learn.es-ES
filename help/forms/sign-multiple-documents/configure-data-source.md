---
title: Configurar AEM fuente de datos
description: Configurar el origen de datos respaldado por MySQL para almacenar y recuperar datos de formulario
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 5%

---

# Configurar el origen de datos

Existen muchas maneras en que AEM permite la integración con una base de datos externa. Una de las formas más comunes de integrar una base de datos es mediante el uso de las propiedades de configuración de fuentes de datos agrupadas de la conexión Apache Sling a través del [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar el [Controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) en AEM.
Cree una fuente de datos agrupada de conexión Apache Sling y proporcione las propiedades especificadas en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![fuente de datos](assets/data-source.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Asegúrese de asignar un nombre a la fuente de datos **aemformstutorial**. El código de ejemplo utiliza el nombre para conectarse a la base de datos.

| Nombre de la propiedad | Valor |
| ------------------------|--------------------------------------- |
| Nombre del origen de datos | SaveAndContinue |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

El archivo SQL para crear el esquema puede ser [descargado desde aquí](assets/sign-multiple-forms.sql). Deberá importar este archivo utilizando MySql Workbench para crear el esquema y la tabla.

## Pasos siguientes

[Creación de un servicio OSGi para almacenar y recuperar datos en la base de datos](./create-osgi-service.md)

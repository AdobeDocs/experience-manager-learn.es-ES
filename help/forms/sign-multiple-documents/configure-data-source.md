---
title: Configurar fuente de datos AEM
description: Configurar el origen de datos respaldado por MySQL para almacenar y recuperar datos de formulario
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 2%

---

# Configurar fuente de datos

Existen muchas maneras en que AEM permite la integración con una base de datos externa. Una de las formas más comunes de integrar una base de datos es utilizar las propiedades de configuración de Apache Sling Connection Pooled DataSource a través de [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar los [controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) adecuados en AEM.
Cree una fuente de datos de conexión compartida de Apache Sling y proporcione las propiedades especificadas en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de este tutorial.

![data-source](assets/data-source.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Asegúrese de asignar un nombre al origen de datos **formstutorial**. El código de muestra utiliza el nombre para conectarse a la base de datos.

| Nombre de propiedad | Value |
------------------------|---------------------------------------
| Nombre del origen de datos | SaveAndContinue |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

El archivo sql para crear el esquema puede [descargarse desde aquí](assets/sign-multiple-forms.sql). Deberá importar este archivo mediante el área de trabajo de MySql para crear el esquema y la tabla.



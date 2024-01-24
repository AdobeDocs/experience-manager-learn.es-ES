---
title: AEM Configurar origen de datos
description: Configurar la fuente de datos respaldada por MySQL para almacenar y recuperar datos de formulario
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 47
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 3%

---

# Configurar fuente de datos

AEM Existen muchas maneras de habilitar la integración con una base de datos externa mediante el uso de la. Una de las formas más comunes de integrar una base de datos es mediante el uso de las propiedades de configuración de la fuente de datos obtenida de una conexión Apache Sling a través del [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar el adecuado [Controladores MySql](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEM en la.
Cree la fuente de datos obtenida de una conexión Apache Sling y proporcione las propiedades que se especifican en la captura de pantalla siguiente. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![data-source](assets/data-source.PNG)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla siguiente.

![data-base](assets/data-base.PNG)


>[!NOTE]
>Asegúrese de nombrar la fuente de datos **aemformstutorial**. El código de ejemplo utiliza el nombre para conectarse a la base de datos.

| Nombre de la propiedad | Valor |
| ------------------------|--------------------------------------- |
| Nombre de Datasource | Guardar y continuar |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

## Assets

El archivo SQL para crear el esquema puede ser [descargado desde aquí](assets/sign-multiple-forms.sql). Deberá importar este archivo mediante MySql Workbench para crear el esquema y la tabla.

## Pasos siguientes

[Crear un servicio OSGi para almacenar y recuperar datos en la base de datos](./create-osgi-service.md)

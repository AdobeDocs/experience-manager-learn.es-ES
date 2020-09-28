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
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Configurar fuente de datos

Existen muchas maneras en que AEM permite la integración con bases de datos externas. Una de las prácticas más comunes y estándar de integración de bases de datos es utilizar las propiedades de configuración de Apache Sling Connection Pooled DataSource a través de [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar los controladores [](https://mvnrepository.com/artifact/mysql/mysql-connector-java) My SQL correspondientes en AEM.
A continuación, establezca las propiedades de Sling Connection Pooled DataSource. Estas propiedades son específicas de la base de datos. La siguiente captura de pantalla muestra la configuración utilizada para este tutorial. El esquema de la base de datos se proporciona como parte de este tutorial.
![data-source](assets/data-source.png)

La base de datos tiene una tabla denominada formdata con las 3 columnas, como se muestra en la captura de pantalla debajo![de data-base](assets/data-base-tables.PNG)

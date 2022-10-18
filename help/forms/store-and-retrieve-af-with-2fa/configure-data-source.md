---
title: Configurar fuentes de datos
description: Crear DataSource apuntando a la base de datos MySQL
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 30c882da3a89820b5e11bc2902bb92dd0629efe9
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 3%

---

# Configurar fuentes de datos

Existen muchas maneras en que AEM permite la integración con una base de datos externa. Una de las prácticas más comunes y estándar de integración de bases de datos es usar las propiedades de configuración de fuentes de datos agrupadas de la conexión Apache Sling a través del [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar el [Controladores MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) a AEM.
A continuación, establezca las propiedades de Origen de datos agrupados de la conexión Sling específicas de la base de datos. La siguiente captura de pantalla muestra la configuración utilizada para este tutorial. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![fuente de datos](assets/data-source.JPG)


* Clase de controlador JDBC: `com.mysql.cj.jdbc.Driver`
* URI de conexión JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Asegúrese de asignar un nombre a la fuente de datos `StoreAndRetrieveAfData` ya que este es el nombre que se utiliza en el servicio OSGi.


## Crear base de datos


La siguiente base de datos se utilizó para este caso de uso. La base de datos tiene una tabla denominada `formdatawithattachments` con las 4 columnas como se muestra en la captura de pantalla siguiente.
![data-base](assets/table-schema.JPG)

* La columna **afdata** albergarán los datos del formulario adaptable.
* La columna **attachmentInfo** contendrá la información sobre los archivos adjuntos del formulario.
* Las columnas **phoneNumber** contendrá el número móvil de la persona que rellena el formulario.

Cree la base de datos importando el [esquema de base de datos](assets/data-base-schema.sql)
usando MySQL workbench.

## Crear modelo de datos de formulario

Cree el modelo de datos de formulario y confíe en el origen de datos creado en el paso anterior.
Configure las variables **get** servicio de este modelo de datos de formulario como se muestra en la captura de pantalla siguiente.
Asegúrese de que no devuelve una matriz en la variable **get** servicio.

El propósito de esto **get** es recuperar el número de teléfono asociado con el id de la aplicación.

![get-service](assets/get-service.JPG)

Este modelo de datos de formulario se utilizará en la variable **MyAccountForm** para obtener el número de teléfono asociado al id de la aplicación.

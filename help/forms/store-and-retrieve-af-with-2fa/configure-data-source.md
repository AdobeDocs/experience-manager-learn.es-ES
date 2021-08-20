---
title: Configurar fuentes de datos
description: Crear DataSource apuntando a la base de datos MySQL
feature: Formularios adaptables
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Desarrollo
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 2%

---


# Configurar fuentes de datos

Existen muchas maneras en que AEM permite la integración con bases de datos externas. Una de las prácticas más comunes y estándar de integración de bases de datos es usar las propiedades de configuración de Apache Sling Connection Pooled DataSource a través de [configMgr](http://localhost:4502/system/console/configMgr).
El primer paso es descargar e implementar los [controladores MySQL](https://mvnrepository.com/artifact/mysql/mysql-connector-java) adecuados para AEM.
A continuación, establezca las propiedades de Origen de datos agrupados de la conexión de Sling específicas de la base de datos. La siguiente captura de pantalla muestra la configuración utilizada para este tutorial. El esquema de la base de datos se proporciona como parte de estos recursos de tutorial.

![fuente de datos](assets/data-source.JPG)


* Clase de controlador JDBC: `com.mysql.cj.jdbc.Driver`
* URI de conexión JDBC: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>Asegúrese de asignar un nombre a la fuente de datos `StoreAndRetrieveAfData` porque es el nombre que se usa en el servicio OSGi.


## Crear base de datos


La siguiente base de datos se utilizó para este caso de uso. La base de datos tiene una tabla llamada `formdatawithattachments` con las 4 columnas como se muestra en la captura de pantalla siguiente.
![data-base](assets/table-schema.JPG)

* La columna **afdata** contiene los datos del formulario adaptable.
* La columna **attachmentInfo** contendrá la información sobre los archivos adjuntos del formulario.
* Las columnas **phoneNumber** contendrán el número de móvil de la persona que rellene el formulario.

Cree la base de datos importando el [esquema de base de datos](assets/data-base-schema.sql)
usando MySQL workbench.

## Crear modelo de datos de formulario

Cree un modelo de datos de formulario y ciónelo en el origen de datos creado en el paso anterior.
Configure el servicio **get** de este modelo de datos de formulario como se muestra en la captura de pantalla siguiente.
Asegúrese de que no devuelve la matriz en el servicio **get**.

Este servicio **get** se utiliza para recuperar el número de teléfono asociado con el id de la aplicación.

![get-service](assets/get-service.JPG)

Este modelo de datos de formulario se utilizará en **MyAccountForm** para recuperar el número de teléfono asociado con el id de la aplicación.

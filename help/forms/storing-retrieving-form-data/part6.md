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
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---


# Implementar esto en el servidor

>[!NOTE]
>
>Para que esto se ejecute en el sistema, es necesario lo siguiente
>
>* AEM Forms(versión 6.3 o superior)
>* Base de datos MySql


Para probar esta capacidad en la instancia de AEM Forms, siga los pasos siguientes

* Descargue e implemente los archivos [Jar del controlador MySql](assets/mysqldriver.jar) mediante la consola web [felix](http://localhost:4502/system/console/bundles)
* Descargue e implemente el paquete [OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) utilizando la consola web [felix](http://localhost:4502/system/console/bundles)
* Descargue e instale el [paquete que contiene la biblioteca de cliente, la plantilla de formulario adaptable y el componente de página personalizado](assets/store-and-fetch-af-with-data.zip) mediante el [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe el [formulario adaptable de ejemplo](assets/sample-adaptive-form.zip) mediante la interfaz [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importe el [form-data-db.sql](assets/form-data-db.sql) mediante MySql Workbench. Esto creará el esquema y las tablas necesarias en la base de datos para que este tutorial funcione.
* Inicie sesión en [configMgr.](http://localhost:4502/system/console/configMgr) Busque &quot;Apache Sling Connection Pooled DataSource. Cree una nueva entrada de fuente de datos agrupada de conexión Apache Sling denominada **SaveAndContinue** con las siguientes propiedades:

| Nombre de propiedad | Value |
------------------------|---------------------------------------
| Nombre del origen de datos | SaveAndContinue |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |


* Abra el [Formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Rellene algunos detalles y haga clic en el botón &quot;Guardar y continuar más tarde&quot;.
* Debería recuperar una dirección URL con un GUID.
* Copie la dirección URL y péguela en una nueva ficha del explorador. **Asegúrese de que no haya espacio vacío al final de la dirección URL.**
* El formulario adaptable debe rellenarse con los datos del paso anterior.

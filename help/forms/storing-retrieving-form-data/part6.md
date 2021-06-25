---
title: Almacenamiento y recuperación de datos de formulario de la base de datos MySQL
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Formularios adaptables
topic: Desarrollo
role: Developer
level: Experienced
version: 6.3,6.4,6.5
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 3%

---


# Implemente esto en su servidor

>[!NOTE]
>
>Para que esto se ejecute en su sistema, es necesario lo siguiente
>
>* AEM Forms(versión 6.3 o posterior)
>* Base de datos MySql


Para probar esta capacidad en su instancia de AEM Forms, siga los siguientes pasos

* Descargue e implemente los archivos [MySql Driver Jar](assets/mysqldriver.jar) utilizando la consola web [felix](http://localhost:4502/system/console/bundles)
* Descargue e implemente el [paquete OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) utilizando la consola web [felix](http://localhost:4502/system/console/bundles)
* Descargue e instale el paquete [que contiene la biblioteca de cliente, la plantilla de formulario adaptable y el componente de página personalizado](assets/store-and-fetch-af-with-data.zip) utilizando el [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe el [formulario adaptable de ejemplo](assets/sample-adaptive-form.zip) utilizando la interfaz [FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importe el [form-data-db.sql](assets/form-data-db.sql) utilizando MySql Workbench. Esto creará el esquema y las tablas necesarios en la base de datos para que funcione este tutorial.
* Inicie sesión en [configMgr.](http://localhost:4502/system/console/configMgr) Busque Fuente de datos agrupada de la conexión Apache Sling. Cree una nueva entrada de fuente de datos agrupada de conexión Apache Sling llamada **SaveAndContinue** con las siguientes propiedades:

| Nombre de propiedad | Value |
| ------------------------|---------------------------------------|
| Nombre del origen de datos | SaveAndContinue |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

* Abra el [Formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Rellene algunos detalles y haga clic en el botón &quot;Guardar y continuar más tarde&quot;.
* Debería recuperar una URL con un GUID en ella.
* Copie la dirección URL y péguela en una nueva pestaña del explorador. **Asegúrese de que no haya espacio vacío al final de la dirección URL.**
* El formulario adaptable debe rellenarse con los datos del paso anterior.

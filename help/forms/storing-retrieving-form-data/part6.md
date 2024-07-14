---
title: 'Almacenar y recuperar datos de formulario de la base de datos MySQL: implementación'
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 2%

---

# Implementar esto en el servidor

>[!NOTE]
>
>Se requiere lo siguiente para que esto funcione en su sistema
>
>* AEM Forms (versión 6.3 o superior)
>* Base de datos MySql

Para probar esta capacidad en su instancia de AEM Forms, siga los siguientes pasos

* Descargue e implemente los archivos [MySql Driver Jar](assets/mysqldriver.jar) mediante la [consola web felix](http://localhost:4502/system/console/bundles)
* Descargue e implemente el [paquete OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) mediante la [consola web felix](http://localhost:4502/system/console/bundles)
* Descargue e instale el [paquete que contiene la biblioteca del cliente, la plantilla de formulario adaptable y el componente de página personalizada](assets/store-and-fetch-af-with-data.zip) con el [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar el [formulario adaptable de ejemplo](assets/sample-adaptive-form.zip) mediante la [interfaz FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importe [form-data-db.sql](assets/form-data-db.sql) mediante MySql Workbench. Esto creará el esquema y las tablas necesarios en la base de datos para que funcione este tutorial.
* Inicie sesión en [configMgr.](http://localhost:4502/system/console/configMgr) buscar &quot;Fuente de datos obtenida de una conexión Apache Sling&quot;. Cree una nueva entrada de origen de datos agrupado de la conexión de Apache Sling llamada **SaveAndContinue** con las siguientes propiedades:

| Nombre de la propiedad | Valor |
| ------------------------|---------------------------------------|
| Nombre de Datasource | `SaveAndContinue` |
| Clase de controlador JDBC | `com.mysql.cj.jdbc.Driver` |
| URI de conexión JDBC | `jdbc:mysql://localhost:3306/aemformstutorial` |

* Abrir [formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Complete algunos detalles y haga clic en el botón &quot;Guardar y continuar más tarde&quot;.
* Debe recuperar una dirección URL con un GUID en ella.
* Copie la dirección URL y péguela en una nueva pestaña del explorador. **Asegúrese de que no haya espacios vacíos al final de la dirección URL.**
* El formulario adaptable debe rellenarse con los datos del paso anterior.

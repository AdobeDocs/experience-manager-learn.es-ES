---
title: 'Almacenar y recuperar datos de formulario de la base de datos MySQL: implementación'
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 74
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '249'
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

* Descargue e implemente el [Jar del controlador MySql](assets/mysqldriver.jar) archivos con el [consola web felix](http://localhost:4502/system/console/bundles)
* Descargue e implemente el [Paquete OSGi](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) uso del [consola web felix](http://localhost:4502/system/console/bundles)
* Descargue e instale [paquete que contiene la biblioteca de cliente, la plantilla de formulario adaptable y el componente de página personalizada](assets/store-and-fetch-af-with-data.zip) uso del [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe el [Formulario adaptable de ejemplo](assets/sample-adaptive-form.zip) uso del [Interfaz de FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Importe el [form-data-db.sql](assets/form-data-db.sql) uso de MySql Workbench. Esto creará el esquema y las tablas necesarios en la base de datos para que funcione este tutorial.
* Inicie sesión en [configMgr.](http://localhost:4502/system/console/configMgr) Busque &quot;Fuente de datos obtenida de una conexión Apache Sling&quot;. Cree una nueva entrada de fuente de datos obtenida de una conexión Apache Sling llamada **Guardar y continuar** mediante las siguientes propiedades:

| Nombre de la propiedad | Valor |
| ------------------------|---------------------------------------|
| Nombre de Datasource | Guardar y continuar |
| Clase de controlador JDBC | com.mysql.cj.jdbc.Driver |
| URI de conexión JDBC | jdbc:mysql://localhost:3306/aemformstutorial |

* Abra el [Formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Complete algunos detalles y haga clic en el botón &quot;Guardar y continuar más tarde&quot;.
* Debe recuperar una dirección URL con un GUID en ella.
* Copie la dirección URL y péguela en una nueva pestaña del explorador. **Asegúrese de que no haya espacios vacíos al final de la dirección URL.**
* El formulario adaptable debe rellenarse con los datos del paso anterior.

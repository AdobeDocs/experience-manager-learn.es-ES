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
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Implementar esto en el servidor

>[!NOTE]
>
>Para que esto se ejecute en el sistema, es necesario lo siguiente
>
>* AEM Forms(versión 6.3 o superior)
>* Base de datos MYSQL


Para probar esta capacidad en la instancia de AEM Forms, siga los pasos siguientes

* Descargue y descomprima los recursos [del](assets/store-retrieve-form-data.zip) tutorial en su sistema local
* Implementar y inicio los paquetes techMarkingdemos.jar y mysqldriver.jar usando la consola web [Felix](http://localhost:4502/system/console/configMgr)
* Importe aemformstutorial.sql mediante MYSQL Workbench. Esto creará el esquema y las tablas necesarias en la base de datos para que este tutorial funcione.
* Importe StoreAndRetrieve.zip mediante [AEM administrador de paquetes.](http://localhost:4502/crx/packmgr/index.jsp) Este paquete contiene la plantilla Formulario adaptable, la biblioteca de cliente de componentes de página y la configuración de origen de datos y formulario adaptable de ejemplo.
* Inicie sesión en [configMgr.](http://localhost:4502/system/console/configMgr) Busque &quot;Apache Sling Connection Pooled DataSource. Abra la entrada del origen de datos asociada con un tutorial de datos e introduzca el nombre de usuario y la contraseña específicos de la instancia de base de datos.
* Abrir el formulario [adaptable](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* Rellene algunos detalles y haga clic en el botón &quot;Guardar y continuar más tarde&quot;
* Debería recuperar una dirección URL con un GUID.
* Copie la dirección URL y péguela en una nueva ficha del explorador. **Asegúrese de que no haya espacio vacío al final de la dirección URL**
* El formulario adaptable debe rellenarse con los datos del paso anterior

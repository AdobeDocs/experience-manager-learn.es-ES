---
title: Almacenamiento de datos de formulario adaptable
seo-title: Almacenamiento de datos de formulario adaptable
description: Almacenamiento de datos de formulario adaptables en DataBase como parte de su flujo de trabajo de AEM
seo-description: Almacenamiento de datos de formulario adaptables en DataBase como parte de su flujo de trabajo de AEM
feature: Adaptive Forms,Workflow,Form Data Model
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---


# Almacenamiento de envíos de formularios adaptables en la base de datos

Existen varias formas de almacenar los datos de formulario enviados en la base de datos que elija. Se puede utilizar una fuente de datos JDBC para almacenar directamente los datos en la base de datos. Se puede escribir un paquete OSGI personalizado para almacenar los datos en la base de datos. Este artículo utiliza el paso de proceso personalizado en el flujo de trabajo de AEM para almacenar los datos.
El caso de uso es activar un flujo de trabajo de AEM en un envío de formulario adaptable y un paso en el flujo de trabajo almacena los datos enviados en la base de datos.

**Siga los pasos que se indican a continuación para que esto funcione en su sistema**

* [Descargue el archivo Zip y extraiga su contenido en su disco duro](assets/storeafdataindb.zip)

   * Importe StoreAFInDBWorkflow.zip en AEM mediante el administrador de paquetes. El paquete tiene un flujo de trabajo de muestra que almacena los datos AF en la base de datos. Abra el modelo de flujo de trabajo. El flujo de trabajo solo tiene un paso. Este paso llama al código escrito en el paquete para almacenar los datos AF en la base de datos. Estoy pasando un solo argumento al proceso. Es el nombre del formulario adaptable cuyos datos se están guardando.
   * Implemente insertdata.core-0.0.1-SNAPSHOT.jar mediante la consola web Felix. Este paquete tiene el código para escribir los datos de formulario enviados en la base de datos

* Vaya a [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * Busque &quot;JDBC Connection Pool&quot;. Cree un nuevo grupo de conexiones JDBC Day Commons. Especifique la configuración específica de la base de datos.

   * ![grupo de conexiones jdbc](assets/jdbc-connection-pool.png)
   * Busque &quot;**Insertar datos de formulario en la base de datos**&quot;
   * Especifique las propiedades específicas de la base de datos.
      * DataSourceName:Nombre del origen de datos que configuró anteriormente.
      * TableName - Nombre de la tabla en la que desea almacenar los datos AF
      * FormName - Nombre de columna que contiene el nombre del formulario
      * ColumnName - Nombre de columna para contener los datos AF

   ![insertdata](assets/insertdata.PNG)

* Crear un formulario adaptable.

* Asocie el formulario adaptable con el flujo de trabajo de AEM (StoreAFValuesinDB) como se muestra en la captura de pantalla siguiente.

* Asegúrese de especificar &quot;data.xml&quot; en la ruta del archivo de datos como se muestra en la captura de pantalla siguiente

   ![presentación](assets/submissionafforms.png)

* Vista previa del formulario y envío

* Si todo ha ido bien, debería ver los datos del formulario almacenados en la tabla y columna que haya especificado




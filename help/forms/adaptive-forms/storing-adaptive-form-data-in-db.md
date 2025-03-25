---
title: Almacenar datos de formularios adaptables
description: Almacenar datos de formulario adaptables en la base de datos como parte del flujo de trabajo de AEM
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
last-substantial-update: 2020-03-20T00:00:00Z
duration: 146
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---

# Almacenar envíos de formularios adaptables en la base de datos

Existen varias formas de almacenar los datos del formulario enviado en la base de datos que elija. Se puede utilizar una fuente de datos JDBC para almacenar directamente los datos en la base de datos. Se puede escribir un paquete OSGI personalizado para almacenar los datos en la base de datos. Este artículo utiliza el paso de proceso personalizado del flujo de trabajo de AEM para almacenar los datos.
El caso de uso es almacenar en déclencheur un flujo de trabajo de AEM en un envío de formulario adaptable y un paso del flujo de trabajo almacena los datos enviados en la base de datos.



## Grupo de conexión JDBC

* Ir a [Configuration Manager](http://localhost:4502/system/console/configMgr)

   * Busque &quot;Grupo de conexión JDBC&quot;. Cree un nuevo grupo de conexiones JDBC de Day Commons. Especifique la configuración específica de la base de datos.

   * ![Configuración OSGi del grupo de conexión JDBC](assets/aemformstutorial-jdbc.png)

## Especificar detalles de base de datos

* Buscar &quot;**Especifique los detalles de la base de datos**&quot;
* Especifique las propiedades específicas de la base de datos.
   * DataSourceName: nombre del origen de datos configurado anteriormente.
   * TableName: nombre de la tabla en la que desea almacenar los datos AF
   * FormName: nombre de columna que contiene el nombre del formulario.
   * ColumnName: nombre de columna que contiene los datos AF

  ![Especificar detalles de la base de datos para la configuración OSGi](assets/specify-database-details.png)



## Código para la configuración de OSGi

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## Leer valores de configuración

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## Código para implementar el paso del proceso

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## Implementar los recursos de muestra

* Asegúrese de haber configurado el grupo de conexiones JDBC
* Especifique los detalles de la base de datos mediante configMgr
* [Descargue el archivo Zip y extraiga su contenido en el disco duro](assets/article-assets.zip)

   * Implemente el archivo jar mediante [la consola web de AEM](http://localhost:4502/system/console/bundles). Este archivo jar contiene el código para almacenar los datos del formulario en la base de datos.

   * Importe los dos archivos zip en [AEM mediante el administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp). Esto le proporcionará [el flujo de trabajo de ejemplo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html) y [el formulario adaptable de ejemplo](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) que almacenará en déclencheur el flujo de trabajo al enviar el formulario. Observe los argumentos de proceso en el paso del flujo de trabajo. Estos argumentos indican el nombre del formulario y el nombre del archivo de datos que contendrá los datos del formulario adaptable. El archivo de datos se almacena en la carpeta de carga útil del repositorio crx. Observe cómo el [formulario adaptable](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) está configurado para almacenar en déclencheur el flujo de trabajo de AEM al enviar y la configuración del archivo de datos (data.xml)

   * Obtenga una vista previa, rellene el formulario y envíelo. Debería ver una fila nueva creada en la base de datos


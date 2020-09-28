---
title: AEM Forms con Esquema y datos JSON[Parte 2]
seo-title: AEM Forms con Esquema y datos JSON[Parte 2]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
seo-description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# Almacenamiento de datos enviados en la base de datos


>[!NOTE]
>
>Se recomienda utilizar MySQL 8 como base de datos ya que es compatible con el tipo de datos JSON. También necesitará instalar el controlador apropiado para MySQL DB. He utilizado el controlador disponible en esta ubicación https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

Para almacenar los datos enviados en la base de datos, escribiremos un servlet para extraer los datos enlazados y el nombre del formulario y almacenar los datos. A continuación se proporciona el código completo para gestionar el envío del formulario y almacenar los datos afBoundData en la base de datos.

Hemos creado un envío personalizado para gestionar el envío del formulario. En este post.POST.jsp de envío personalizado reenviamos la solicitud a nuestro servlet.

Para obtener más información sobre los motivos de envío personalizados, lea este [artículo](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmit&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connectionpool](assets/connectionpooled.gif)

Para que esto funcione, siga los siguientes pasos

* [Descargar y descomprimir el archivo zip](assets/aemformswithjson.zip)
* Crear AdaptiveForm con Esquema JSON. Puede utilizar el esquema JSON que se proporciona como parte de los recursos de este artículo. Asegúrese de que la acción de envío del formulario está configurada correctamente. La acción de envío debe configurarse en &quot;CustomSubmitHelpx&quot;.
* Cree un esquema en la instancia de MySQL importando el archivo esquema.sql con la herramienta de MySQL Workbench. El archivo esquema.sql también se proporciona como parte de este tutorial.
* Configurar el origen de datos agrupados de conexión Apache Sling desde la consola web Felix
* Asegúrese de asignar un nombre al nombre del origen de datos &quot;aemformswithjson&quot;. Este es el nombre que utiliza el paquete OSGi de muestra que se le proporciona
* Consulte la imagen de arriba para ver las propiedades. Esto supone que va a usar MySQL como base de datos.
* Implemente los paquetes OSGi que se proporcionan como parte de los recursos de este artículo.
* Previsualización del formulario y envío.
* Los datos JSON se almacenarán en la base de datos que se creó al importar el archivo &quot;esquema.sql&quot;.

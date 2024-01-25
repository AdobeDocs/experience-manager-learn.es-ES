---
title: AEM Forms con esquema y datos JSON [Part2]
description: Tutorial de varias partes para guiarle por los pasos necesarios para crear un formulario adaptable con esquema JSON y consultar los datos enviados.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
duration: 110
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Almacenar datos enviados en la base de datos


>[!NOTE]
>
>Se recomienda utilizar MySQL 8 como base de datos, ya que es compatible con el tipo de datos JSON. También deberá instalar el controlador apropiado para MySQL DB. He utilizado el controlador disponible en esta ubicación https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

Para almacenar los datos enviados en la base de datos, escribiremos un servlet para extraer los datos enlazados y el nombre del formulario y almacenarlo. A continuación se proporciona el código completo para administrar el envío del formulario y almacenar afBoundData en la base de datos.

Hemos creado envíos personalizados para gestionar el envío del formulario. En el post.POST.jsp de este envío personalizado, reenviamos la solicitud a nuestro servlet.

Para obtener más información sobre los envíos personalizados, lea esto [artículo](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

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

Para que esto funcione en su sistema, siga los siguientes pasos

* [Descargue y descomprima el archivo zip](assets/aemformswithjson.zip)
* Crear formulario adaptable con esquema JSON. Puede utilizar el esquema JSON proporcionado como parte de los recursos de este artículo. Asegúrese de que la acción de envío del formulario está configurada correctamente. La acción de envío debe configurarse como &quot;CustomSubmitHelpx&quot;.
* Cree un esquema en la instancia de MySQL importando el archivo schema.sql con la herramienta MySQL Workbench. El archivo schema.sql también se proporciona como parte de estos recursos de tutorial.
* Configure la fuente de datos obtenida de una conexión Apache Sling desde la consola web de Felix
* Asegúrese de nombrar su nombre de fuente de datos &quot;aemformswithjson&quot;. Este es el nombre que utiliza el paquete OSGi de muestra que se le proporciona
* Consulte la imagen anterior para ver las propiedades. Esto supone que va a usar MySQL como base de datos.
* Implemente los paquetes OSGi que se proporcionan como parte de los recursos de este artículo.
* Previsualice el formulario y envíelo.
* Los datos JSON se almacenan en la base de datos que se creó al importar el archivo schema.sql.

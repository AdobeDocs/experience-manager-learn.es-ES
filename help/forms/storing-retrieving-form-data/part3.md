---
title: Almacenamiento y recuperación de datos de formulario de la base de datos MySQL
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 5%

---


# Creación del servicio OSGi para recuperar datos

El siguiente código se escribió para almacenar y recuperar los datos almacenados del formulario adaptable. Se utiliza una consulta simple para recuperar los datos del formulario adaptable asociados a un GUID determinado. A continuación, los datos recuperados se devuelven a la aplicación que realiza la llamada. En este código se hace referencia a la misma fuente de datos creada en el paso anterior.


```java
package com.aemforms.saveandcontinue.core.impl;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.UUID;

import javax.sql.DataSource;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class FetchFormData implements com.aemforms.saveandcontinue.core.FetchStoredFormData {
  private final Logger log = LoggerFactory.getLogger(getClass());@Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=SaveAndContinue))")
  private DataSource dataSource;@Override
  public String getData(String guid) {
    log.debug("### inside my getData of AemformWithDB");
    Connection con = getConnection();
    try {
      Statement st = con.createStatement();
      String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '" + guid + "'" + "";
      log.debug(" Got Result Set" + query);
      ResultSet rs = st.executeQuery(query);
      while (rs.next()) {
        return rs.getString("afdata");
      }
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }

    return null;
  }
  public Connection getConnection() {
    log.debug("Getting Connection ");
    Connection con = null;
    try {

      con = dataSource.getConnection();
      log.debug("got connection");
      return con;
    } catch(Exception e) {
      log.debug("not able to get connection " + e.getMessage());

    }
    return null;
  }

  public String storeFormData(String formData) {
    // TODO Auto-generated method stub
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData);

    String insertRowSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
      pstmt = null;
      pstmt = c.prepareStatement(insertRowSQL);
      pstmt.setString(1, randomUUIDString);
      pstmt.setString(2, formData);
      log.debug("Executing the insert statment  " + pstmt.executeUpdate());
      c.commit();
    } catch(SQLException e) {

      log.error("unable to insert data in the table", e.getMessage());
    } finally {
      if (pstmt != null) {
        try {
          pstmt.close();
        } catch(SQLException e) {
          log.debug("error in closing prepared statement " + e.getMessage());
        }
      }
      if (c != null) {
        try {
          c.close();
        } catch(SQLException e) {
          log.debug("error in closing connection " + e.getMessage());
        }
      }
    }
    return randomUUIDString;
  }@Override
  public String updateData(String guid, String afData) {
    String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {

      pstmt = null;
      pstmt = c.prepareStatement(updateTableSQL);
      pstmt.setString(1, afData);
      pstmt.setString(2, guid);
      log.debug("Executing the insert statment  " + pstmt.executeUpdate());

    } catch(SQLException e) {

      log.debug("Getting errors", e.getMessage());
    } finally {
      if (pstmt != null) {
        try {
          pstmt.close();
        } catch(SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
      }
      if (c != null) {
        try {
          c.close();
        } catch(SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
      }
    }
    return guid;

  }

}
```

## Interfaz

A continuación se muestra la declaración de interfaz que se utilizó

```java
package com.aemforms.saveandcontinue.core;

public interface FetchStoredFormData 
{public String getData(String guid);
public String storeFormData(String formData);
public String updateData(String guid,String afData);

}
```

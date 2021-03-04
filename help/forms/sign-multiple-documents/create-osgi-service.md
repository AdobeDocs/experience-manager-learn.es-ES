---
title: Creación del servicio OSGi
description: Creación del servicio OSGi para almacenar los formularios que se van a firmar
feature: formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 6886.jpg
kt: 6886
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 0%

---


# Creación del servicio OSGi

Se escribió el siguiente código para almacenar los formularios que deben firmarse. Cada formulario que se va a firmar está asociado con una guía única y un ID de cliente. Por lo tanto, uno o varios formularios se pueden asociar al mismo ID de cliente, pero tendrán un GUID único asignado al formulario.

## Interfaz

A continuación se muestra la declaración de interfaz que se utilizó

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## Insertar datos

El método insert data inserta una fila en la base de datos identificada por el origen de datos. Cada fila de la base de datos corresponde a un formulario y se identifica de forma única mediante un GUID y un ID de cliente. Los datos del formulario y la dirección URL del formulario también se almacenan en esta fila. La columna de estado es para indicar si el formulario se ha rellenado y firmado. El valor 0 indica que el formulario aún no se ha firmado.

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## Obtener datos del formulario

El siguiente código se utiliza para recuperar datos de formulario adaptables asociados con el GUID dado. A continuación, los datos del formulario se utilizan para rellenar previamente el formulario adaptable.

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## Actualizar estado de firma

La finalización correcta de la ceremonia de firma desencadena un flujo de trabajo de AEM asociado al formulario. El primer paso en el flujo de trabajo es un paso de proceso que actualiza el estado de la base de datos para la fila identificada por la guía y el ID de cliente. También establecemos el valor del elemento firmado en los datos del formulario en Y para indicar que el formulario se ha rellenado y firmado. El formulario adaptable se rellenará con estos datos y el valor del elemento de datos firmado en los datos xml se utilizará para mostrar el mensaje adecuado. El código updateSignatureStatus se invoca desde el paso de proceso personalizado.


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## Obtener el siguiente formulario para firmar

El siguiente código se utilizó para obtener el siguiente formulario para firmar para un customerID determinado con un estado de 0. Si la consulta sql no devuelve ninguna fila, devolvemos la cadena **&quot;AllDone&quot;** que indica que no hay más formularios para firmar para el id de cliente dado.

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Assets

El paquete OSGi con los servicios mencionados anteriormente se puede [descargar desde aquí](assets/sign-multiple-forms.jar)
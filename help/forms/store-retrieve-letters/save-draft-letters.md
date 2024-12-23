---
title: Guardar y recuperar borradores de cartas
description: Obtenga información sobre cómo guardar y recuperar borradores de cartas
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: dc6f64a0-7059-4392-9c29-e66bdef4fd4d
duration: 116
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Guardar y recuperar borradores de cartas

El siguiente código se utiliza para guardar la instancia de carta. Los metadatos de la instancia de carta se almacenan en la tabla _icdrafts_. Se genera y devuelve una cadena única (DraftID). Esta cadena única se utiliza para recuperar la instancia de carta guardada.

```java
public String save(CCRDocumentInstance letterToSave) throws CCRDocumentException {
  String insertRowSQL = "INSERT INTO aemformstutorial.icdrafts(draftID,documentID,status,owner,name) VALUES(?,?,?,?,?)";
  log.debug(" in save IC Draft" + letterToSave.getDocumentId() + letterToSave.getName());
  UUID uuid = UUID.randomUUID();
  String uuidString = uuid.toString();
  Connection connection = getConnection();
  PreparedStatement pstmt = null;
  Document icData = letterToSave.getData();
  try {
    pstmt = connection.prepareStatement(insertRowSQL);
    pstmt.setString(1, uuidString);
    pstmt.setString(2, letterToSave.getDocumentId());
    pstmt.setString(3, "DRAFT");
    pstmt.setString(4, letterToSave.getCreatedBy());
    pstmt.setString(5, letterToSave.getName());
    icData.copyToFile(new File(uuidString + ".xml"));
    log.debug("Executing the insert statment  " + pstmt.executeUpdate());
    connection.commit();
  } catch (IOException | SQLException e) {
    log.debug("The error is " + e.getMessage());
  } finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch (SQLException e) {
        log.debug("Error in closing prepared statment" + e.getMessage());
      }
    }
    if (connection != null) {
      try {
        log.debug("Closing the connection in Save Letter Draft");
        connection.close();
      } catch (SQLException e) {
        log.debug("Error in closing connection" + e.getMessage());
      }
    }

  }

  return uuidString;
}
```

## Obtener carta

El siguiente código se escribió para recuperar el borrador de la carta guardada.
Para cargar instancias de carta guardada, debe proporcionar el ID de borrador. En función de este ID de borrador, consultamos la base de datos para obtener los metadatos adicionales sobre la carta. El mismo DraftID se utiliza para crear los datos de la carta leyendo el xml correspondiente del sistema de archivos. A continuación, se construye y se devuelve un objeto CCRDocumentInstance.


```java
@Override
public CCRDocumentInstance get(String draftID) throws CCRDocumentException {

  String selectStatement = "Select documentID from aemformstutorial.icdrafts where draftID='" + draftID + "'";
  log.debug("The select statement is " + selectStatement);
  Connection connection = getConnection();
  Statement statement = null;
  String documentID = "";
  try {
    statement = connection.createStatement();
    ResultSet rs = statement.executeQuery(selectStatement);
    while (rs.next()) {
      documentID = rs.getString("documentID");

    }
  } catch (SQLException e) {
    log.debug("The error is " + e.getMessage());
  }
  Document draftData = new Document(new File(draftID + ".xml"));
  CCRDocumentInstance draftInstance = new CCRDocumentInstance(draftData, "abc", documentID, CCRDocumentInstance.Status.DRAFT);
  draftInstance.setId(draftID);
  return draftInstance;
}
```

### Actualizar carta

El siguiente código se utilizó para actualizar la instancia de carta guardada. Los datos de la carta actualizada se escriben en el sistema de archivos mediante el ID de la carta.

```java
public void update(CCRDocumentInstance letterInstanceToUpdate) throws CCRDocumentException {
        Document icData = letterInstanceToUpdate.getData();
        String draftID = letterInstanceToUpdate.getId();
        log.debug("updating letter instance with draft id =  "+draftID);
        try
            {
                icData.copyToFile(new File(draftID+".xml"));
            } 
        catch (IOException e)
            {
                log.debug("Error updating "+e.getMessage());;
            }
        
    }
```

### Obtener todas las cartas guardadas

AEM Forms no proporciona ninguna interfaz de usuario predeterminada para enumerar las cartas guardadas. Para este artículo, enumere las instancias de carta guardadas en formato tabular mediante un formulario adaptable.
Puede personalizar la consulta para recuperar las instancias de carta guardadas. En este ejemplo, estoy consultando la instancia de carta guardada por &quot;admin&quot;.

```java
    public List < CCRDocumentInstance > getAll(String arg0, Date arg1, Date arg2, Map < String, Object > arg3) throws CCRDocumentException {
      String selectStatement = "Select * from aemformstutorial.icdrafts where owner = 'admin'";
      Connection connection = getConnection();
      Statement statement = null;
      String documentID = "";
      List < CCRDocumentInstance > listOfDrafts = new ArrayList < CCRDocumentInstance > ();
      String draftID;
      String savedInstanceName = "";
      try {
        statement = connection.createStatement();
        ResultSet rs = statement.executeQuery(selectStatement);
        while (rs.next()) {
          documentID = rs.getString("documentID");
          draftID = rs.getString("draftID");
          savedInstanceName = rs.getString("name");
          Document draftData = new Document(new File(draftID + ".xml"));
          CCRDocumentInstance draftLetter = new CCRDocumentInstance(draftData, savedInstanceName, documentID, CCRDocumentInstance.Status.DRAFT);
          listOfDrafts.add(draftLetter);
        }
      } catch (SQLException e) {
        log.debug("The error is " + e.getMessage());
      } finally {
        if (statement != null) {
          try {
            statement.close();
          } catch (SQLException e) {
            log.debug("error in closing statement" + e.getMessage());
          }
        }
        if (connection != null) {
          try {
            connection.close();
          } catch (SQLException e) {
            log.debug("error in closing connection" + e.getMessage());
          }
        }
      }

      return listOfDrafts;
    }
```

### Proyecto Eclipse

El proyecto Eclipse con implementación de ejemplo se puede [descargar desde aquí](assets/icdrafts-eclipse-project.zip)

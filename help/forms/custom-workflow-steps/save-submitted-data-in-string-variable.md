---
title: Guardar datos enviados en una variable de cadena
description: Paso de proceso personalizado para extraer datos enlazados y guardarlos en una variable de flujo de trabajo de tipo cadena
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-11199
source-git-commit: 5277b7a6ceba4473ab2808f980c8faa5bf69c757
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Extraer datos enlazados y guardarlos en una variable de cadena

Esta capacidad le permite incluir los datos enviados en el cuerpo del correo electrónico. El paso de proceso personalizado extrae el **datos enlazados** del envío del formulario adaptable y rellena una variable de tipo cadena con los datos. A continuación, puede utilizar esta variable de cadena para insertar los datos en la plantilla de correo electrónico.
La siguiente captura de pantalla muestra los argumentos que debe pasar al paso de proceso personalizado
![paso del proceso](assets/save-submitted-data-string.png)

Los siguientes son los parámetros

* `data.xml` - El archivo que tiene los datos enviados . Si el formato está en json, el nombre del archivo puede ser data.json

El paso de proceso personalizado extraerá los datos enlazados y los almacenará en la variable submitDataString definida en el flujo de trabajo


[El paquete personalizado se puede descargar desde aquí](assets/AEMFormsProcessStep.core-1.0.0-SNAPSHOT.jar)

```java
package AEMFormsProcessStep.core;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import javax.jcr.Session;
import com.google.gson.Gson;
import com.google.gson.JsonObject;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;
import org.w3c.dom.Document;
import javax.xml.parsers.DocumentBuilder;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.StringWriter;

import javax.jcr.Node;

@Component(property = {
  "service.description=Save submitted Payload as String",
  "process.label=Save submitted Payload as String"
})
public class SaveSubmittedDataInStringVariable implements WorkflowProcess {
  private Logger log;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap) throws WorkflowException {

    String submittedDataFile = ((String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string")).toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log = LoggerFactory.getLogger(SaveSubmittedDataInStringVariable.class);

    String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
    Session session = (Session) workflowSession.adaptTo((Class) Session.class);
    Node submittedDataNode = null;
    try {
      submittedDataNode = session.getNode(dataFilePath);
      if (submittedDataFile.endsWith("json")) {
        InputStream jsonDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
        BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        while ((inputStr = streamReader.readLine()) != null) {
          responseStrBuilder.append(inputStr);
        }
        JsonObject jsonObject = new Gson().fromJson(responseStrBuilder.toString(), JsonObject.class);
        JsonObject boundData = jsonObject.getAsJsonObject("afData").getAsJsonObject("afBoundData").getAsJsonObject("data");
        System.out.println(jsonObject.getAsJsonObject("afData").getAsJsonObject("afBoundData").getAsJsonObject("data"));
        metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
        metaDataMap.put("submittedDataString", boundData.toString());

      }
      if (submittedDataFile.endsWith("xml")) {
        log.debug("Got xml file");
        DocumentBuilderFactory factory = null;
        DocumentBuilder builder = null;
        Document xmlDocument = null;
        InputStream xmlDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
        System.out.println("Got xml file" + submittedDataNode);
        XPath xPath = XPathFactory.newInstance().newXPath();
        factory = DocumentBuilderFactory.newInstance();
        builder = factory.newDocumentBuilder();
        xmlDocument = builder.parse(xmlDataStream);

        org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afBoundData").evaluate(xmlDocument, XPathConstants.NODE);
        Transformer transformer = TransformerFactory.newInstance().newTransformer();
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");
        StreamResult result = new StreamResult(new StringWriter());
        DOMSource source = new DOMSource(node);
        transformer.transform(source, result);
        String xmlString = result.getWriter().toString();
        log.debug("The xml string is " + xmlString);
        metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
        metaDataMap.put("submittedDataString", xmlString);
      }

    } catch (Exception e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

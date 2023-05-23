---
title: Crear flujo de trabajo principal para almacenar en déclencheur el proceso de firma
description: Cree un flujo de trabajo para almacenar los formularios para su firma en la base de datos
feature: Adaptive Forms
version: 6.4,6.5
thumbnail: 6887.jpg
kt: 6887
topic: Development
role: Developer
level: Intermediate
exl-id: 338d9522-f6da-4aa7-b5d8-b9fff39ea94b
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---

# Crear flujo de trabajo principal

El flujo de trabajo principal se activa cuando el usuario envía el formulario inicial (**RefinanciarFormulario**). A continuación se muestra el flujo de trabajo

![main-workflow](assets/main-workflow.PNG)

**Almacenar Forms Para Firmar** es un paso de proceso personalizado.

AEM La motivación para implementar un paso de proceso personalizado es ampliar un flujo de trabajo de. El siguiente código implementa un paso de proceso personalizado. El código extrae los nombres de los formularios que se van a firmar y pasa los datos del formulario enviado a `insertData` en el servicio SignMultipleForms. El `insertData` a continuación, inserta las filas en la base de datos identificada por el origen de datos **aemformstutorial**.

El código de este paso de proceso personalizado hace referencia a `SignMultipleForms` servicio.



```java
package com.aem.forms.signmultipleforms;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.Charset;
import java.nio.charset.StandardCharsets;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=StoreFormsToSign",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=StoreFormsToSign"
})
public class StoreFormsToSignWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(StoreFormsToSignWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log.debug("The payload  in StoreFormsToSign " + workItem.getWorkflowData().getPayload().toString());
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    String serverURL = arg2.get("PROCESS_ARGS", "string").toString();
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/formsToSign").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      log.debug("The form names to sign are  t" + node.getTextContent());
      String formNamesToSign[] = node.getTextContent().split(",");
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      DOMSource source = new DOMSource(xmlDocument);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      InputStream inputStream = new ByteArrayInputStream(outputStream.toByteArray());
      StringWriter writer = new StringWriter();
      IOUtils.copy(inputStream, writer, Charset.defaultCharset());
      String formData = writer.toString();
      signMultipleForms.insertData(formNamesToSign, formData, serverURL, workItem, workflowSession);

  }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }
}
```




## Assets

El flujo de trabajo Firmar varios Forms utilizado en este artículo se puede [descargado desde aquí](assets/sign-multiple-forms-workflows.zip)

>[!NOTE]
> Asegúrese de configurar Day CQ Mail Service para enviar notificaciones por correo electrónico. La plantilla de correo electrónico también se proporciona en el paquete anterior.

## Pasos siguientes

[Actualizar estado de firma al firmar documento](./update-signature-status.md)

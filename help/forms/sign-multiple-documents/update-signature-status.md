---
title: Actualizar el estado de la firma del formulario en la base de datos
description: AEM Actualizar el estado de firma del formulario firmado en la base de datos mediante el flujo de trabajo de la
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6888
thumbnail: 6888.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 75852a4b-7008-4c65-bab1-cc5dbf525e20
duration: 54
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 2%

---

# Actualizar estado de firma

El flujo de trabajo UpdateSignatureStatus se activa cuando el usuario ha completado la ceremonia de firma. A continuación se muestra el flujo de trabajo

![main-workflow](assets/update-signature.PNG)

Actualizar el estado de la firma es un paso de proceso personalizado.
AEM El motivo principal para implementar el paso de proceso personalizado es ampliar un flujo de trabajo de. El siguiente es el código personalizado que se utiliza para actualizar el estado de la firma.
El código de este paso de proceso personalizado hace referencia al servicio SignMultipleForms.


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## Assets

El flujo de trabajo de actualización del estado de firma puede ser [descargado desde aquí](assets/update-signature-status-workflow.zip)

## Pasos siguientes

[Personalizar el paso de resumen para mostrar el siguiente formulario para firmar](./customize-summary-component.md)

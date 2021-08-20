---
title: Actualizar el estado de firma del formulario en la base de datos
description: Actualizar el estado de firma del formulario firmado en la base de datos mediante el flujo de trabajo AEM
feature: Formularios adaptables
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 3%

---


# Actualizar estado de firma

El flujo de trabajo UpdateSignatureStatus se activa cuando el usuario ha completado la ceremonia de firma. A continuación se muestra el flujo del flujo de trabajo

![main-workflow](assets/update-signature.PNG)

Actualizar estado de firma es un paso de proceso personalizado.
La razón principal para implementar el paso de proceso personalizado es ampliar un flujo de trabajo AEM. El siguiente es el código personalizado utilizado para actualizar el estado de firma.
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

El flujo de trabajo de estado de la firma de actualización se puede [descargar desde aquí](assets/update-signature-status-workflow.zip)


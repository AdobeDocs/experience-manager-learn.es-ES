---
title: Personalizar notificación de asignación de tarea
description: Incluir datos de formulario en los correos electrónicos de notificación de asignación de tarea
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
last-substantial-update: 2020-07-07T00:00:00Z
duration: 180
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 1%

---

# Personalizar notificación de asignación de tarea

El componente Asignar tarea se utiliza para asignar tareas a los participantes del flujo de trabajo. Cuando se asigna una tarea a un usuario o grupo, se envía una notificación por correo electrónico al usuario o a los miembros del grupo definidos.
Esta notificación por correo electrónico generalmente contiene datos dinámicos relacionados con la tarea. Estos datos dinámicos se recuperan mediante el sistema generado [propiedades de metadatos](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification).
Para incluir valores de los datos de formulario enviados en la notificación por correo electrónico, es necesario crear una propiedad de metadatos personalizada y, a continuación, utilizar estas propiedades de metadatos personalizadas en la plantilla de correo electrónico



## Crear una propiedad de metadatos personalizada

El método recomendado es crear un componente OSGI que implemente el método getUserMetadata del [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

El siguiente código crea 4 propiedades de metadatos (_firstName_,_lastName_,_razonar_ y _amountRequested_) y establece su valor a partir de los datos enviados. Por ejemplo, la propiedad metadata _firstName_ El valor de se establece en el valor del elemento llamado firstName de los datos enviados. El siguiente código supone que los datos enviados del formulario adaptable están en formato xml. Los Forms adaptables basados en el esquema JSON o en el modelo de datos de formulario generan datos en formato JSON.


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## Usar las propiedades de metadatos personalizadas en la plantilla de correo electrónico de notificación de tareas

En la plantilla de correo electrónico puede incluir la propiedad de metadatos mediante la siguiente sintaxis, donde amountRequested es la propiedad de metadatos `${amountRequested}`

## Configurar la asignación de tareas para utilizar la propiedad de metadatos personalizada

AEM Una vez que el componente OSGi se haya creado e implementado en el servidor, configure el componente Asignar tarea, tal y como se muestra a continuación, para usar propiedades de metadatos personalizadas.


![Notificación de tarea](assets/task-notification.PNG)

## Habilitar el uso de propiedades de metadatos personalizadas

![Propiedades de metadatos personalizadas](assets/custom-meta-data-properties.PNG)

## Para probar esto en el servidor

* [Configurar el servicio Day CQ Mail](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* Asocie un ID de correo electrónico válido con [usuario administrador](http://localhost:4502/security/users.html)
* Descargue e instale [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) usando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Descargar [Formulario adaptable](assets/request-travel-authorization.zip) AEM e importar a la desde el [iu de formularios y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Implementación e inicio de [Paquete personalizado](assets/work-items-user-service-bundle.jar) uso del [consola web](http://localhost:4502/system/console/bundles)
* [Vista previa y envío del formulario](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Al enviar el formulario, se envía una notificación de asignación de tareas al ID de correo electrónico asociado al usuario administrador. La siguiente captura de pantalla muestra una notificación de asignación de tareas de ejemplo

![Notificación](assets/task-nitification-email.png)

>[!NOTE]
>La plantilla de correo electrónico para la notificación de asignación de tareas debe tener el siguiente formato.
>
> subject=Tarea asignada - `${workitem_title}`
>
> message=Cadena que representa la plantilla de correo electrónico sin caracteres de línea nuevos.

## Comentarios de la tarea en la notificación de correo electrónico Asignar tarea

En algunos casos, es posible que desee incluir los comentarios del propietario de la tarea anterior en las notificaciones de tareas posteriores. El código para capturar el último comentario de la tarea se muestra a continuación:

```java
package samples.aemforms.taskcomments.core;

import org.osgi.service.component.annotations.Component;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.HistoryItem;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=A sample implementation of a user metadata service.",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Capture Workflow Comments"
})

public class CaptureTaskComments implements WorkitemUserMetadataService {
  private static final Logger log = LoggerFactory.getLogger(CaptureTaskComments.class);
  @Override
  public Map <String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metadataMap) {
    HashMap < String, String > customMetadataMap = new HashMap < String, String > ();
    workflowSession.adaptTo(Session.class);
    try {
      List <HistoryItem> workItemsHistory = workflowSession.getHistory(workItem.getWorkflow());
      int listSize = workItemsHistory.size();
      HistoryItem lastItem = workItemsHistory.get(listSize - 1);
      String reviewerComments = (String) lastItem.getWorkItem().getMetaDataMap().get("workitemComment");
      log.debug("####The comment I got was ...." + reviewerComments);
      customMetadataMap.put("comments", reviewerComments);
      log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

    } catch (Exception e) {
      log.debug(e.getMessage());
    }
    return customMetadataMap;
  }

}
```

El paquete con el código anterior puede ser [descargado desde aquí](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)

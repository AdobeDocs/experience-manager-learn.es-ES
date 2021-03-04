---
title: Personalizar notificación para asignar tareas
description: Incluir datos de formulario en los mensajes de correo electrónico de notificación de tareas asignadas
sub-product: formularios
feature: Flujo de trabajo
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 2%

---


# Personalizar notificación para asignar tareas

El componente Asignar tarea se utiliza para asignar tareas a los participantes del flujo de trabajo. Cuando se asigna una tarea a un usuario o grupo, se envía una notificación por correo electrónico a los miembros definidos del usuario o grupo.
Esta notificación por correo electrónico generalmente contiene datos dinámicos relacionados con la tarea. Estos datos dinámicos se recuperan usando las [propiedades de metadatos](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification) generadas por el sistema.
Para incluir valores de los datos del formulario enviados en la notificación por correo electrónico, es necesario crear una propiedad de metadatos personalizada y, a continuación, utilizar estas propiedades de metadatos personalizadas en la plantilla de correo electrónico



## Creación de una propiedad de metadatos personalizada

El método recomendado es crear un componente OSGI que implemente el método getUserMetadata de [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

El siguiente código crea 4 propiedades de metadatos (_firstName_,_lastName_,_reason_ y _amountRequested_) y establece su valor a partir de los datos enviados. Por ejemplo, el valor de la propiedad de metadatos _firstName_ se establece en el valor del elemento llamado firstName de los datos enviados. El siguiente código supone que los datos enviados del formulario adaptable están en formato xml. Los formularios adaptables basados en el esquema JSON o en el modelo de datos de formulario generan datos en formato JSON.


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

## Utilizar las propiedades de metadatos personalizadas en la plantilla de correo electrónico de notificación de tareas

En la plantilla de correo electrónico, puede incluir la propiedad metadata utilizando la siguiente sintaxis, donde amountRequested es la propiedad de metadatos `${amountRequested}`

## Configurar Asignar tarea para que utilice la propiedad de metadatos personalizada

Una vez que el componente OSGi se haya creado e implementado en el servidor AEM, configure el componente Asignar tarea como se muestra a continuación para utilizar propiedades de metadatos personalizadas.


![Notificación de tarea](assets/task-notification.PNG)

## Habilitar el uso de propiedades de metadatos personalizadas

![Propiedades de metadatos personalizadas](assets/custom-meta-data-properties.PNG)

## Para probar esto en su servidor

* [Configurar el servicio Day CQ Mail](https://docs.adobe.com/content/help/en/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* Asociar un id. de correo electrónico válido con [admin user](http://localhost:4502/security/users.html)
* Descargue e instale la [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) utilizando [package manager](http://localhost:4502/crx/packmgr/index.jsp)
* Descargue [Formulario adaptable](assets/request-travel-authorization.zip) e impórtelo en AEM desde la interfaz de usuario de [forms and documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Implementar e iniciar el [Paquete personalizado](assets/work-items-user-service-bundle.jar) mediante la [consola web](http://localhost:4502/system/console/bundles)
* [Vista previa y envío del formulario](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

En el envío del formulario, la notificación de asignación de tareas se envía al id de correo electrónico asociado al usuario administrador. La siguiente captura de pantalla muestra la notificación de asignación de tareas de ejemplo

![Notificación](assets/task-nitification-email.png)

>[!NOTE]
>La plantilla de correo electrónico para la notificación de asignación de tarea debe tener el formato siguiente.
>
> subject=Task Assigned - `${workitem_title}`
>
> message=String que representa la plantilla de correo electrónico sin caracteres de línea nuevos.

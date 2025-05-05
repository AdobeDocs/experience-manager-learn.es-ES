---
title: Implementación del paso de proceso personalizado con cuadro de diálogo
description: Escribir archivos adjuntos de formularios adaptables en el sistema de archivos mediante el paso de proceso personalizado
feature: Workflow
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
duration: 135
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Etapa de proceso personalizado

Este tutorial está diseñado para los clientes de AEM Forms que necesiten implementar un componente de flujo de trabajo personalizado. El primer paso para crear un componente de flujo de trabajo es escribir el código java que se asociará al componente de flujo de trabajo. Para el propósito de este tutorial, vamos a escribir una clase java simple para almacenar los archivos adjuntos de los formularios adaptables en el sistema de archivos. Este código java leerá los argumentos especificados en el componente de flujo de trabajo.

Se requieren los siguientes pasos para escribir la clase java e implementarla como un paquete OSGi

## Crear proyecto de Maven

El primer paso es crear un proyecto de Maven utilizando el arquetipo de Maven de Adobe adecuado. Los pasos detallados se enumeran en este [artículo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=es). Una vez que tenga el proyecto de Maven importado en Eclipse, estará listo para empezar a escribir el primer componente OSGi que se pueda utilizar en el paso del proceso.


### Crear clase que implemente WorkflowProcess

Abra el proyecto de Maven en el IDE de Eclipse. Expandir la carpeta **projectname** > **core**. Expanda la carpeta src/main/java. Debería ver un paquete que termina con &quot;core&quot;. Cree una clase Java que implemente WorkflowProcess en este paquete. Deberá anular el método de ejecución. La firma del método execute es la siguiente
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException

En este tutorial, vamos a escribir los archivos adjuntos agregados al formulario adaptable en el sistema de archivos como parte del flujo de trabajo de AEM.

Para aplicar este caso de uso, se escribió la siguiente clase java

Echemos un vistazo a este código

```java
package com.mysite.core;
import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import javax.jcr.Node;
import javax.jcr.Session;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Custom component to wrtie form attachments to file system",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Custom component to wrtie form attachments to file system"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

  private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
  @Reference
  QueryBuilder queryBuilder;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap)
  throws WorkflowException {

    String attachmentsPath = metaDataMap.get("attachmentsPath", String.class);

    log.debug("Got attachments path: " + attachmentsPath);
    String saveToLocation = metaDataMap.get("SaveToLocation", String.class);
    log.debug("Got save location: " + saveToLocation);

    log.debug("The seperator is" + File.separator);
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Map < String, String > map = new HashMap < String, String > ();
    map.put("path", payloadPath + "/" + attachmentsPath);
    File saveLocationFolder = new File(saveToLocation);
    if (!saveLocationFolder.exists()) {
      saveLocationFolder.mkdirs();
    }

    map.put("type", "nt:file");
    Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
    query.setStart(0);
    query.setHitsPerPage(20);

    SearchResult result = query.getResult();
    log.debug("Got  " + result.getHits().size() + " attachments ");
    Node attachmentNode = null;
    for (Hit hit: result.getHits()) {
      try {
        String path = hit.getPath();
        log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
        attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
        InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
        Document attachmentDoc = new Document(documentStream);
        attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
        attachmentDoc.close();
      } catch (Exception e) {
        log.error("Error saving file " + e.getMessage());
      }
    }
  }
}
```


* attachmentsPath: es la misma ubicación que especificó en el formulario adaptable cuando configuró la acción de envío del formulario adaptable para invocar el flujo de trabajo de AEM. Este es un nombre de la carpeta en la que desea que se guarden los archivos adjuntos en AEM en relación con la carga útil del flujo de trabajo.

* saveToLocation: es la ubicación en la que desea guardar los archivos adjuntos en el sistema de archivos del servidor de AEM.

Estos dos valores se pasan como argumentos de proceso mediante el cuadro de diálogo del componente de flujo de trabajo

![Etapa de proceso](assets/custom-workflow-component.png)

El servicio QueryBuilder se utiliza para consultar nodos de tipo nt:file en la carpeta attachmentsPath. El resto del código se repite en los resultados de búsqueda para crear el objeto Document y guardarlo en el sistema de archivos


>[!NOTE]
>
>Dado que estamos utilizando un objeto de documento específico de AEM Forms, es necesario incluir la dependencia aemfd-client-sdk en su proyecto de Maven.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### Creación e implementación

[Genere el paquete como se describe aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=es)
[Asegúrese de que el paquete esté implementado y en estado activo](http://localhost:4502/system/console/bundles)

## Siguientes pasos

Cree su [componente de flujo de trabajo personalizado](./custom-workflow-component.md)


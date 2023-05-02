---
title: Implementación del paso de proceso personalizado con el cuadro de diálogo
description: Escritura de archivos adjuntos de formularios adaptables en el sistema de archivos mediante el paso de proceso personalizado
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-06-09T00:00:00Z
exl-id: 149d2c8c-bf44-4318-bba8-bec7e25da01b
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# Paso de proceso personalizado

Este tutorial está diseñado para los clientes de AEM Forms que necesitan implementar un componente de flujo de trabajo personalizado. El primer paso para crear un componente de flujo de trabajo es escribir el código Java que se asociará con el componente de flujo de trabajo. Para este tutorial, vamos a escribir una clase java simple para almacenar los archivos adjuntos de los formularios adaptables en el sistema de archivos. Este código java leerá los argumentos especificados en el componente de flujo de trabajo.

Se requieren los siguientes pasos para escribir la clase java e implementar la clase como un paquete OSGi

## Crear proyecto de Maven

El primer paso es crear un proyecto maven utilizando el tipo de archivo Maven de Adobe apropiado. Los pasos detallados se enumeran en esta [article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Una vez que haya importado el proyecto maven en eclipse, estará listo para empezar a escribir su primer componente OSGi que se pueda utilizar en el paso del proceso.


### Crear clase que implemente WorkflowProcess

Abra el proyecto maven en su IDE de eclipse. Expandir **projectname** > **core** carpeta. Expanda la carpeta src/main/java. Debería ver un paquete que termina con &quot;core&quot;. Cree una clase Java que implemente WorkflowProcess en este paquete. Deberá anular el método execute . La firma del método execute es la siguiente: public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)lanza WorkflowException

En este tutorial, se escriben los archivos adjuntos añadidos al formulario adaptable al sistema de archivos como parte del flujo de trabajo AEM.

Para lograr este caso de uso, se escribió la siguiente clase java

Veamos este código

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


* attachmentPath : es la misma ubicación que especificó en el formulario adaptable cuando configuró la acción de envío del formulario adaptable para invocar AEM flujo de trabajo. Es el nombre de la carpeta en la que desea guardar los archivos adjuntos en AEM en relación con la carga útil del flujo de trabajo.

* saveToLocation : es la ubicación en la que desea guardar los archivos adjuntos en el sistema de archivos del servidor de AEM.

Estos dos valores se pasan como argumentos de proceso mediante el cuadro de diálogo del componente de flujo de trabajo

![ProcessStep](assets/custom-workflow-component.png)

El servicio QueryBuilder se utiliza para consultar nodos de tipo nt:file en la carpeta attachmentPath. El resto del código se repite a través de los resultados de búsqueda para crear el objeto Document y guardarlo en el sistema de archivos


>[!NOTE]
>
>Como se usa un objeto Document específico de AEM Forms, es necesario incluir la dependencia aemfd-client-sdk en el proyecto maven.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.772</version>
</dependency>
```

#### Generar e implementar

[Cree el paquete como se describe aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[Asegúrese de que el paquete esté implementado y en estado activo](http://localhost:4502/system/console/bundles)

## Pasos siguientes

Cree su [componente de flujo de trabajo personalizado](./custom-workflow-component.md)


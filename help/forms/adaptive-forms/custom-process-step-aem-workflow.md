---
title: Implementación de pasos de proceso personalizados
description: Escribir archivos adjuntos de formularios adaptables en el sistema de archivos mediante un paso de proceso personalizado
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 226
source-git-commit: 7ebc33932153cf024e68fc5932b7972d9da262a7
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Etapa de proceso personalizado

Este tutorial está diseñado para los clientes de AEM Forms que necesitan implementar un paso de proceso personalizado. Un paso del proceso puede ejecutar un script ECMA o llamar al código Java™ personalizado para realizar operaciones. Este tutorial explicará los pasos necesarios para implementar WorkflowProcess que ejecuta el paso del proceso.

AEM El motivo principal para implementar un paso de proceso personalizado es ampliar el flujo de trabajo de la. Por ejemplo, si utiliza componentes de AEM Forms en el modelo de flujo de trabajo, puede que desee realizar las siguientes operaciones

* Guardar los archivos adjuntos de los formularios adaptables en el sistema de archivos
* Manipulación de los datos enviados

Para aplicar el caso de uso anterior, normalmente escribirá un servicio OSGi que se ejecuta en el paso del proceso.

## Crear proyecto de Maven

El primer paso es crear un proyecto de Maven utilizando el Arquetipo de Maven de Adobe adecuado. Los pasos detallados se enumeran en esta sección [artículo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Una vez que haya importado el proyecto Maven en Eclipse, estará listo para empezar a escribir el primer componente OSGi que se pueda utilizar en el paso del proceso.


### Crear clase que implemente WorkflowProcess

Abra el proyecto Maven en su Eclipse IDE. Expandir **projectname** > **núcleo** carpeta. Expanda el `src/main/java` carpeta. Debería ver un paquete que termina con `core`. Cree una clase Java™ que implemente WorkflowProcess en este paquete. Deberá anular el método execute. La firma del método execute es la siguiente:

```java
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException 
```

El método execute proporciona acceso a las siguientes 3 variables:

**WorkItem**: la variable workItem proporciona acceso a los datos relacionados con el flujo de trabajo. La documentación de la API pública está disponible [aquí.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: esta variable workflowSession le permite controlar el flujo de trabajo. La documentación de la API pública está disponible [aquí](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html).

**MetaDataMap**: todos los metadatos asociados al flujo de trabajo. Todos los argumentos de proceso que se pasan al paso de proceso están disponibles mediante el objeto MetaDataMap.[Documentación de API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

AEM En este tutorial, vamos a escribir los archivos adjuntos agregados al formulario adaptable en el sistema de archivos como parte del flujo de trabajo de la.

Para aplicar este caso de uso, se escribió la siguiente clase Java™

Vamos a ver este código

```java
package com.learningaemforms.adobe.core;

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
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
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
                log.debug("Error saving file " + e.getMessage());
            }
```

Línea 1: define las propiedades del componente. El `process.label` Esta propiedad es lo que verá al asociar el componente OSGi con el paso del proceso, como se muestra en una de las capturas de pantalla a continuación.

Líneas 13-15: los argumentos de proceso pasados a este componente OSGi se dividen mediante el separador &quot;,&quot;. A continuación, los valores de attachmentPath y saveToLocation se extraen de la matriz de cadenas.

* AEM attachmentPath: Es la misma ubicación que especificó en el formulario adaptable cuando configuró la acción de envío del formulario adaptable para invocar el flujo de trabajo de. AEM Este es un nombre de la carpeta en la que desea que se guarden los archivos adjuntos en relación con la carga útil del flujo de trabajo.

* AEM saveToLocation: es la ubicación en la que desea que se guarden los archivos adjuntos en el sistema de archivos del servidor de la.

Estos dos valores se pasan como argumentos de proceso como se muestra en la captura de pantalla siguiente.

![ProcessStep](assets/implement-process-step.gif)

El servicio QueryBuilder se utiliza para consultar nodos de tipo `nt:file` en la carpeta attachmentsPath. El resto del código se repite en los resultados de búsqueda para crear el objeto Document y guardarlo en el sistema de archivos.


>[!NOTE]
>
>Dado que estamos utilizando un objeto de documento específico de AEM Forms, es necesario incluir la dependencia aemfd-client-sdk en el proyecto de Maven. El ID de grupo es `com.adobe.aemfd` y el id de artefactos es `aemfd-client-sdk`.

#### Creación e implementación

[Genere el paquete como se describe aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[Asegúrese de que el paquete esté implementado y en estado activo](http://localhost:4502/system/console/bundles)

Cree un modelo del flujo de trabajo. Arrastre y suelte el paso del proceso en el modelo de flujo de trabajo. Asocie el paso del proceso con Guardar archivos adjuntos de formularios adaptables en el sistema de archivos.

Proporcione los argumentos de proceso necesarios separados por una coma. Por ejemplo, Attachments,c:\\scrappp\\. El primer argumento es la carpeta, cuando los archivos adjuntos del formulario adaptable se van a almacenar en relación con la carga útil del flujo de trabajo. Debe ser el mismo valor que especificó al configurar la acción de envío del formulario adaptable. El segundo argumento es la ubicación en la que desea almacenar los archivos adjuntos.

Crear un formulario adaptable. Arrastre y suelte el componente Archivos adjuntos en el formulario. Configure la acción de envío del formulario para invocar el flujo de trabajo creado en los pasos anteriores. Proporcione la ruta de archivos adjuntos adecuada.

Guarde la configuración.

Previsualice el formulario. Agregue un par de archivos adjuntos y envíe el formulario. Los archivos adjuntos deben guardarse en el sistema de archivos en la ubicación especificada por usted en el flujo de trabajo.

---
title: Paso de proceso personalizado para comprimir archivos adjuntos
description: Paso de proceso personalizado para agregar archivos adjuntos de formularios adaptables a un archivo zip y almacenar el archivo zip en una variable de flujo de trabajo
sub-product: formularios
feature: Flujo de trabajo
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
topic: Desarrollo
role: Developer
level: Beginner
source-git-commit: 22437e93cbf8f36d723dc573fa327562cb51b562
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---


# Paso de proceso personalizado


Se ha implementado un paso de proceso personalizado para crear el archivo zip que contiene los archivos adjuntos del formulario. SI no está familiarizado con la creación de un paquete OSGi, [siga estas instrucciones](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=en)

El código del paso de proceso personalizado hace lo siguiente

* Consulte todos los archivos adjuntos de formularios adaptables de la carpeta de carga útil. El nombre de la carpeta se pasa como argumento de proceso al paso de proceso.
* Cree un archivo zip y agregue los archivos adjuntos del formulario al archivo zip.
* Establezca el valor de 2 variables de flujo de trabajo (attachment_zip y no_of_attachment)

```java
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

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
import com.day.cq.search.result.SearchResult;;

@Component(property = {
        Constants.SERVICE_DESCRIPTION + "=Zip form attachments",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Zip form attachments"
})

public class CreateFormAttachmentCopy implements WorkflowProcess {
        private static final Logger log = LoggerFactory.getLogger(CreateFormAttachmentCopy.class);
        @Reference
        QueryBuilder queryBuilder;

        @Override
        public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException
        {
                String payloadPath = workItem.getWorkflowData().getPayload().toString();
                log.debug("The payload path  is" + payloadPath);
                MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
                Session session = workflowSession.adaptTo(Session.class);
                Map < String, String > map = new HashMap < String, String > ();
                map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
                map.put("type", "nt:file");
                Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
                query.setStart(0);
                query.setHitsPerPage(20);
                SearchResult result = query.getResult();
                log.debug("Get result hits " + result.getHits().size());
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                ZipOutputStream zipOut = new ZipOutputStream(baos);
                int no_of_attachments = result.getHits().size();
                for (Hit hit: result.getHits())
                {
                        try
                        {
                                String attachmentPath = hit.getPath();
                                log.debug("The hit path is" + hit.getPath());
                                Node attachmentNode = session.getNode(attachmentPath + "/jcr:content");
                                InputStream attachmentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                                ByteArrayOutputStream buffer = new ByteArrayOutputStream();
                                int nRead;
                                byte[] data = new byte[1024];
                                while ((nRead = attachmentStream.read(data, 0, data.length)) != -1)
                                {
                                        buffer.write(data, 0, nRead);
                                }

                                buffer.flush();
                                byte[] byteArray = buffer.toByteArray();
                                ZipEntry zipEntry = new ZipEntry(hit.getTitle());
                                zipOut.putNextEntry(zipEntry);
                                zipOut.write(byteArray);
                                zipOut.closeEntry();

                        } 
                        catch (Exception e)
                        {
                                log.debug("The error message is " + e.getMessage());
                        }
                }
                try
                {
                        zipOut.close();

                }
                catch (IOException e)
                {
                        
                        log.debug(("Error in closing zipout" + e.getMessage()));
                }

                // set the value of the workflow variables.
                metaDataMap.put("attachments_zip", new Document(baos.toByteArray()));
                metaDataMap.put("no_of_attachments", no_of_attachments);

                workflowSession.updateWorkflowData(workItem.getWorkflow(), workItem.getWorkflow().getWorkflowData());
                log.debug("Updated workflow");

        }

}
```

>[!NOTE]
>
> Asegúrese de que tiene una variable denominada *attachment_zip* de tipo documento y *no_of_attachment* de tipo Double en el flujo de trabajo para que este código funcione



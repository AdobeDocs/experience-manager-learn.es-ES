---
title: 'Déclencheur del flujo de trabajo de AEM en el envío de formularios HTML5: administrar el envío de formularios'
description: Obtenga información sobre cómo almacenar en déclencheur el flujo de trabajo de AEM cuando se envía el formulario HTML5 y se almacenan los datos enviados en el repositorio.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
badgeVersions: label="AEM Forms 6.5" before-title="false"
level: Experienced
jira: kt-16215
exl-id: e0bde892-1da0-4b72-a408-ad7b84086939
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 1%

---

# Almacenar datos enviados

El siguiente paso es almacenar los datos enviados en el repositorio del autor de AEM. El servlet montado en `/bin/startworkflow` guarda los datos enviados.
Se configura un iniciador de flujo de trabajo de AEM para que se almacene en déclencheur cada vez que se cree un nuevo recurso de tipo `nt:file` en el nodo &lt;node_to_store_submit_data>. Este flujo de trabajo creará un PDF estático o no interactivo combinando los datos enviados con la plantilla xdp. A continuación, el PDF generado se asigna a un usuario para su revisión y aprobación.

Para almacenar los datos enviados en el nodo &lt;node_to_store_submitted_data>, utilizamos el servicio OSGi `GetResolver`, que nos permite guardar los datos enviados mediante el usuario del sistema `fd-service`, que está disponible en cada instalación de AEM Forms.

El nodo en el cual se almacenan los datos enviados se puede configurar usando el Configuration Manager como se explica en [implementar recursos de muestra](./deploy-assets.md)

```java
package com.aemforms.mobileforms.core.servlets;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.UUID;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import com.aemforms.mobileforms.core.configuration.service.AemServerCredentialsConfigurationService;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import org.apache.sling.api.servlets.SlingAllMethodsServlet;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/startworkflow"})
public class StartWorkflow extends SlingAllMethodsServlet {

    private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

    @Reference
    private GetResolver getResolver;
    
    @Reference
    private AemServerCredentialsConfigurationService aemServerCredentialsConfigurationService;
    
    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
    {
            String xmlData = null;
            logger.debug("in start workflow");
            response.setContentType("text/html;charset=UTF-8");
            if (request.getParameter("xmlData") != null)
                {
                    logger.debug("The form was submitted from Acrobat/Reader");
                    xmlData = request.getParameter("xmlData");
                    logger.debug("In start workflow" + xmlData);
                }
                logger.debug("Trying to get resource  "+aemServerCredentialsConfigurationService.getFolderPath());
                Resource r = getResolver.getFormsServiceResolver().getResource(aemServerCredentialsConfigurationService.getFolderPath());
                String responseMessage =null;
                if(r!= null)
                {

                    Session session = r.getResourceResolver().adaptTo(Session.class);
                    logger.debug("Got reosurce pdfsubmissions" + r.getPath());
                    UUID uidName = UUID.randomUUID();
                    Node xmlDataFilesNode = r.adaptTo(Node.class);
                    InputStream is = new ByteArrayInputStream(xmlData.getBytes());
                    Binary binary;
                    try {
                            Node xmlFileNode = xmlDataFilesNode.addNode(uidName.toString(), "nt:file");
                            logger.debug("Added nt file node");
                            Node jcrContent = xmlFileNode.addNode("jcr:content", "nt:resource");
                            logger.debug("Added jcr content");
                            binary = session.getValueFactory().createBinary(is);
                            jcrContent.setProperty("jcr:data", binary);
                            session.save();
                        } catch (RepositoryException e)
                        {
                            throw new RuntimeException(e);
                        }
                    responseMessage = "Your form was successfully submitted";
                }else
                {
                    logger.debug("The resource IS NULL");
                    responseMessage = "Error is processing your submission!!! Please contact the administrator";
                }

        try {
            response.setContentType("text/plain");
            response.getWriter().write(responseMessage);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

}
```

## Siguientes pasos

[Iniciador y flujo de trabajo de flujo](./review-workflow.md)

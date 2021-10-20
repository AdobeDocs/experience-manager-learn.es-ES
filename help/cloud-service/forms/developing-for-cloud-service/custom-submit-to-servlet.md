---
title: Creación de un controlador de acciones de envío personalizado
description: Envío de un formulario adaptable a un controlador de envío personalizado
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8852
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Crear servlet para procesar los datos enviados

Inicie su proyecto de aem-banking en IntelliJ.
Cree un servlet simple para generar los datos enviados en el archivo de registro. Asegúrese de que el código esté en el proyecto principal, como se muestra en la captura de pantalla siguiente
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## Crear envío personalizado

Cree su envío personalizado en la carpeta de la aplicación de la aplicación de la misma manera que lo haría en la [versiones anteriores de AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
El siguiente código en post.POST.jsp simplemente reenvía la solicitud al servlet montado en /bin/formstutorial. Este es el mismo servlet que se creó en el paso anterior

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## Configurar formulario adaptable

Ahora puede configurar el formulario adaptable para que se envíe a este controlador de envío personalizado llamado **Enviar A AEM Servlet**




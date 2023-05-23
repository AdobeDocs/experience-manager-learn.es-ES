---
title: Crear un controlador de acciones de envío personalizado
description: Enviar formularios adaptables a un controlador de envío personalizado
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8852
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Crear servlet para procesar los datos enviados

Inicie el proyecto de banca aem en IntelliJ.
Cree un servlet simple para generar los datos enviados en el archivo de registro. Asegúrese de que el código esté en el proyecto principal como se muestra en la captura de pantalla siguiente
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

Cree su envío personalizado en la carpeta de la aplicación/aplicación bancaria del mismo modo que lo haría en la carpeta [versiones anteriores de AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
El siguiente código de post.POST.jsp simplemente reenvía la solicitud al servlet montado en /bin/formstutorial. Es el mismo servlet que se creó en el paso anterior

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## Configurar formulario adaptable

Ahora puede configurar el formulario adaptable para que se envíe a este controlador de envío personalizado llamado **AEM Enviar A Servlet De**




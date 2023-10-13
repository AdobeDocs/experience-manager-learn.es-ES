---
title: Crear un controlador de acciones de envío personalizado
description: Enviar formularios adaptables a un controlador de envío personalizado
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
kt: 8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 2%

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

## Crear controlador de envío personalizado

Cree la acción de envío personalizada en `apps/bankingapplication` del mismo modo que se crearía en la carpeta [versiones anteriores de AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en). Para los fines de este tutorial, creo una carpeta denominada SubmitToAEMervlet en el `apps/bankingapplication` en el repositorio CRX.

El siguiente código de post.POST.jsp simplemente reenvía la solicitud al servlet montado en /bin/formstutorial. Es el mismo servlet que se creó en el paso anterior

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

AEM En el proyecto de la en IntelliJ, haga clic con el botón secundario en `apps/bankingapplication` y seleccione Nuevo | Empaquete y escriba SubmitToAEMervlet después de la aplicación apps.banking en el cuadro de diálogo nuevo paquete. Haga clic con el botón derecho en el nodo SubmitToAEMervlet y seleccione repositorio AEM AEM | Get Command para sincronizar el proyecto de la con el repositorio del servidor de la aplicación.


## Configurar formulario adaptable

Ahora puede configurar cualquier formulario adaptable para enviarlo a este controlador de envío personalizado llamado **AEM Enviar A Servlet De**

## Pasos siguientes

[Habilitar componentes del portal de Forms](./forms-portal-components.md)

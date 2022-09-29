---
title: Creación del primer servlet en AEM Forms
description: Cree el primer servlet de sling para combinar los datos con la plantilla de formulario.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Sling Servlet

Un Servlet es una clase que se utiliza para ampliar las capacidades de los servidores que hospedan aplicaciones a las que se accede mediante un modelo de programación de respuesta a solicitudes. Para estas aplicaciones, la tecnología Servlet define clases de servlet específicas de HTTP.
Todos los servlets deben implementar la interfaz de Servlet, que define los métodos del ciclo de vida.


Un servlet en AEM puede registrarse como servicio OSGi: puede ampliar SlingSafeMethodsServlet para la implementación de solo lectura o SlingAllMethodsServlet para implementar todas las operaciones RESTful.

## Código Servlet

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## Generar e implementar

Para crear el proyecto, siga los siguientes pasos:

* Apertura **ventana del símbolo del sistema**
* Vaya a `c:\aemformsbundles\mysite\core`
* Ejecutar el comando `mvn clean install -PautoInstallBundle`
* El comando anterior crea e implementa automáticamente el paquete en la instancia de AEM que se ejecuta en localhost:4502

El paquete también está disponible en la siguiente ubicación `C:\AEMFormsBundles\mysite\core\target`. El paquete también se puede implementar en AEM usando la variable [Consola web Felix.](http://localhost:4502/system/console/bundles)


## Probar el Servlet Resolver

Apunte el navegador a [URL de resolución de servlet](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Esto le indica el servlet que se invoca para una ruta determinada, como se ve en la captura de pantalla siguiente
![servlet-resolver](assets/servlet-resolver.JPG)

## Probar el servlet con Postman

![Probar el servlet con Postman](assets/test-servlet-postman.JPG)

---
title: Creación del primer servlet en AEM Forms
description: Cree su primer servlet sling para combinar datos con la plantilla de formulario.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
last-substantial-update: 2021-04-23T00:00:00Z
duration: 55
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# Sling Servlet

Un servlet es una clase que se utiliza para ampliar las capacidades de los servidores a los que se accede a las aplicaciones host mediante un modelo de programación de solicitud-respuesta. Para estas aplicaciones, la tecnología Servlet define clases de servlet específicas de HTTP.
Todos los servlets deben implementar la interfaz Servlet, que define los métodos de ciclo de vida.


Un servlet en AEM se puede registrar como servicio OSGi: puede ampliar SlingSafeMethodsServlet para la implementación de solo lectura o SlingAllMethodsServlet para implementar todas las operaciones RESTful.

## Código servlet

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import java.io.File;
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

## Creación e implementación

Para crear su proyecto, siga los siguientes pasos:

* Abrir **ventana del símbolo del sistema**
* Navegue hasta `c:\aemformsbundles\mysite\core`
* Ejecutar el comando `mvn clean install -PautoInstallBundle`
* El comando anterior crea e implementa automáticamente el paquete en la instancia de AEM que se ejecuta en localhost:4502

El paquete también está disponible en la siguiente ubicación `C:\AEMFormsBundles\mysite\core\target`. El paquete también se puede implementar en AEM mediante la consola web [Felix.](http://localhost:4502/system/console/bundles)


## Prueba de la resolución de servlet

Dirija su navegador a la [URL de resolución de servlets](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). Esto le indica el servlet que se invoca para una ruta determinada, como se ve en la captura de pantalla siguiente
![servlet-resolver](assets/servlet-resolver.JPG)

## Prueba del servlet con Postman

![Probar el servlet mediante Postman](assets/test-servlet-postman.JPG)

## Siguientes pasos

[Incluir JAR de terceros](./include-third-party-jars.md)


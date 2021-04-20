---
title: Activador del flujo de trabajo de AEM en el envío de formularios HTML5
seo-title: Activar el flujo de trabajo de AEM en el envío de formularios HTML5
description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
seo-description: Siga rellenando el formulario móvil en modo sin conexión y envíe el formulario móvil para activar el flujo de trabajo de AEM
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 1%

---


# Crear perfil personalizado

En esta parte crearemos un [perfil personalizado.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Un perfil es responsable de procesar el XDP como HTML. Se proporciona un perfil predeterminado listo para usar para procesar XDP como HTML. Representa una versión personalizada del servicio de representación de formularios móviles. Puede utilizar el servicio de representación de formularios móviles para personalizar el aspecto, el comportamiento y las interacciones de los formularios móviles. En nuestro perfil personalizado, capturaremos los datos rellenados en el formulario móvil mediante la API de guía. A continuación, estos datos se envían al servlet personalizado que genera un PDF interactivo y lo reenvía a la aplicación que realiza la llamada.

Obtenga los datos del formulario mediante la API de JavaScript `formBridge`. Utilizamos el método `getDataXML()`:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

En el método del controlador de éxito hacemos una llamada al servlet personalizado que se ejecuta en AEM. Este servlet procesará y devolverá un pdf interactivo con los datos del formulario móvil

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Generar PDF interactivo

El siguiente es el código servlet que es responsable de procesar el pdf interactivo y devolver el pdf a la aplicación que realiza la llamada. El servlet invoca el método `mobileFormToInteractivePdf` del servicio personalizado DocumentServices OSGi.

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
          response.setContentType("application/pdf");
          response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
          response.setContentLength((int) fileInputStream.available());
          OutputStream responseOutputStream = response.getOutputStream();
          int bytes;
          while ((bytes = fileInputStream.read()) != -1) {
              responseOutputStream.write(bytes);
          }
          responseOutputStream.flush();
          responseOutputStream.close();
      } catch (IOException e) {
        // TODO Add proper error logging
      }
    }
}
```

### Procesar PDF interactivo

El siguiente código utiliza la [API del servicio de formularios](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) para procesar un PDF interactivo con los datos del formulario móvil.

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

Para ver la capacidad de descargar PDF interactivo desde un formulario móvil parcialmente completado, [haga clic aquí](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content).
Una vez descargado el PDF, el siguiente paso es enviar el PDF para activar un flujo de trabajo de AEM. Este flujo de trabajo combina los datos del PDF enviado y genera un PDF no interactivo para su revisión.

El perfil personalizado creado para este caso de uso está disponible como parte de estos recursos de tutorial.

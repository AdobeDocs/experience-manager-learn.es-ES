---
title: Activar AEM flujo de trabajo en el envío de formulario HTML5
seo-title: Activar flujo de trabajo AEM en envío de formulario HTML5
description: Seguir rellenando formularios móviles en modo sin conexión y enviar formularios móviles para activar AEM flujo de trabajo
seo-description: Seguir rellenando formularios móviles en modo sin conexión y enviar formularios móviles para activar AEM flujo de trabajo
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---


# Crear Perfil personalizado

En esta parte crearemos un perfil [personalizado.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Un perfil es responsable de procesar el XDP como HTML. Se proporciona un perfil predeterminado para procesar XDP como HTML. Representa una versión personalizada del servicio de representación de Forms móvil. Puede utilizar el servicio de representación de formularios móviles para personalizar el aspecto, el comportamiento y las interacciones del Forms móvil. En nuestro perfil personalizado capturaremos los datos rellenados en el formulario móvil mediante la API guidebridge. A continuación, estos datos se envían al servlet personalizado para generar un PDF interactivo y retransmitirlo a la aplicación que realiza la llamada.

Obtenga los datos del formulario mediante la API de `formBridge` JavaScript. Utilizamos el `getDataXML()` método:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

En el método de controlador de éxito realizamos una llamada al servlet personalizado que se ejecuta en AEM. Este servlet procesará y devolverá PDF interactivos con los datos del formulario móvil

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

## Generar archivo PDF interactivo

El siguiente es el código servlet que se encarga de procesar el PDF interactivo y devolver el PDF a la aplicación que realiza la llamada. El servlet invoca `mobileFormToInteractivePdf` el método del OSGi personalizado de DocumentServices.

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

### Representar PDF interactivo

El siguiente código utiliza la API [de servicio de](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) Forms para procesar un PDF interactivo con los datos del formulario móvil.

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

Para vista la capacidad de descargar PDF interactivos desde un formulario móvil parcialmente completado, [haga clic aquí](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content).
Una vez descargado el PDF, el siguiente paso es enviar el PDF para activar un flujo de trabajo AEM. Este flujo de trabajo combinará los datos del PDF enviado y generará archivos PDF no interactivos para su revisión.

El perfil personalizado creado para este caso de uso está disponible como parte de este tutorial.

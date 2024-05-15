---
title: 'Déclencheur AEM del flujo de trabajo de la en el envío de formularios HTM5: crear perfil personalizado'
description: Continúe rellenando el formulario móvil en el modo sin conexión y envíe el formulario móvil al flujo de trabajo de déclencheur AEM de la
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Crear perfil personalizado

En esta parte crearemos una [perfil personalizado.](https://helpx.adobe.com/es/livecycle/help/mobile-forms/creating-profile.html) Un perfil es responsable de procesar el XDP como HTML. Se proporciona un perfil predeterminado para procesar XDP como HTML. Representa una versión personalizada del servicio de representación de Forms móvil. Puede utilizar el servicio de representación de formularios móviles para personalizar el aspecto, el comportamiento y las interacciones de Mobile Forms. En nuestro perfil personalizado, capturaremos los datos rellenados en el formulario móvil mediante la API de guidebridge. A continuación, estos datos se envían a un servlet personalizado que genera un PDF interactivo y lo transmite de nuevo a la aplicación que realiza la llamada.

Obtener los datos del formulario mediante `formBridge` API de JavaScript. Hacemos uso de la `getDataXML()` método:

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

AEM En el método del controlador de éxito, realizamos una llamada al servlet personalizado que se ejecuta en el modo de ejecución de la. Este servlet procesará y devolverá un pdf interactivo con los datos del formulario móvil

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

El siguiente es el código de servlet responsable de procesar el pdf interactivo y devolver el pdf a la aplicación que realiza la llamada. El servlet invoca `mobileFormToInteractivePdf` del servicio OSGi de Document Services personalizado.

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

El siguiente código utiliza la variable [API del servicio Forms](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) para procesar el PDF interactivo con los datos del formulario móvil.

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

Para ver la capacidad de descargar el PDF interactivo desde un formulario móvil parcialmente completado, [haga clic aquí](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Una vez descargado el PDF, el siguiente paso es enviar al PDF al déclencheur AEM de un flujo de trabajo de la. Este flujo de trabajo combina los datos del PDF enviado y genera un PDF no interactivo para su revisión.

El perfil personalizado creado para este caso de uso está disponible como parte de los recursos de este tutorial.

---
title: Generar DoR interactivo con datos de formulario adaptable
description: Combinación de datos de formulario adaptables con plantilla XDP para generar pdf descargable
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
source-git-commit: 72a9edb3edc73cf14f13bb53355a37e707ed4c79
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---


# Descargar DoR interactivo

Un caso de uso común es poder descargar un DoR interactivo con los datos del Formulario adaptable. El documento de resolución de problemas descargado se completará con Adobe Acrobat o Adobe Reader.

Para lograr este caso de uso necesitamos hacer lo siguiente

## Generar datos de ejemplo para el xdp

Abra el XDP en AEM Forms designer.
Haga clic en Archivo | Propiedades del formulario | Vista previa Haga clic en Generar datos de vista previa Haga clic en Generar Proporcione un nombre de archivo significativo como &quot;form-data.xml&quot;

## Generar XSD a partir de los datos xml

Puede utilizar cualquiera de las herramientas gratuitas en línea para [generar xsd](https://www.freeformatter.com/xsd-generator.html) a partir de los datos xml generados en el paso anterior.

## Crear adaptable

Cree un formulario adaptable basado en el xsd del paso anterior. Asocie el formulario para utilizar el cliente lib &quot;irs&quot;. Esta biblioteca de cliente tiene el código para realizar una llamada de POST al servlet que devuelve el PDF a la aplicación que realiza la llamada. El siguiente código se activa cuando la variable _Descargar PDF_ se hace clic

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## Crear servlet personalizado

Cree un servlet personalizado que combine los datos con la plantilla xdp y devuelva el pdf. El código para lograr esto se enumera a continuación. El servlet personalizado forma parte del [Paquete AEMFormsDocumentServices.core-1.0-SNAPSHOT](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)).

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

En el código de muestra, el nombre de la plantilla (f8918-r14e_redo-barcode_3 2.xdp) está codificado de forma rígida. Puede pasar fácilmente el nombre de la plantilla al servlet para que este código sea genérico y funcione con todas las plantillas.


Para probar esto en el servidor local, siga los siguientes pasos:
1. [Descargar e instalar el paquete DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Añada la siguiente entrada en el servicio MAPA de usuario del servicio Apache Sling DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Descargar e instalar el paquete personalizado de DocumentServices](/hep/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Esto tiene el servlet para combinar los datos con la plantilla XDP y transmitir el pdf hacia atrás
1. [Importar la biblioteca del cliente](assets/irs.zip)
1. [Importar el formulario adaptable](assets/f8918complete.zip)
!. [Importación del esquema y la plantilla XDP](assets/xdp-template-and-xsd.zip)
1. [Vista previa del formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Rellene algunos campos del formulario
1. Haga clic en Descargar PDF para obtener el PDF

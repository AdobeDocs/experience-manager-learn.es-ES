---
title: Mostrar documento de registro en línea
description: Combine datos de formulario adaptables con la plantilla XDP y muestre el PDF en línea mediante la API de PDF incrustado de Document Cloud.
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9411
exl-id: 327ffe26-e88e-49f0-9f5a-63e2a92e1c8a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 171
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 1%

---

# Mostrar DoR en línea

Un caso de uso común es mostrar un documento pdf con los datos introducidos por el usuario que rellena el formulario.

Para llevar a cabo este caso de uso, hemos utilizado el [API de incrustación de Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

Se han realizado los siguientes pasos para completar la integración

## Crear un componente personalizado para mostrar el PDF en línea

Se ha creado un componente personalizado (incrustado-pdf) para incrustar el PDF devuelto por la llamada del POST.

## Biblioteca de cliente

El siguiente código se ejecuta cuando el `viewPDF` al hacer clic en el botón de casilla. Pasamos los datos del formulario adaptable, el nombre de la plantilla, al punto final para generar el pdf. A continuación, el PDF generado se muestra al usuario que rellena el formulario mediante la biblioteca JavaScript de PDF incrustados.

```javascript
$(document).ready(function() {

    $(".viewPDF").click(function() {
        console.log("view pdfclicked");
        window.guideBridge.getDataXML({
            success: function(result) {
                var obj = new FormData();
                obj.append("data", result.data);
                obj.append("template", document.querySelector("[data-template]").getAttribute("data-template"));
                const fetchPromise = fetch(document.querySelector("[data-endpoint]").getAttribute("data-endpoint"), {
                        method: "POST",
                        body: obj,
                        contentType: false,
                        processData: false,

                    })
                    .then(response => {

                        var adobeDCView = new AdobeDC.View({
                            clientId: document.querySelector("[data-apikey]").getAttribute("data-apikey"),
                            divId: "adobe-dc-view"
                        });
                        console.log("In preview file");
                        adobeDCView.previewFile(

                            {
                                content: {
                                    promise: response.arrayBuffer()
                                },
                                metaData: {
                                    fileName: document.querySelector("[data-filename]").getAttribute("data-filename")
                                }
                            }
                        );


                        console.log("done")
                    })


            }
        });
    });



});
```

## Generar datos de ejemplo para el XDP

* Abra el XDP en AEM Forms Designer.
* Haga clic en Archivo | Propiedades del formulario | Previsualizar
* Haga clic en Generar datos de previsualización
* Haga clic en Generar
* Proporcionar un nombre de archivo significativo como &quot;form-data.xml&quot;

## Generar XSD a partir de los datos xml

Puede utilizar cualquiera de las herramientas gratuitas en línea para [generar XSD](https://www.freeformatter.com/xsd-generator.html) a partir de los datos xml generados en el paso anterior.

## Cargar la plantilla

Asegúrese de cargar la plantilla xdp en [AEM Forms](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments) con el botón crear


## Crear formulario adaptable

Cree un formulario adaptable basado en el XSD del paso anterior.
Agregue una nueva pestaña al formulario adaptable. Agregue un componente Casilla de verificación e incrustar PDF a esta pestaña. Asegúrese de asignar un nombre a la casilla de verificación viewPDF.
Configure el componente incrustado-pdf como se muestra en la captura de pantalla siguiente
![embed-pdf](assets/embed-pdf-configuration.png)

**Incrustar clave API de PDF** : Esta es la clave que puede utilizar para incrustar el pdf. Esta clave solo funcionará con localhost. Puede crear [su propia clave](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) y asociarlo a otro dominio.

**El extremo devuelve el PDF** : Este es el servlet personalizado que combinará los datos con la plantilla xdp y devolverá el pdf.

**Nombre de plantilla** - Esta es la ruta al xdp. Normalmente, se almacena en la carpeta formsanddocuments.

**Nombre de archivo del PDF** : esta es la cadena que aparecerá en el componente incrustar pdf.

## Crear servlet personalizado

Se ha creado un servlet personalizado para combinar los datos con la plantilla XDP y devolver el PDF. El código para lograrlo se enumera a continuación. El servlet personalizado forma parte del [incrustar paquete pdf](assets/embedpdf.core-1.0-SNAPSHOT.jar)

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.io.StringWriter;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;

package com.embedpdf.core.servlets;
@Component(service = {
   Servlet.class
}, property = {
   "sling.servlet.methods=post",
   "sling.servlet.paths=/bin/getPDFToEmbed"
})
public class StreamPDFToEmbed extends SlingAllMethodsServlet {
   @Reference
   OutputService outputService;
   private static final long serialVersionUID = 1 L;
   private static final Logger log = LoggerFactory.getLogger(StreamPDFToEmbed.class);

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      String xdpName = request.getParameter("template");
      String formData = request.getParameter("data");
      log.debug("in doPOST of Stream PDF Form Data is >>> " + formData + " template is >>> " + xdpName);

      try {

         XPathFactory xfact = XPathFactory.newInstance();
         XPath xpath = xfact.newXPath();
         DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
         DocumentBuilder builder = factory.newDocumentBuilder();

         org.w3c.dom.Document xmlDataDoc = builder.parse(new InputSource(new StringReader(formData)));

         // get the data to merge with template

         Node afBoundData = (Node) xpath.evaluate("afData/afBoundData", xmlDataDoc, XPathConstants.NODE);
         NodeList afBoundDataChildren = afBoundData.getChildNodes();
         String afDataNodeName = afBoundDataChildren.item(0).getNodeName();
         Node nodeWithDataToMerge = (Node) xpath.evaluate("afData/afBoundData/" + afDataNodeName, xmlDataDoc, XPathConstants.NODE);
         StringWriter writer = new StringWriter();
         Transformer transformer = TransformerFactory.newInstance().newTransformer();
         transformer.transform(new DOMSource(nodeWithDataToMerge), new StreamResult(writer));
         String xml = writer.toString();
         InputStream targetStream = new ByteArrayInputStream(xml.getBytes());
         Document xmlDataDocument = new Document(targetStream);
         // get the template
         Document xdpTemplate = new Document(xdpName);
         log.debug("got the  xdp Template " + xdpTemplate.length());

         // use output service the merge data with template
         com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
         pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
         com.adobe.aemfd.docmanager.Document documentToReturn = outputService.generatePDFOutput(xdpTemplate, xmlDataDocument, pdfOptions);

         // stream pdf to the client

         InputStream fileInputStream = documentToReturn.getInputStream();
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

      } catch (Exception e) {

         System.out.println("Error " + e.getMessage());
      }

   }

}
```


## Implementar el ejemplo en el servidor

Para probar esto en el servidor local, siga los siguientes pasos:

1. [Descargar e instalar el paquete incrustado en PDF](assets/embedpdf.core-1.0-SNAPSHOT.jar).
Tiene el servlet para combinar los datos con la plantilla XDP y transmitir el PDF de vuelta.
1. Añada la ruta /bin/getPDFToEmbed en la sección de rutas excluidas del filtro CSRF de Granite de Adobe utilizando [AEM ConfigMgr de](http://localhost:4502/system/console/configMgr). En el entorno de producción se recomienda utilizar la variable [Marco de protección CSRF](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=en)
1. [Importar la biblioteca de cliente y el componente personalizado](assets/embed-pdf.zip)
1. [Importar el formulario adaptable y la plantilla](assets/embed-pdf-form-and-xdp.zip)
1. [Previsualizar formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/from1040/jcr:content?wcmmode=disabled)
1. Rellene algunos de los campos del formulario
1. Vaya a la pestaña Ver PDF. Seleccione la casilla de verificación Ver PDF. Debería ver un PDF mostrado en el formulario rellenado con los datos del formulario adaptable

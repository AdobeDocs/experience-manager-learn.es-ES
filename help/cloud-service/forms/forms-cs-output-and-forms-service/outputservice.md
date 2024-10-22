---
title: Generación de documentos de PDF mediante el servicio Output
description: Combinar datos con la plantilla XDP para generar el de un PDF no interactivo
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a0de7eaa391749b6b0d90e7cf3e363c2d5a232b5
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 15%

---


# Generación de documentos de PDF mediante el servicio Output

AEM El [servicio Output](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html) es un servicio OSGi que forma parte de Servicios de documentos de. Admite varios formatos de salida y funciones de diseño de AEM Forms Designer. El servicio Output convierte las plantillas XFA y los datos XML para generar documentos de impresión en diferentes formatos.

El servicio Output en AEM Forms as a Cloud Service se parece mucho al de AEM Forms 6.5, por lo que si está familiarizado con el uso del servicio Output en AEM Forms 6.5, la transición a AEM Forms as a Cloud Service debería ser sencilla.

Con el servicio Output, puede crear aplicaciones que le permiten lo siguiente:

+ Generar formularios finales al rellenar archivos de plantilla con datos XML.
+ Generar formularios de salida en varios formatos, incluidos los flujos de impresión no interactivos PDF, PostScript, PCL y ZPL.
+ Generar PDF de impresión a partir de PDF de formularios XFA.
+ Generar documentos de PDF, PostScript, PCL y ZPL por lotes combinando varios conjuntos de datos con plantillas de origen.

Este servicio está diseñado para utilizarse en el contexto de una instancia as a Cloud Service de AEM Forms. El siguiente fragmento de código genera un documento de PDF en un servlet utilizando `OutputService`.

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```

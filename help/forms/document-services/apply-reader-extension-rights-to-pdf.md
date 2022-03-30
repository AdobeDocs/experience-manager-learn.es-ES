---
title: Aplicar derechos de uso a pdf cargado
description: Aplicar derechos de uso a pdf
version: 6.4,6.5
feature: Reader Extensions
topic: Development
role: Developer
level: Experienced
exl-id: ea433667-81db-40f7-870d-b16630128871
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 0%

---

# Aplicación de extensiones de Reader

Extensiones de Reader permite manipular los derechos de uso en documentos de PDF. Los derechos de uso pertenecen a una funcionalidad disponible en Acrobat pero no en Adobe Reader. La funcionalidad controlada por Extensiones de Reader incluye la capacidad de agregar comentarios a un documento, rellenar formularios y guardar el documento. Los documentos del PDF que tienen derechos de uso añadidos se denominan documentos con derechos activados. Un usuario que abre un documento PDF con derechos activados en Adobe Reader puede realizar las operaciones habilitadas para ese documento.

Para lograr este caso de uso necesitamos hacer lo siguiente:
* [Añadir el certificado de extensiones de Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) a `fd-service` usuario.

## Crear un servicio OSGi personalizado

Cree un servicio OSGi personalizado que aplique derechos de uso a los documentos. El código para lograr esto se enumera a continuación

```java
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = ApplyUsageRights.class)
public class ApplyUsageRights implements ReaderExtendPDF {
        @Reference
        DocAssuranceService docAssuranceService;
        @Reference
        GetResolver getResolver;
        Logger logger = LoggerFactory.getLogger(ApplyUsageRights.class);
        @Override
        public Document applyUsageRights(Document pdfDocument, UsageRights usageRights) {

                ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
                UnlockOptions unlockOptions = null;
                ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
                reOptions.setCredentialAlias("ares");

                reOptions.setResourceResolver(getResolver.getFormsServiceResolver());

                reOptions.setReOptions(reOptionsSpec);
                System.out.println("Applying Usage Rights");

                try {
                        Document readerExtended = docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
                                unlockOptions);
                        reOptions.getResourceResolver().close();
                        return readerExtended;
                } catch (Exception e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                return null;
        }

}
```

## Cree un servlet para transmitir el PDF ampliado del lector

El siguiente paso es crear un servlet con un método de POST para devolver al usuario el PDF ampliado del lector. En este caso, se pedirá al usuario que guarde el PDF en su sistema de archivos. Esto se debe a que el PDF se representa como PDF dinámico y los visualizadores de pdf que se incluyen con los exploradores no gestionan los archivos pdf dinámicos.

A continuación se muestra el código del servlet. El servlet se invocará desde la acción customsubmit del formulario adaptable.
Servlet crea el objeto UsageRights y establece sus propiedades en función de los valores introducidos por el usuario en el formulario adaptable. A continuación, el servlet llama al método applyUsageRights del servicio creado para este fin.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Map;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        private static final long serialVersionUID = -883724052368090823 L;
        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];

                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got form attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);
                                        if (logger.isDebugEnabled()) {
                                                documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        }

                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();

                                        response.setContentType("application/pdf");
                                        response.addHeader("Content-Disposition", "attachment; filename=" + param.getFileName().split("/")[1]);
                                        response.setContentLength((int) fileInputStream.available());
                                        OutputStream responseOutputStream = response.getOutputStream();
                                        documentToReaderExtend.close();
                                        int bytes;
                                        while ((bytes = fileInputStream.read()) != -1) {
                                                responseOutputStream.write(bytes);
                                        }
                                        responseOutputStream.flush();
                                        responseOutputStream.close();

                                } catch (IOException e) {
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

                }

        }

}
```

Para probar esto en el servidor local, siga los siguientes pasos:
1. [Descargar e instalar el paquete DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Descargar e instalar el paquete ares.ares.core-ares](assets/ares.ares.core-ares.jar). Esto tiene el servicio personalizado y el servlet para aplicar derechos de uso y retransmitir el pdf
1. [Importar las bibliotecas de cliente y el envío personalizado](assets/applyaresdemo.zip)
1. [Importar el formulario adaptable](assets/applyaresform.zip)
1. Agregue el certificado de Extensiones de Reader al usuario &quot;fd-service&quot;. Asegúrese de que el alias sea &quot;ares&quot;.
1. [Vista previa del formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Seleccione los derechos adecuados y cargue el archivo del PDF
1. Haga clic en Enviar para obtener el PDF ampliado del Reader

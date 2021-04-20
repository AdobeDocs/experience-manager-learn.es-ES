---
title: Renderización de XDP en PDF con derechos de uso
seo-title: Renderización de XDP en PDF con derechos de uso
description: Aplicar derechos de uso a pdf
seo-description: Aplicar derechos de uso a pdf
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 1%

---


# Aplicación de extensiones de lector

Reader Extensions permite manipular los derechos de uso en documentos PDF. Los derechos de uso pertenecen a funciones disponibles en Acrobat pero no en Adobe Reader. La funcionalidad controlada por Reader Extensions incluye la capacidad de agregar comentarios a un documento, rellenar formularios y guardar el documento. Los documentos PDF que tienen derechos de uso añadidos se denominan documentos habilitados para derechos. Un usuario que abre un documento PDF con derechos activados en Adobe Reader puede realizar las operaciones habilitadas para ese documento.
Para probar esta capacidad, puede probar este [enlace](https://forms.enablementadobe.com/content/samples/samples.html?query=0). El nombre de muestra es &quot;Render XDP with RE&quot;

Para lograr este caso de uso necesitamos hacer lo siguiente:
* Agregue el certificado de Reader Extensions al usuario &quot;fd-service&quot;. Los pasos para agregar las credenciales de Reader Extensions se enumeran [aquí](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)

* Cree un servicio OSGi personalizado que aplique derechos de uso a los documentos. El código para lograr esto se enumera a continuación

```java
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service=ApplyUsageRights.class,immediate = true)
public class ApplyUsageRights implements ReaderExtendPDF {
@Reference
DocAssuranceService docAssuranceService;
@Reference
GetResolver getResolver;
@Override
public Document applyUsageRights(Document pdfDocument,UsageRights usageRights) {
      ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
      UnlockOptions unlockOptions = null;
      ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
      reOptions.setCredentialAlias("ares");
      reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
      reOptions.setReOptions(reOptionsSpec);
    try {
          return docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
          unlockOptions);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return null;
}

}
```

## Crear servlet para transmitir el PDF {#create-servlet-to-stream-the-pdf}

El siguiente paso es crear un servlet con un método POST para devolver al usuario el PDF ampliado del lector. En este caso, se pedirá al usuario que guarde el PDF en su sistema de archivos. Esto se debe a que el PDF se representa como PDF dinámico y los visualizadores de pdf que se incluyen con los navegadores no gestionan los pdf dinámicos.

A continuación se muestra el código del servlet. El servlet se invocará desde la acción **customsubmit** del formulario adaptable.
Servlet crea el objeto UsageRights y establece sus propiedades en función de los valores introducidos por el usuario en el formulario adaptable. A continuación, el servlet llama al método **applyUsageRights** del servicio creado para este fin.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Enumeration;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.PathNotFoundException;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFormatException;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = Servlet.class, property = {
sling.servlet.methods=get", "sling.servlet.paths=/bin/applyrights"
})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {
  @Reference
  GetResolver getResolver;
  @Reference
  ApplyUsageRights applyRights;
  public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
  try {
        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
        InputSource is = new InputSource();
        is.setCharacterStream(new StringReader(xmlString));
  return db.parse(is);
} catch (ParserConfigurationException e) {
    e.printStackTrace();
} catch (SAXException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
return null;
}
@Override
protected void doGet(SlingHttpServletRequest request,SlingHttpServletResponse response)
{
  doPost(request,response);
}
@Override
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  UsageRights usageRights = new UsageRights();
  String submittedData = request.getParameter("jcr:data");
  org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
  String formFill = submittedXml.getElementsByTagName("formfill").item(0).getTextContent();
  usageRights.setEnabledFormDataImportExport(true);
  usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
  usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
  usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
  usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
  String attachmentPath = submittedXml.getElementsByTagName("attachmentpath").item(0).getTextContent();
  Node pdfNode = getResolver.getFormsServiceResolver().getResource(attachmentPath).adaptTo(Node.class);
  javax.jcr.Node jcrContent = null;
try {
    jcrContent = pdfNode.getNode("jcr:content");
    } catch (RepositoryException e1) {
      e1.printStackTrace();
    }

try {
    InputStream is = jcrContent.getProperty("jcr:data").getBinary().getStream();
    Session jcrSession = getResolver.getFormsServiceResolver().adaptTo(Session.class);
    Document documentToReaderExtend = new Document(is);
    documentToReaderExtend =  applyRights.applyUsageRights(documentToReaderExtend,usageRights);
    documentToReaderExtend.copyToFile(new File("c:\\scrap\\doctore.pdf"));
    InputStream fileInputStream = documentToReaderExtend.getInputStream();
    documentToReaderExtend.close();
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
  } catch (ValueFormatException e) {
      e.printStackTrace();
  } catch (PathNotFoundException e) {
      e.printStackTrace();
  } catch (RepositoryException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

}

}
```

Para probar esto en el servidor local, siga los siguientes pasos:
1. [Descargar e instalar el paquete DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Descargue e instale el paquete ares.ares.core-ares](assets/ares.ares.core-ares.jar). Esto tiene el servicio personalizado y el servlet para aplicar derechos de uso y retransmitir el pdf
1. [Importar las bibliotecas de cliente y el envío personalizado](assets/applyaresdemo.zip)
1. [Importar el formulario adaptable](assets/applyaresform.zip)
1. Añadir el certificado de Reader Extensions al usuario &quot;fd-service&quot;
1. [Vista previa del formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Seleccione los derechos adecuados y cargue el archivo PDF
1. Haga clic en Enviar para obtener el PDF ampliado de Reader




---
title: Procesar XDP en un PDF con derechos de uso
description: Aplicar derechos de uso a PDF
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
duration: 190
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# Procesar XDP en un PDF con derechos de uso{#rendering-xdp-into-pdf-with-usage-rights}

Un caso de uso común es procesar xdp en PDF y aplicar extensiones de Reader al PDF procesado.

Por ejemplo, en el portal de formularios de AEM Forms, cuando un usuario hace clic en XDP, podemos procesar XDP como PDF y Extenderlo como PDF.


Para llevar a cabo este caso de uso, debemos hacer lo siguiente.

* Agregue el certificado de Extensiones de Reader al usuario &quot;fd-service&quot;. Se enumeran los pasos para agregar la credencial Extensiones de Reader [aquí](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=en)


* También puede consultar el vídeo sobre [configurar las credenciales de Extensiones de Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* Cree un servicio OSGi personalizado que procese y aplique derechos de uso. El código para lograrlo se enumera a continuación

## Procesar XDP y aplicar derechos de uso {#render-xdp-and-apply-usage-rights}

* Línea 7: con el renderPDFForm del servicio Forms, se genera un PDF a partir del XDP.

* Líneas 8-14: se establecen los derechos de uso correspondientes. Estos derechos de uso se recuperan de las opciones de configuración de OSGi.

* Línea 20 : Utilice resourceresolver asociado al usuario de servicio fd-service

* Línea 24: se utiliza el método secureDocument de DocumentAssuranceService para aplicar los derechos de uso

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

La siguiente captura de pantalla muestra las propiedades de configuración expuestas. La mayoría de los derechos de uso comunes se exponen a través de esta configuración.

![Propiedades de configuración](assets/configurationproperties.gif)

El siguiente código muestra el código que se utiliza para generar las opciones de configuración de OSGi

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## Crear servlet para transmitir el PDF {#create-servlet-to-stream-the-pdf}

El siguiente paso es crear un servlet con un método de GET para devolver el PDF de reader Extended al usuario. En este caso, se solicita al usuario que guarde el PDF en su sistema de archivos. Esto se debe a que el PDF se procesa como PDF dinámico y los visualizadores de PDF que vienen con los navegadores no gestionan los PDF dinámicos.

El siguiente es el código del servlet. Pasamos la ruta del XDP en el repositorio CRX a este servlet.

Luego llamamos al método renderAndExtendXdp de com.aemformssamples.documentservices.core.DocumentServices.

A continuación, el PDF extendido de reader se transmite a la aplicación que realiza la llamada

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

Para probar esto en el servidor local, siga los siguientes pasos
1. [Descargar e instalar el paquete DevelopersWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Descargar e instalar el paquete AEM FormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [Descargar el HTML de plantilla de portal personalizada](assets/render-and-extend-template.zip)
1. [AEM Descargue e importe los recursos relacionados con este artículo en el administrador de paquetes de, usando el administrador de paquetes de](assets/renderandextendxdp.zip)
   * Este paquete tiene un portal de muestra y un archivo xdp
1. Añadir certificado de Extensiones de Reader al usuario &quot;fd-service&quot;
1. Dirija el explorador a [página web del portal](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. Haga clic en el icono pdf para procesar el xdp como archivo pdf con derechos de uso aplicados.

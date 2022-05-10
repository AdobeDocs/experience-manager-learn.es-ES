---
title: Etiquetado y almacenamiento de AEM Forms DoR en DAM
description: Este artículo tratará el caso de uso de almacenar y etiquetar el DoR generado por AEM Forms en AEM DAM. El etiquetado del documento se realiza en función de los datos de formulario enviados.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 0%

---

# Etiquetado y almacenamiento de AEM Forms DoR en DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Este artículo tratará el caso de uso de almacenar y etiquetar el DoR generado por AEM Forms en AEM DAM. El etiquetado del documento se realiza en función de los datos de formulario enviados.

Una solicitud habitual de los clientes es almacenar y etiquetar el documento de registro (DoR) generado por AEM Forms en AEM DAM. El etiquetado del documento debe basarse en los datos enviados por el Forms adaptable. Por ejemplo, si el estado de empleo en los datos enviados es &quot;Retirado&quot;, queremos etiquetar el documento con la etiqueta &quot;Retirado&quot; y almacenarlo en DAM.

El caso de uso es el siguiente:

* Un usuario rellena el formulario adaptable. En la forma adaptativa, se captura el estado civil (ex Single) del usuario y el estado laboral (Ex Retirado).
* Al enviar el formulario, se activa un flujo de trabajo AEM. Este flujo de trabajo etiqueta el documento con el estado civil (único) y el estado de empleo (retirado) y almacena el documento en DAM.
* Una vez que el documento está almacenado en DAM, el administrador debe poder buscar en el documento mediante estas etiquetas. Por ejemplo, la búsqueda en Único o Retirado obtendría los documentos de referencia adecuados.

Para satisfacer este caso de uso, se escribió un paso de proceso personalizado. En este paso, recuperamos los valores de los elementos de datos adecuados de los datos enviados. A continuación, construimos el mosaico de la etiqueta con este valor. Por ejemplo, si el valor del elemento de estado civil es &quot;Single&quot;, el título de la etiqueta se convierte en **Peak:EmployStatus/Single. **Utilizando la API TagManager , encontramos la etiqueta y la aplicamos al DoR.

El siguiente es el código completo para etiquetar y almacenar el documento de registro en AEM DAM.

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

Para que este ejemplo funcione en su sistema, siga los pasos que se indican a continuación:
* [Implementar el paquete de usuario Desarrollo con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargar e implementar el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que establece las etiquetas de los datos del formulario enviado.

* [Descargar el formulario adaptable de ejemplo](assets/tag-and-store-in-dam-assets.zip)

* [Vaya a Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Haga clic en Crear | Archivo Cargar y cargar el archivo sampleadaptiveform.zip

* [Importación de los recursos del artículo](assets/tag-and-store-in-dam-assets.zip) uso de AEM administrador de paquetes
* Abra el [formulario de ejemplo en modo de vista previa](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Complete la sección Personas y envíe el formulario.
* [Vaya a la carpeta Pico de DAM](http://localhost:4502/assets.html/content/dam/Peak). Debería ver DoR en la carpeta Peak. Compruebe las propiedades del documento. Debe etiquetarse adecuadamente.
Felicitaciones!! Ha instalado correctamente el ejemplo en su sistema

* Exploremos el [flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) que se activa al enviar el formulario.
* El primer paso en el flujo de trabajo crea un nombre de archivo único concatenando el nombre del solicitante y el condado de residencia.
* El segundo paso del flujo de trabajo pasa la jerarquía de etiquetas y los elementos de campos de formulario que deben etiquetarse. El paso de proceso extrae el valor de los datos enviados y construye el título de la etiqueta que debe etiquetar el documento.
* Si desea almacenar DoR en una carpeta diferente de DAM, especifique la ubicación de la carpeta utilizando las propiedades de configuración especificadas en la captura de pantalla siguiente.

Los otros dos parámetros son específicos de DoR y Ruta del archivo de datos, tal como se especifica en las opciones de envío del formulario adaptable. Asegúrese de que los valores especificados aquí coincidan con los valores especificados en las opciones de envío del formulario adaptable.

![Puerta de etiqueta](assets/tag_dor_service_configuration.gif)

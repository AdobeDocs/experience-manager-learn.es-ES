---
title: Generar un documento de canal de impresión combinando datos
description: Obtenga información sobre cómo generar un documento de canal de impresión combinando datos contenidos en un flujo de entrada
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 3bfbb4ef-0c51-445a-8d7b-43543a5fa191
last-substantial-update: 2019-07-07T00:00:00Z
duration: 151
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 0%

---

# Generar documentos de canal de impresión mediante datos enviados

Los documentos del canal de impresión generalmente se generan al recuperar datos de una fuente de datos back-end mediante el servicio get del modelo de datos de formulario. En algunos casos, es posible que tenga que generar documentos del canal de impresión con los datos proporcionados. Por ejemplo: el cliente rellena el cambio del formulario de beneficiario y es posible que desee generar un documento del canal de impresión con los datos del formulario enviado. Para realizar este caso de uso, se deben seguir los siguientes pasos

## Crear servicio de relleno previo

El nombre de servicio &quot;ccm-print-test&quot; se utiliza para acceder a este servicio Una vez definido este servicio de rellenado previo, puede acceder a este servicio en el servlet o en la implementación del paso del proceso de flujo de trabajo para generar el documento del canal de impresión.

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### Crear implementación de WorkflowProcess

A continuación se muestra el fragmento de código de implementación workflowProcess. Este código se ejecuta cuando el paso de proceso del flujo de trabajo de AEM está asociado a esta implementación. Esta implementación espera 3 argumentos de proceso que se describen a continuación:

* Nombre de la ruta DataFile especificada al configurar el formulario adaptable
* Nombre de la plantilla del canal de impresión
* Nombre del documento del canal de impresión generado

Línea 98: dado que el formulario adaptable se basa en el modelo de datos de formulario, se extraen los datos que residen en el nodo de datos de afBoundData.
Línea 128: el nombre del servicio de opciones de datos está establecido. Anote el nombre del servicio. Debe coincidir con el nombre devuelto en la línea 45 del listado de código anterior.
Línea 135: el documento se genera mediante el método de procesamiento del objeto PrintChannel


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

Para probar esto en el servidor, siga los siguientes pasos:

* [Configurar el servicio Day CQ Mail.](https://helpx.adobe.com/experience-manager/6-5/communities/using/email.html) Esto es necesario para enviar un correo electrónico con el documento generado como datos adjuntos.
* [Implementación del paquete de usuario Desarrollo con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Asegúrese de haber agregado la siguiente entrada en la configuración del servicio de asignación de usuarios del servicio Apache Sling
* **Desarrollo con ServiceUser.core:getformsresourceresolver=fd-service**
* [Descargue y descomprima los recursos relacionados con este artículo en su sistema de archivos](assets/prefillservice.zip)
* [Importe los siguientes paquetes mediante el Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp)
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Implementar lo siguiente mediante la consola web Felix de AEM](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Este paquete contiene el código mencionado en este artículo.

* [Abrir formulario de cambio de beneficiario](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Asegúrese de que el formulario adaptable está configurado para enviarse al flujo de trabajo de AEM como se muestra a continuación
  ![imagen](assets/generateic.PNG)
* [Configurar el modelo de flujo de trabajo.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Asegúrese de que el paso de proceso y los componentes de envío de correo electrónico estén configurados según su entorno
* [Vista previa del formulario ChangeOfBeneficiaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled): complete algunos detalles y envíe
* El flujo de trabajo debe invocarse y el documento del canal de impresión IC debe enviarse al destinatario especificado en el componente Enviar correo electrónico como archivo adjunto

---
title: Generación de Documento de comunicaciones interactivas para canal de impresión mediante el mecanismo de carpetas de inspección
seo-title: Generación de Documento de comunicaciones interactivas para canal de impresión mediante el mecanismo de carpetas de inspección
description: Usar carpeta vigilada para generar documentos de canal de impresión
seo-description: Usar carpeta vigilada para generar documentos de canal de impresión
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---


# Generación de Documento de comunicaciones interactivas para canal de impresión mediante el mecanismo de carpetas de inspección

Después de haber diseñado y probado el documento de canal de impresión, normalmente necesitará generar el documento realizando una llamada REST o generando documentos de impresión mediante el mecanismo de carpeta de inspección.

En este artículo se explica el caso de uso de la generación de documentos de canal de impresión mediante un mecanismo de carpetas vigiladas.

Cuando se coloca un archivo en la carpeta vigilada, se ejecuta una secuencia de comandos asociada a la carpeta vigilada. Esta secuencia de comandos se explica en el artículo siguiente.

El archivo colocado en una carpeta vigilada tiene la siguiente estructura. El código generará instrucciones para todos los números de cuenta enumerados en el documento XML.

&lt;accountnumber>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumber>

El siguiente código hace lo siguiente:

Línea 1 - Ruta al documento de InteractiveCommunicationsDocument

Líneas 15-20: Obtener la lista de los números de cuenta del documento XML colocado en la carpeta controlada

Líneas 24-25: Obtenga los Canales PrintChannelService e Print asociados al documento.

Línea 30: Pase el número de cuenta como elemento clave al Modelo de datos de formulario.

Líneas 32-36: Configure las opciones de datos para el Documento que se va a generar.

Línea 38: Representar el documento.

Líneas 39-40: Guarda el documento generado en el sistema de archivos.

El extremo REST del modelo de datos de formulario espera un identificador como parámetro de entrada. esta ID se asigna a un atributo de solicitud llamado accountnumber como se muestra en la captura de pantalla siguiente.

![requestattribute](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**Para probar esto en su sistema local, siga las siguientes instrucciones:**

* Configure Tomcat tal como se describe en este [artículo.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat tiene el archivo de guerra que genera los datos de muestra.
* Configure el servicio conocido como usuario del sistema como se describe en este [artículo](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Asegúrese de que este usuario del sistema tenga permisos de lectura en el nodo siguiente. Para otorgar los permisos de inicio de sesión al administrador [de](https://localhost:4502/useradmin) usuarios y buscar los &quot;datos&quot; del usuario del sistema y otorgar los permisos de lectura en el nodo siguiente mediante el tabulador en la ficha Permisos
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importe los siguientes paquetes en AEM mediante el administrador de paquetes. Este paquete contiene lo siguiente:


* [Ejemplo de Documento de comunicaciones interactivas](assets/retirementstatementprint.zip)
* [Secuencia de comandos de carpeta vigilada](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configuración de origen de datos](assets/datasource.zip)

* Abra el archivo /etc/fd/watchfolder/scripts/PrintPDF.ecma. Asegúrese de que la ruta de acceso al documento interactivo de CommunicationsDocument en la línea 1 apunte al documento correcto que desea imprimir

* Modifique saveLocation según sus preferencias en la línea 2

* Cree un archivo accountnumber.xml con el siguiente contenido

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* Coloque el archivo accountnumber.xml en el directorio C:\RenderPrintChannel\input folder.

* Los archivos PDF generados se escriben en saveLocation tal como se especifica en la secuencia de comandos de ecma.

>[!NOTE]
>
>Si planea usar esto en un sistema operativo que no sea Windows, navegue hasta
>
>/etc/fd/watchfolder /config/PrintChannelDocument y cambie folderPath según sus preferencias


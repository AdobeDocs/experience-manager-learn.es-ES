---
title: Generar documentos de comunicaciones interactivas para el canal Imprimir mediante el mecanismo de carpeta de inspección
description: Usar carpeta vigilada para generar documentos del canal de impresión
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 1%

---

# Generar documentos de comunicaciones interactivas para el canal Imprimir mediante el mecanismo de carpeta de inspección

Después de diseñar y probar el documento del canal de impresión, normalmente deberá generar el documento realizando una llamada REST o generando documentos de impresión mediante el mecanismo de carpeta de inspección.

Este artículo explica el caso de uso de la generación de documentos del canal de impresión mediante el mecanismo de carpetas vigiladas.

Cuando suelta un archivo en la carpeta inspeccionada, se ejecuta una secuencia de comandos asociada a la carpeta inspeccionada. Esta secuencia de comandos se explica en el artículo siguiente.

El archivo colocado en la carpeta inspeccionada tiene la siguiente estructura. El código generará instrucciones para todos los números de cuenta enumerados en el documento XML.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

La lista de códigos siguiente hace lo siguiente:

Línea 1: ruta al documento de comunicaciones interactivas

Líneas 15-20: obtenga la lista de números de cuenta del documento XML colocado en la carpeta vigilada

Líneas 24 -25: Obtenga el servicio PrintChannel y el canal de impresión asociados al documento.

Línea 30: Pase el número de cuenta como elemento clave al modelo de datos de formulario.

Líneas 32-36: define las opciones de datos para el documento que se va a generar.

Línea 38: procese el documento.

Líneas 39-40: guarda el documento generado en el sistema de archivos.

El extremo REST del modelo de datos de formulario espera un ID como parámetro de entrada. este id se asigna a un atributo de solicitud denominado accountnumber, como se muestra en la captura de pantalla siguiente.

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

* Configure Tomcat como se describe en este [ artículo.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat tiene el archivo WAR que genera los datos de ejemplo.
* Configure el usuario del sistema alias de servicio como se describe en este [artículo](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Asegúrese de que este usuario del sistema tiene permisos de lectura en el siguiente nodo. Para conceder los permisos, inicie sesión en [user admin](https://localhost:4502/useradmin), busque los &quot;datos&quot; del usuario del sistema y conceda los permisos de lectura en el siguiente nodo presionando la pestaña Permisos
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importe los siguientes paquetes en AEM mediante el administrador de paquetes. Este paquete contiene lo siguiente:


* [Ejemplo de documento de comunicaciones interactivas](assets/retirementstatementprint.zip)
* [Script de carpeta inspeccionada](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configuración de origen de datos](assets/datasource.zip)

* Abra el archivo /etc/fd/watchfolder/scripts/PrintPDF.ecma. Asegúrese de que la ruta al interactiveCommunicationsDocument de la línea 1 apunte al documento correcto que desea imprimir

* Modifique saveLocation según sus preferencias en la Línea 2

* Cree el archivo accountnumbers.xml con el siguiente contenido

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


* Coloque accountnumbers.xml en la carpeta C:\RenderPrintChannel\input.

* Los archivos PDF generados se escriben en saveLocation tal como se especifica en el script ecma.

>[!NOTE]
>
>Si planea usar esto en un sistema operativo que no sea de Windows, vaya a
>
>/etc/fd/watchfolder /config/PrintChannelDocument y cambie folderPath según sus preferencias

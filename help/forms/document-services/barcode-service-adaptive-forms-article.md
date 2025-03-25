---
title: Servicio De Códigos De Barras Con Forms Adaptable
description: Usar el servicio de código de barras para descodificar el código de barras y rellenar los campos de formulario de los datos extraídos.
feature: Barcoded Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# Servicio De Códigos De Barras Con Forms Adaptable{#barcode-service-with-adaptive-forms}

Este artículo mostrará el uso del servicio de código de barras para rellenar formularios adaptables. El caso de uso es el siguiente:

1. El usuario agrega PDF con código de barras como datos adjuntos del formulario adaptable
1. La ruta del archivo adjunto se envía al servlet
1. El servlet descodificó el código de barras y devuelve los datos en formato JSON
1. A continuación, el formulario adaptable se rellena utilizando los datos descodificados

El siguiente código descodifica el código de barras y rellena un objeto JSON con los valores descodificados. A continuación, el servlet devuelve el objeto JSON en su respuesta a la aplicación que realiza la llamada.



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

El siguiente es el código de servlet. Se llama a este servlet cuando el usuario agrega un archivo adjunto al formulario adaptable. El servlet devuelve el objeto JSON de nuevo a la aplicación que realiza la llamada. A continuación, la aplicación que realiza la llamada rellena el formulario adaptable con los valores extraídos del objeto JSON.

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

El siguiente código forma parte de la biblioteca de cliente a la que hace referencia el formulario adaptable. Cuando un usuario agrega el archivo adjunto al formulario adaptable, este código se activa. El código realiza una llamada de GET al servlet con la ruta del archivo adjunto pasada en el parámetro de solicitud. A continuación, los datos recibidos de la llamada al servlet se utilizan para rellenar el formulario adaptable.

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>El formulario adaptable incluido con este paquete se creó con AEM Forms 6.4. Si tiene intención de utilizar este paquete en el entorno de AEM Forms 6.3, cree el formulario adaptable en AEM Form 6.3

Línea 12: código personalizado para obtener la resolución del servicio. Este paquete se incluye como parte de los recursos de este artículo.

Línea 23: Llame al método extractBarCode de DocumentServices para que el objeto JSON se rellene con datos descodificados

Para que esto funcione en su sistema, siga los siguientes pasos:

1. [Descargue BarcodeService.zip](assets/barcodeservice.zip) e impórtelo a AEM mediante el administrador de paquetes
1. [Descargar e instalar el paquete de servicios de documentos personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Descargue e instale el paquete Desarrollando con usuario de servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Descargue el formulario de ejemplo de PDF](assets/barcode.pdf)
1. Dirija su explorador al [formulario adaptable de ejemplo](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. Cargue el PDF de muestra proporcionado
1. Debería ver los formularios rellenados con los datos

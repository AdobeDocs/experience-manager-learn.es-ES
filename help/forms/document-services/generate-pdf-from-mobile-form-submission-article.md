---
title: Generar PDF a partir del envío de formulario HTML5
description: Generar PDF a partir del envío de formulario móvil
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---

# Generar PDF a partir del envío de formulario HTML5 {#generate-pdf-from-htm-form-submission}

Este artículo le guiará por los pasos involucrados en la generación de pdf a partir de un envío de formulario del HTML5 (también conocido como Forms móvil). Esta demostración también explicará los pasos necesarios para agregar una imagen al formulario HTML5 y combinar la imagen en el pdf final.


Para combinar los datos enviados con la plantilla xdp, se hace lo siguiente

Escriba un servlet para gestionar el envío de formularios de HTML5

* Dentro de este servlet, obtenga los datos enviados
* Combine estos datos con la plantilla xdp para generar pdf
* Vuelva a enviar el pdf a la aplicación que realiza la llamada.

El siguiente es el código de servlet que extrae los datos enviados de la solicitud. A continuación, llama al método personalizado documentServices .mobileFormToPDF para obtener el pdf.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
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

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

Para agregar una imagen al formulario móvil y mostrarla en el pdf hemos utilizado lo siguiente

Plantilla XDP : en la plantilla xdp hemos agregado un campo de imagen y un botón llamado btnAddImage. El siguiente código gestiona el evento click de btnAddImage en nuestro perfil personalizado. Como puede ver, almacenamos en déclencheur el evento de clics del archivo 1. No se necesita ningún código en el xdp para lograr este caso de uso

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Perfil personalizado](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). El uso de perfiles personalizados facilita la manipulación de objetos DOM HTML del formulario móvil. Se agrega un elemento de archivo oculto al HTML.jsp. Cuando el usuario hace clic en &quot;Añadir su foto&quot;, se déclencheur el evento click del elemento del archivo. Esto permite al usuario examinar y seleccionar la fotografía que desea adjuntar. A continuación, utilizamos el objeto FileReader de javascript para obtener la cadena codificada base64 de la imagen. La cadena de imagen base64 se almacena en el campo de texto del formulario. Cuando se envía el formulario, se extrae este valor e se inserta en el elemento img del XML. Este XML se utiliza para combinar con el xdp para generar el pdf final.

El perfil personalizado utilizado para este artículo se ha puesto a su disposición como parte de los recursos de este artículo.

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

El código anterior se ejecuta cuando se déclencheur el evento click del elemento del archivo. Línea 5 extraemos el contenido del archivo cargado como base64 string y lo almacenamos en el campo de texto. Este valor se extrae cuando el formulario se envía a nuestro servlet.

A continuación, configuramos las siguientes propiedades (avanzadas) de nuestro formulario móvil en AEM

* Dirección URL de envío: http://localhost:4502/bin/handlemobileformsubmission. Este es nuestro servlet que combina los datos enviados con la plantilla xdp
* Perfil de procesamiento del HTML: asegúrese de seleccionar &quot;AddImageToMobileForm&quot;. Esto colocará el código en déclencheur para agregar una imagen al formulario.

Para probar esta capacidad en su propio servidor, siga los siguientes pasos:

* [Implementar el paquete AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Implementar el paquete Desarrollar con el usuario de servicios](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargue e instale el paquete asociado a este artículo.](assets/pdf-from-mobile-form-submission.zip)

* Asegúrese de que la URL de envío y el perfil de procesamiento del HTML estén correctamente configurados viendo la página de propiedades de la variable  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Vista previa del XDP como html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Agregue una imagen al formulario y envíelo. Debe volver a obtener el PDF con la imagen que contiene.

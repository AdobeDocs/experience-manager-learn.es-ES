---
title: Generar PDF a partir de envío de formulario HTML5
seo-title: Generar PDF a partir de envío de formulario HTML5
description: Generar archivo PDF a partir del envío de formulario móvil
seo-description: Generar archivo PDF a partir del envío de formulario móvil
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Generar PDF a partir de envío de formulario HTML5 {#generate-pdf-from-htm-form-submission}

Este artículo le guiará por los pasos necesarios para generar PDF a partir de un envío de formulario HTML5 (también conocido como Mobile Forms). Esta demostración también explicará los pasos necesarios para agregar una imagen a un formulario HTML5 y combinar la imagen en el PDF final.

Para ver una demostración en vivo de esta capacidad, visite el servidor [de](https://forms.enablementadobe.com/content/samples/samples.html?query=0) muestra y busque &quot;Formulario móvil a PDF&quot;.

Para combinar los datos enviados con la plantilla xdp, se hace lo siguiente

Escribir un servlet para controlar el envío de formularios HTML5

* Dentro de este servlet, obtenga los datos enviados
* Combinar estos datos con la plantilla xdp para generar pdf
* Volver a transmitir el PDF a la aplicación que realiza la llamada

El siguiente es el código servlet que extrae los datos enviados de la solicitud. A continuación, llama al método documentServices .mobileFormToPDF personalizado para obtener el PDF.

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

Para agregar una imagen al formulario móvil y mostrarla en el PDF, hemos utilizado lo siguiente

Plantilla XDP: en la plantilla xdp hemos añadido un campo de imagen y un botón denominado btnAddImage. El siguiente código controla el evento de clics de btnAddImage en nuestro perfil personalizado. Como puede ver, activamos el evento de clic del archivo1. No se necesita código en xdp para llevar a cabo este caso de uso

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Perfil](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)personalizado. El uso de perfil personalizado facilita la manipulación de objetos DOM HTML del formulario móvil. Se agrega un elemento de archivo oculto al archivo HTML.jsp. Cuando el usuario hace clic en &quot;Añadir la foto&quot; activamos el evento de clic del elemento de archivo. Esto permite al usuario examinar y seleccionar la fotografía que desea adjuntar. A continuación, utilizamos el objeto FileReader de javascript para obtener la cadena codificada base64 de la imagen. La cadena de imagen base64 se almacena en el campo de texto del formulario. Cuando se envía el formulario, se extrae este valor y se inserta en el elemento img del XML. Este XML se utiliza para combinar con el xdp para generar el PDF final.

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

El código anterior se ejecuta cuando activamos el evento de clic del elemento de archivo. Línea 5 extraemos el contenido del archivo cargado como una cadena base64 y lo almacenamos en el campo de texto. Este valor se extrae cuando el formulario se envía a nuestro servlet.

Luego configuramos las siguientes propiedades (avanzadas) de nuestro formulario móvil en AEM

* Enviar URL: http://localhost:4502/bin/handlemobileformsubmission. Este es nuestro servlet que combinará los datos enviados con la plantilla xdp
* PERFIL de procesamiento HTML: asegúrese de seleccionar &quot;AddImageToMobileForm&quot;. Esto activará el código para agregar una imagen al formulario.

Para probar esta capacidad en su propio servidor, siga los pasos siguientes:

* [Implementar el paquete AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Implementar el paquete Desarrollar con el usuario de servicios](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargue e instale el paquete asociado con este artículo.](assets/pdf-from-mobile-form-submission.zip)

* Asegúrese de que la URL de envío y el perfil de procesamiento HTML están correctamente establecidos al ver la página de propiedades de [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Previsualización del XDP como html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Añada una imagen al formulario y envíela. Debería recuperar el PDF con la imagen dentro.


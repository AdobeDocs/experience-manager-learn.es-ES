---
title: Generar PDF desde envío de formulario HTML5
description: Generar PDF a partir del envío de formularios móviles
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
duration: 132
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# Generar PDF desde envío de formulario HTML5 {#generate-pdf-from-htm-form-submission}

Este artículo le guiará por los pasos necesarios para generar el pdf a partir del envío de un formulario HTML5 (también conocido como Forms móvil). En esta demostración también se explican los pasos necesarios para agregar una imagen al formulario de HTML5 y combinarla en el PDF final.


Para combinar los datos enviados en la plantilla xdp, realizamos lo siguiente

Escriba un servlet para gestionar el envío del formulario de HTML5

* Dentro de este servlet, obtenga los datos enviados
* Combine estos datos con la plantilla xdp para generar un pdf
* Vuelva a enviar el PDF a la aplicación que realiza la llamada

El siguiente es el código de servlet que extrae los datos enviados de la solicitud. A continuación, llama al método personalizado documentServices .mobileFormToPDF para obtener el PDF.

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

Plantilla XDP: en la plantilla xdp hemos añadido un campo de imagen y un botón llamado btnAddImage. El siguiente código administra el evento de clic de btnAddImage en el perfil personalizado. Como puede ver, almacenamos en déclencheur el evento de clic file1. No se necesita codificación en el xdp para realizar este caso de uso

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Perfil personalizado](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). El uso de perfiles personalizados facilita la manipulación de los objetos DOM HTML del formulario móvil. Se agrega un elemento de archivo oculto al archivo HTML.jsp. Cuando el usuario hace clic en &quot;Añadir su foto&quot;, almacenamos en déclencheur el evento de clic del elemento de archivo. Esto permite al usuario examinar y seleccionar la fotografía que desea adjuntar. A continuación, se utiliza el objeto FileReader de javascript para obtener la cadena codificada en base64 de la imagen. La cadena de imagen base64 se almacena en el campo de texto del formulario. Cuando se envía el formulario, extraemos este valor e lo insertamos en el elemento img del XML. A continuación, este XML se utiliza para combinar con el xdp para generar el pdf final.

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

El código anterior se ejecuta cuando almacenamos en déclencheur el evento de clic del elemento de archivo. Línea 5: extraemos el contenido del archivo cargado como una cadena base64 y lo almacenamos en el campo de texto. Este valor se extrae cuando se envía el formulario a nuestro servlet.

AEM A continuación, configuramos las siguientes propiedades (avanzadas) de nuestro formulario móvil en el espacio de trabajo de la plataforma de datos de

* Enviar URL: http://localhost:4502/bin/handlemobileformsubmission. Este es nuestro servlet que combinará los datos enviados con la plantilla xdp
* Perfil de procesamiento del HTML: asegúrese de seleccionar &quot;AddImageToMobileForm&quot;. Esto almacenará en déclencheur el código para agregar una imagen al formulario.

Para probar esta capacidad en su propio servidor, siga los siguientes pasos:

* [Implementar el paquete AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Implementación del paquete Desarrollar con usuario de servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargue e instale el paquete asociado con este artículo.](assets/pdf-from-mobile-form-submission.zip)

* Asegúrese de que la dirección URL de envío y el perfil de procesamiento del HTML estén correctamente configurados. Para ello, consulte la página de propiedades del  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Previsualice el XDP como HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Agregue una imagen al formulario y envíela. Usted debe obtener PDF de nuevo con la imagen en ella.

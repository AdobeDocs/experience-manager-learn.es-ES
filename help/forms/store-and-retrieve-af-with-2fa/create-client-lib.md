---
title: Creación de bibliotecas de cliente
description: Crear clientlibrary para gestionar el evento click del botón "Guardar y salir"
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 6%

---

# Crear biblioteca de cliente

Crear [biblioteca de cliente](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=es) que incluye el código para invocar el método `doAjaxSubmitWithFileAttachment` del `guideBridge` API en el evento click del botón identificado por la clase CSS **savebutton**.  Pasamos los datos del formulario adaptable, `fileMap`y `mobileNumber` al extremo que escucha en `**/bin/storeafdatawithattachments`

Una vez guardados los datos del formulario, se genera un identificador de aplicación único que se presenta al usuario en un cuadro de diálogo. Al descartar el cuadro de diálogo, el usuario recibe el formulario que le permite recuperar el formulario adaptable guardado con el identificador de aplicación único.

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> Hemos usado [biblioteca javascript de bootbox](http://bootboxjs.com/examples.html) cuadro de diálogo para mostrar

Las bibliotecas de cliente utilizadas en este ejemplo pueden ser [descargado desde aquí](assets/client-libraries.zip)

## Pasos siguientes

[Verificar usuarios con servicio OTP](./verify-users-with-otp.md)
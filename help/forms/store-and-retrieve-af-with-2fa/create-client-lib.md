---
title: Creación de bibliotecas de cliente
description: Cree una biblioteca de cliente para gestionar el evento de clic del botón Guardar y salir
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

Crear [biblioteca del cliente](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=es) que incluirá el código para invocar el método `doAjaxSubmitWithFileAttachment` de la `guideBridge` API en el evento &quot;click&quot; del botón identificado por la clase CSS **savebutton**.  Pasamos los datos del formulario adaptable, `fileMap`, y el `mobileNumber` al extremo que escucha en `**/bin/storeafdatawithattachments`

Una vez guardados los datos del formulario, se genera un ID de aplicación único que se presenta al usuario en un cuadro de diálogo. Al cerrar el cuadro de diálogo, el usuario se dirige al formulario, que le permite recuperar el formulario adaptable guardado mediante el ID de aplicación único.

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
> Hemos utilizado [biblioteca javascript de bootbox](http://bootboxjs.com/examples.html) para mostrar el cuadro de diálogo

Las bibliotecas de cliente utilizadas en este ejemplo pueden ser [descargado desde aquí](assets/client-libraries.zip)

## Pasos siguientes

[Verificación de usuarios con el servicio OTP](./verify-users-with-otp.md)
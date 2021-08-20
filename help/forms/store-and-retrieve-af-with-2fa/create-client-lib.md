---
title: Creación de bibliotecas de cliente
description: Crear clientlibrary para gestionar el evento click del botón "Guardar y salir"
feature: Formularios adaptables
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Desarrollo
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 2%

---

# Crear biblioteca de cliente

Cree [client lib](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) que incluirá el código para invocar el método `doAjaxSubmitWithFileAttachment` de la API `guideBridge` en el evento click del botón identificado por la clase CSS **savebutton**.  Pasamos los datos del formulario adaptable, `fileMap` y `mobileNumber` al punto final que escucha en `**/bin/storeafdatawithattachments`

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
> Hemos utilizado la [biblioteca javascript de bootbox](http://bootboxjs.com/examples.html) para mostrar el cuadro de diálogo

Las bibliotecas de cliente utilizadas en este ejemplo se pueden [descargar desde aquí](assets/client-libraries.zip)

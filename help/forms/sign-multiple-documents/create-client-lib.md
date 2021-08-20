---
title: Crear biblioteca de cliente
description: Código de la biblioteca del cliente para recuperar el siguiente formulario que se va a firmar
feature: Formularios adaptables
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Desarrollo
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 4%

---

# Crear una biblioteca de cliente

Cree una biblioteca de cliente personalizada, clientlib for short, para extraer los parámetros de url y pasar esos parámetros en la llamada de GET. La llamada de GET se realiza a un servlet montado en /bin/getnextformtosign que devuelve la url del siguiente formulario para iniciar sesión en el paquete.

El siguiente es el código utilizado en la función clientlib javascript


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[La clientlib puede descargarse desde aquí](assets/get-next-form-client-lib.zip)

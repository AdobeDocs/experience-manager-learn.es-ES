---
title: Crear biblioteca de cliente
description: Código de biblioteca de cliente para recuperar el siguiente formulario que se va a firmar
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 34
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# Crear una biblioteca de cliente

Cree una biblioteca de cliente personalizada, clientlib para abreviar, para extraer los parámetros de URL que pasan esos parámetros en la llamada de GET. La llamada de GET se realiza a un servlet montado en /bin/getnextformtosign, que devuelve la dirección URL del siguiente formulario para iniciar sesión en el paquete.

El siguiente es el código utilizado en la función clientlib de javascript


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

## Recursos

[La clientlib se puede descargar desde aquí](assets/get-next-form-client-lib.zip)

## Siguientes pasos

[Crear una plantilla de formulario personalizada para este caso de uso](./create-af-template.md)
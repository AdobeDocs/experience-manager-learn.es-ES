---
title: 'Almacenar y recuperar datos de formulario de la base de datos MySQL: crear biblioteca de cliente'
description: Tutorial de varias partes para guiarle por los pasos necesarios para almacenar y recuperar datos de formulario
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: eef98a55-80d0-4598-abf2-02a6c5247b64
duration: 120
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# Crear biblioteca de cliente

AEM La biblioteca de clientes de administra todo el código JavaScript del lado del cliente. Para este artículo, he creado un JavaScript simple para recuperar los datos del formulario adaptable mediante la API de Guide Bridge. Una vez recuperados los datos del formulario adaptable, se realiza la llamada al POST al servlet para insertar o actualizar los datos del formulario adaptable en la base de datos. La función getALLUrlParams devuelve los parámetros de la dirección URL. Si el parámetro guid está presente en la dirección URL, es necesario realizar la operación de actualización; en caso contrario, se trata de una operación de inserción. El resto de la funcionalidad se controla en el código asociado con el evento de clic de la clase .savebutton.

>[!NOTE]
>
>La biblioteca de cliente se proporciona como parte de los recursos de este tutorial

```javascript
function getAllUrlParams(url) {

    // get query string from url (optional) or window
    var queryString = url ? url.split('?')[1] : window.location.search.slice(1);

    // we'll store the parameters here
    var obj = {};

    // if query string exists
    if (queryString) {

        // stuff after # is not part of query string, so get rid of it
        queryString = queryString.split('#')[0];

        // split our query string into its component parts
        var arr = queryString.split('&');

        for (var i = 0; i < arr.length; i++) {
            // separate the keys and the values
            var a = arr[i].split('=');

            // set parameter name and value (use 'true' if empty)
            var paramName = a[0];
            var paramValue = typeof(a[1]) === 'undefined' ? true : a[1];

            // (optional) keep case consistent
            paramName = paramName.toLowerCase();
            if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();

            // if the paramName ends with square brackets, e.g. colors[] or colors[2]
            if (paramName.match(/\[(\d+)?\]$/)) {

                // create key if it doesn't exist
                var key = paramName.replace(/\[(\d+)?\]/, '');
                if (!obj[key]) obj[key] = [];

                // if it's an indexed array e.g. colors[2]
                if (paramName.match(/\[\d+\]$/)) {
                    // get the index value and add the entry at the appropriate position
                    var index = /\[(\d+)\]/.exec(paramName)[1];
                    obj[key][index] = paramValue;
                } else {
                    // otherwise add the value to the end of the array
                    obj[key].push(paramValue);
                }
            } else {
                // we're dealing with a string
                if (!obj[paramName]) {
                    // if it doesn't exist, create property
                    obj[paramName] = paramValue;
                } else if (obj[paramName] && typeof obj[paramName] === 'string') {
                    // if property does exist and it's a string, convert it to an array
                    obj[paramName] = [obj[paramName]];
                    obj[paramName].push(paramValue);
                } else {
                    // otherwise add the property
                    obj[paramName].push(paramValue);
                }
            }
        }
    }

    return obj;
}

$(document).ready(function() {
    console.log(window.location.hostname + "   " + window.location.protocol);
    var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
    var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
    linktext.visible = false;
    linktext1.visible = false;
    $(".savebutton").click(function() {
        var params = getAllUrlParams(window.location.href);
        console.log(getAllUrlParams(window.location.href));
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                console.log("The data is " + guideResultObject.data);
                let xhr = new XMLHttpRequest();
                xhr.open('POST', '/bin/storeafdata');
                let formData = new FormData();
                if (typeof(params.guid) != "undefined") {
                    formData.append("operation", "update");
                    formData.append("guid", params.guid);

                }
                if (typeof(params.guid) == "undefined") {
                    formData.append("operation", "insert");


                }


                formData.append("formdata", guideResultObject.data);
                xhr.send(formData);
                xhr.onload = function(e) {
                    console.log("The data is ready");
                    if (this.status == 200) {
                        var jsonResponse = JSON.parse(this.response);
                        console.log(jsonResponse.guid);
                        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                        linktext1.visible = true;
                        linktext.value =  "/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled&guid=" + jsonResponse.guid;
                        linktext.visible = true;
                        guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                    }

                }
            }
        });


    });


});
```

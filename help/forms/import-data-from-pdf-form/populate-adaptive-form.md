---
title: Rellenar formularios adaptables mediante el método setData
description: Envíe el archivo PDF cargado para extraer datos y rellenar el formulario adaptable con los datos extraídos
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 53
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# Realizar llamada de Ajax

Cuando el usuario ha cargado el archivo PDF, es necesario realizar una llamada al POST a un servlet y pasar el documento del PDF cargado en la solicitud del POST. La solicitud del POST devuelve una ruta a los datos exportados en el repositorio crx

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

El servlet montado en **_/bin/ExtractDataFromPDF_** extrae los datos del archivo PDF y devuelve la ruta del nodo crx donde se almacenan los datos extraídos.
El [setData de GuideBridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) a continuación, se utiliza el método para establecer los datos del formulario adaptable.

## Pasos siguientes

[Implementar los recursos de muestra](./test-the-solution.md)

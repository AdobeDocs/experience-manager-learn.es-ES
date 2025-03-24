---
title: Rellenar formularios adaptables mediante el método setData
description: Envíe el archivo PDF cargado para extraer datos y rellenar el formulario adaptable con los datos extraídos
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# Realizar llamada de Ajax

Cuando el usuario ha cargado el archivo PDF, necesitamos realizar una llamada de POST a un servlet y pasar el documento de PDF cargado en la solicitud de POST. La petición POST devuelve una ruta a los datos exportados en el repositorio crx

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
A continuación, se utiliza el método [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) para establecer los datos del formulario adaptable.

## Siguientes pasos

[Implementar los recursos de muestra](./test-the-solution.md)

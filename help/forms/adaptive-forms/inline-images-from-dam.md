---
title: Visualización de imágenes DAM en línea en Forms adaptable
description: Mostrar imágenes DAM en línea en Forms adaptable
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
duration: 60
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Mostrar imagen DAM en Forms adaptable

Un caso de uso común es mostrar las imágenes que residen en el repositorio CRX en línea en un formulario adaptable.

## Agregar imagen de marcador

El primer paso es anteponer un div de marcador de posición al componente del panel. En el código siguiente, el componente del panel se identifica con su nombre de clase CSS photo-upload. La función JavaScript forma parte de la biblioteca de cliente asociada a los formularios adaptables. Se llama a esta función en el evento initialize del componente file attachment.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Mostrar imagen en línea

Una vez que el usuario ha seleccionado la imagen, el campo oculto ImageName se rellena con el nombre de imagen seleccionado. Este nombre de imagen se pasa a continuación a la función damURLToFile que invoca la función createFile para convertir una URL en un blob para FileReader.readAsDataURL().

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### Implementación en el servidor

* Descargue e instale la [biblioteca de cliente e imágenes de ejemplo](assets/InlineDAMImage.zip) en su instancia de AEM mediante el Administrador de paquetes de AEM.
* Descargue e instale [formulario de ejemplo](assets/FieldInspectionForm.zip) en su instancia de AEM mediante el administrador de paquetes de AEM.
* Dirija su explorador a [FieldInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Seleccione uno de los accesorios
* Debería ver la imagen en el formulario

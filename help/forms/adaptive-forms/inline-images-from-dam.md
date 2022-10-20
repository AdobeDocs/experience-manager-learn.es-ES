---
title: Visualización de imágenes en línea en Forms adaptable
description: Mostrar imágenes cargadas en línea en Forms adaptable
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
kt: kt-11307
source-git-commit: 853c4fedd4b8db594aa0b53fd2d27d996811f14e
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Mostrar la imagen DAM en Forms adaptable

Un caso de uso común es mostrar las imágenes que residen en el repositorio crx en línea en un formulario adaptable.

## Agregar imagen de marcador de posición

El primer paso es anteponer un div marcador de posición al componente de panel. En el código siguiente, el componente del panel se identifica con su nombre de clase CSS de carga de fotos. La función JavaScript forma parte de la biblioteca de cliente asociada a los formularios adaptables. Se llama a esta función en el suceso initialize del componente de archivo adjunto.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Mostrar imagen en línea

Una vez que el usuario ha seleccionado la imagen, el campo oculto ImageName se rellena con el nombre de imagen seleccionado. Este nombre de imagen se pasa entonces a la función damURLToFile que invoca la función createFile para convertir una URL a un Blob para FileReader.readAsDataURL().

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

### Implementar en el servidor

* Descargue e instale el [biblioteca de cliente e imágenes de ejemplo](assets/InlineDAMImage.zip) en la instancia de AEM mediante el Administrador de paquetes de AEM.
* Descargue e instale el [formulario de ejemplo](assets/FieldInspectionForm.zip) en su instancia de AEM usando AEM administrador de paquetes.
* Especifique el explorador para [Formulario de inspección de campos](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Seleccione una de las funciones
* Debería ver la imagen mostrada en el formulario
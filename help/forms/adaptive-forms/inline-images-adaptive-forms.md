---
title: Visualización de imágenes en línea en Forms adaptable
description: Mostrar imágenes cargadas en línea en Forms adaptable
feature: Adaptive Forms
topics: development
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# Imágenes en línea en Adaptive Forms

Un caso de uso común es mostrar la imagen cargada como una imagen en línea en el formulario adaptable. De forma predeterminada, la imagen cargada se muestra como un vínculo y esta experiencia se puede mejorar mostrando la imagen en el formulario adaptable. Este artículo le guiará por los pasos necesarios para mostrar la imagen en línea.

## Agregar imagen de marcador de posición

El primer paso es anteponer un div marcador de posición al componente de archivo adjunto. En el código siguiente, el componente de archivo adjunto se identifica con su nombre de clase CSS de carga de foto. La función JavaScript forma parte de la biblioteca de cliente asociada a los formularios adaptables. Se llama a esta función en el suceso initialize del componente de archivo adjunto.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Mostrar imagen en línea

Una vez que el usuario ha cargado la imagen, la función que se muestra a continuación se invoca en el evento commit del componente de archivo adjunto. La función recibe el objeto de archivo cargado como argumento.

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### Implementar en el servidor

* Descargue e instale el [biblioteca cliente](assets/inline-image-client-library.zip) en la instancia de AEM mediante AEM administrador de paquetes.
* Descargue e instale el [formulario de ejemplo](assets/inline-image-af.zip) en su instancia de AEM usando AEM administrador de paquetes.
* Especifique el explorador para [Agregar imagen en línea](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Haga clic en el botón &quot;Adjuntar la foto&quot; para añadir la imagen

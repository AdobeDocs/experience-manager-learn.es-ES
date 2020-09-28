---
title: Visualización de imágenes en línea en Forms adaptable
seo-title: Visualización de imágenes en línea en Forms adaptable
description: Mostrar imágenes cargadas en línea en Forms adaptable
seo-description: Mostrar imágenes cargadas en línea en Forms adaptable
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Imágenes en línea en Forms adaptable

Un caso de uso común es mostrar la imagen cargada como una imagen en línea en el formulario adaptable. De forma predeterminada, la imagen cargada se muestra como vínculo y esta experiencia se puede mejorar mostrando la imagen en Formulario adaptable. Este artículo le guiará por los pasos necesarios para mostrar imágenes en línea.

## Añadir imagen de marcador de posición

El primer paso es anteponer un div marcador de posición al componente de datos adjuntos de archivo. En el código que aparece debajo, el componente de archivo adjunto se identifica mediante el nombre de clase CSS de la carga de fotografías. La función JavaScript forma parte de la biblioteca del cliente asociada a los formularios adaptables. Esta función se llama en el evento de inicialización del componente de datos adjuntos del archivo.

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

Después de que el usuario haya cargado la imagen, la función que se muestra a continuación se invoca en el evento de confirmación del componente de archivo adjunto. La función recibe el objeto de archivo cargado como argumento.

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

* Descargue e instale la biblioteca [de](assets/inline-image-client-library.zip) cliente en la instancia de AEM mediante AEM administrador de paquetes.
* Descargue e instale el formulario [de](assets/inline-image-af.zip) ejemplo en la instancia de AEM mediante AEM administrador de paquetes.
* Seleccione el explorador para [Añadir la imagen en línea](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Haga clic en el botón &quot;Adjuntar la foto&quot; para agregar la imagen
---
title: Visualización de imágenes en línea en formularios adaptables
seo-title: Visualización de imágenes en línea en formularios adaptables
description: Mostrar imágenes cargadas en línea en formularios adaptables
seo-description: Mostrar imágenes cargadas en línea en formularios adaptables
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---


# Imágenes en línea en formularios adaptables

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

* Descargue e instale la [biblioteca de cliente](assets/inline-image-client-library.zip) en su instancia de AEM mediante el administrador de paquetes de AEM.
* Descargue e instale el [ejemplo de formulario](assets/inline-image-af.zip) en su instancia de AEM con el administrador de paquetes de AEM.
* Apunte el navegador para [Agregar imagen en línea](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Haga clic en el botón &quot;Adjuntar la foto&quot; para añadir la imagen
---
title: Incrustación de formularios adaptables Forms/HTML5 en una página web
description: Pasos de configuración necesarios para incrustar formularios adaptables de Forms o HTMLAEM 5 en una página web no.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 13%

---

# Incrustación de formularios adaptables o formularios de HTML5 en la página web

El formulario adaptable incrustado es completamente funcional, y los usuarios pueden rellenarlo y enviarlo sin abandonar la página. Esto permite al usuario mantenerse en el contexto de otros elementos de la página web e interactuar simultáneamente con el formulario.

En el siguiente vídeo se explican los pasos necesarios para incrustar un formulario adaptable o HTML5 en una página web.
Consulte la [documentación](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html) para conocer los requisitos previos recomendados, las prácticas recomendadas, etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

Puede descargar los archivos de muestra utilizados en el vídeo [desde aquí](assets/embedding-af-web-page.zip)

El siguiente es el código utilizado para recuperar el formulario adaptable e incrustar el formulario en el contenedor identificado por el nombre de clase **derecha**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```

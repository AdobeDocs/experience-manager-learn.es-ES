---
title: Incrustación de formularios Forms/HTML5 adaptables en una página web
description: Pasos de configuración necesarios para incrustar formularios Forms adaptables o HTML5 en una página web que no sea AEM.
feature: Formularios adaptables
type: Tutorial
version: 6.4,6.5
topic: Desarrollo
role: Developer
level: Beginner
kt: 8390
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---


# Incrustación de formularios adaptables o HTML5 en una página web

El formulario adaptable integrado es totalmente funcional y los usuarios pueden rellenar y enviar el formulario sin salir de la página. Ayuda al usuario a permanecer en el contexto de otros elementos de la página web e interactuar simultáneamente con el formulario.

En el siguiente vídeo se explican los pasos necesarios para integrar un formulario Adaptable o HTML5 en una página web.
Consulte la [documentación](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) para conocer los requisitos previos, las prácticas recomendadas, etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

Puede descargar los archivos de muestra utilizados en el vídeo [desde aquí](assets/embedding-af-web-page.zip)

El siguiente es el código utilizado para recuperar el formulario adaptable e incrustar el formulario en el contenedor identificado por el nombre de clase **right**

```javascript
$(document).ready(function(){
  
	var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
	console.log(formName);
	$(".right").append(data);
      
    });
  
});
```















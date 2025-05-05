---
title: Incrustación de formularios adaptables Forms/HTML5 en una página web
description: Pasos de configuración necesarios para incrustar formularios adaptables de Forms o HTML5 en una página web que no sea de AEM.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 398
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 29%

---

# Incrustación de formularios adaptables o formularios HTML5 en una página web

El formulario adaptable incrustado es completamente funcional, y los usuarios pueden rellenarlo y enviarlo sin abandonar la página. Esto permite al usuario mantenerse en el contexto de otros elementos de la página web e interactuar simultáneamente con el formulario.

En el siguiente vídeo se explican los pasos necesarios para incrustar un formulario adaptable o HTML5 en una página web.
Consulte la [documentación](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=es) para conocer los requisitos previos recomendados, las prácticas recomendadas, etc.
>[!VIDEO](https://video.tv.adobe.com/v/3418454?quality=12&learn=on&captions=spa)

Puede descargar los archivos de ejemplo utilizados en el vídeo [desde aquí](assets/embedding-af-web-page.zip)

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

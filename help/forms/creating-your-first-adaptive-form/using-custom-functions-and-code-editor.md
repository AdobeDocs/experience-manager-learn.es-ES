---
title: Uso de funciones y editor de código
seo-title: Uso de funciones y editor de código
description: Uso de funciones y editor de código para crear reglas comerciales
seo-description: Uso de funciones y editor de código para crear reglas comerciales
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: formularios adaptables
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---


# Uso de funciones personalizadas y del editor de código {#using-functions-and-code-editor}

En esta parte, utilizaremos funciones personalizadas y el editor de código para crear reglas comerciales.

ya ha instalado [ClientLib con función personalizada](assets/client-libs-and-logo.zip) anteriormente en este tutorial.

Normalmente, una biblioteca de cliente consta de un archivo CSS y Javascript. Esta biblioteca de cliente contiene un archivo javascript que expone una función para rellenar valores de lista desplegable.


## Función para rellenar la lista desplegable {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Establecer título de resumen del panel {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Validar panel {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

El siguiente es el código utilizado para validar los campos del panel

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

Puede descomentar la línea 1 para depurar el código en la ventana del explorador.

Línea 4: Obtención del panel actual

Línea 5: Valide el panel actual.

Línea 9: Si no hay errores, pase al panel siguiente

Obtenga una vista previa del formulario y pruebe la funcionalidad recién habilitada.

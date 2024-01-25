---
title: Uso de funciones y editor de código
description: Uso de funciones y editor de código para crear reglas empresariales
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
duration: 466
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Uso de funciones personalizadas y editor de código {#using-functions-and-code-editor}

En esta parte, utilizaremos funciones personalizadas y el editor de código para crear reglas empresariales.

ya ha instalado el [ClientLib con función personalizada](assets/client-libs-and-logo.zip) anteriormente en este tutorial.

Normalmente, una biblioteca de cliente consta de un archivo CSS y un archivo Javascript. Esta biblioteca de cliente contiene un archivo javascript que expone una función para rellenar valores de lista desplegable.


## Función para rellenar la lista desplegable {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### Definir título de resumen del panel {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### Validar panel {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

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

Puede quitar el comentario de la línea 1 para depurar el código en la ventana del explorador.

Línea 4: Obtención del panel actual

Línea 5: Valide el panel actual.

Línea 9: si no hay errores, vaya al siguiente panel

Obtenga una vista previa del formulario y pruebe la funcionalidad recién habilitada.

---
title: Navegar por paneles anidados
description: Navegar por paneles anidados
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 1%

---

# Pestañas de navegación con varios paneles

Cuando el formulario ha dejado fichas de navegación y una de las fichas tiene varios paneles, es posible que desee ocultar el título de los paneles secundarios y poder navegar entre las fichas y los paneles secundarios de estas fichas

## Crear formulario adaptable

Cree un formulario adaptable con la siguiente estructura. El panel raíz tiene paneles secundarios que se muestran como fichas a la izquierda. Algunos de estos &quot;**pestañas**&quot; tienen paneles secundarios adicionales. Por ejemplo, la ficha Familia tiene dos paneles secundarios llamados cónyuge e hijos.

También se agrega una barra de herramientas bajo FormContainer con los botones Anterior y Siguiente

![espaciado de la barra de herramientas](assets/multiple-panels.png)



El comportamiento predeterminado de este formulario sería mostrar todos los paneles de la izquierda y navegar de una ficha a otra al hacer clic en el botón siguiente.

Para cambiar este comportamiento predeterminado necesitamos hacer lo siguiente

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=12&learn=on)


Agregue el siguiente código al evento click de la variable **Siguiente** usando el editor de código

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Agregue el siguiente código al evento click de la variable **Anterior** usando el editor de código

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

El código anterior le ayudará a desplazarse entre las pestañas y los paneles secundarios de cada pestaña.

## Ocultar el título de los paneles secundarios

Utilice el editor de estilos para ocultar el título de los paneles secundarios de las pestañas.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=12&learn=on)

>[!NOTE]
>
>La capacidad descrita en este artículo no funciona en la última pestaña. Por ejemplo, si la ficha Dirección tenía paneles secundarios, esta funcionalidad no funcionaría.

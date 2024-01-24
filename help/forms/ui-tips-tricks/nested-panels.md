---
title: Desplazamiento por paneles anidados
description: Desplazamiento por paneles anidados
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 270
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 1%

---

# Fichas de navegación con varios paneles

Cuando el formulario tiene pestañas de navegación a la izquierda y una de las pestañas tiene varios paneles, es posible que desee ocultar el título de los paneles secundarios y seguir pudiendo desplazarse entre las pestañas y los paneles secundarios de estas pestañas

## Crear formulario adaptable

Cree un formulario adaptable con la siguiente estructura. El panel raíz tiene paneles secundarios que se muestran como pestañas a la izquierda. Algunos de estos &quot;**pestañas**&quot; tienen paneles secundarios adicionales. Por ejemplo, la pestaña Familia tiene dos paneles secundarios denominados Cónyuge e Hijos.

También se agrega una barra de herramientas debajo de FormContainer con los botones Anterior y Siguiente

![espaciado de barra de herramientas](assets/multiple-panels.png)



El comportamiento predeterminado de este formulario sería mostrar todos los paneles a la izquierda y, a continuación, desplazarse de una pestaña a otra al hacer clic en el botón siguiente.

Para cambiar este comportamiento predeterminado, debemos hacer lo siguiente

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=12&learn=on)


Agregue el siguiente código al evento de clic del **Siguiente** mediante el editor de código

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Agregue el siguiente código al evento de clic del **Anterior** mediante el editor de código

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

El código anterior le ayudará a desplazarse entre las pestañas y los paneles secundarios de cada pestaña.

## Ocultar el título de los paneles secundarios

Utilice el editor de estilos para ocultar el título de las pestañas de los paneles secundarios.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=12&learn=on)

>[!NOTE]
>
>La capacidad descrita en este artículo no funciona en la última pestaña. Por ejemplo, si la pestaña Dirección tuviera paneles secundarios, esta funcionalidad no funcionaría.

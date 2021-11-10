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
source-git-commit: 20cae7a327131927f831ae9c49fb5eebfb00f5c4
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Pestañas de navegación con varios paneles

Cuando el formulario ha dejado fichas de navegación y una de las fichas tiene varios paneles, es posible que desee ocultar el título de los paneles secundarios y poder navegar entre las fichas y los paneles secundarios de estas fichas

[Esta capacidad está activa en este formulario](https://forms.enablementadobe.com/content/forms/af/testnav1.html)




## Crear formulario adaptable

Cree un formulario adaptable con la siguiente estructura. El panel raíz tiene paneles secundarios que se muestran como fichas a la izquierda. Algunos de estos &quot;**pestañas**&quot; tienen paneles secundarios adicionales. Por ejemplo, la ficha Familia tiene dos paneles secundarios llamados cónyuge e hijos.

También se agrega una barra de herramientas bajo FormContainer con los botones Anterior y Siguiente

![espaciado de la barra de herramientas](assets/multiple-panels.png)



El comportamiento predeterminado de este formulario sería mostrar todos los paneles de la izquierda y navegar de una ficha a otra al hacer clic en el botón siguiente.

Para cambiar este comportamiento predeterminado necesitamos hacer lo siguiente

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


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

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
>
>La capacidad descrita en este artículo no funciona en la última pestaña. Por ejemplo, si la ficha Dirección tenía paneles secundarios, esta funcionalidad no funcionaría.

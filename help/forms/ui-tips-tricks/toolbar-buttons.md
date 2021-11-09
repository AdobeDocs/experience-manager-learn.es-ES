---
title: Espaciar los botones siguiente y anterior de la barra de herramientas
description: Espaciar los botones de la barra de herramientas
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
source-git-commit: 84a0c78f89f78e161b460574b5927fc4aba2fe3a
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Espaciar el botón de la barra de herramientas

Al agregar los botones Siguiente y Anterior a la barra de herramientas de AEM Forms, los botones se colocan uno junto al otro de forma predeterminada. Puede que desee pulsar el botón Siguiente al extremo derecho de la barra de herramientas y mantener el botón Anterior/Atrás a la izquierda

![espaciado de la barra de herramientas](assets/toolbar-spacing.png)


## Estilo de la barra de herramientas

El caso de uso anterior se puede lograr fácilmente utilizando el editor de estilos. Una vez agregado el botón Anterior/Siguiente a la barra de herramientas, asegúrese de seleccionar la capa Estilo en el menú de edición. Con el modo de estilo seleccionado, seleccione la barra de herramientas para abrir su hoja de propiedades de estilo. Expanda la sección Dimension y posición y asegúrese de que está viendo todas las propiedades. Establezca las siguientes propiedades
* Dimension y posición
   * Anchura: 100 %
   * Posición: relativo

Guarde los cambios

## Estilo del botón Siguiente

Seleccione el botón Next y asegúrese de abrir la hoja de propiedades de estilo del botón siguiente (no el texto del botón siguiente). Establezca las siguientes propiedades
* Dimension y posición
   * posición: absoluto Superior 1px Derecha 1px
* Borde
   * Radio del borde: 4px(arriba,derecha,abajo,izquierda)

Guarde los cambios

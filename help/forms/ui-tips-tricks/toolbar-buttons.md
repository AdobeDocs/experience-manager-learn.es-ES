---
title: Espaciar los botones siguiente y anterior de la barra de herramientas
description: Espaciar los botones de la barra de herramientas
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 59
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 3%

---

# Espaciar el botón de barra de herramientas

Cuando se agregan botones Siguiente y Anterior a la barra de herramientas de AEM Forms, los botones predeterminados se colocan uno junto al otro. Puede presionar el botón Next en el extremo derecho de la barra de herramientas, mientras mantiene el botón prev/back en la izquierda

![espaciado de barra de herramientas](assets/toolbar-spacing.png)


## Aplicar estilo a la barra de herramientas

El caso de uso anterior se puede realizar fácilmente mediante el editor de estilos. Una vez añadido el botón Anterior/Siguiente a la barra de herramientas, asegúrese de haber seleccionado la capa Estilo en el menú de edición. Con el modo de estilo seleccionado, seleccione la barra de herramientas para abrir su hoja de propiedades de estilo. Expanda la sección Dimension y posición y asegúrese de ver todas las propiedades. Establezca las siguientes propiedades
* Dimensiones y posición
   * Anchura: 100%
   * Posición: relativa

Guarde los cambios

## Aplicar estilo al botón Siguiente

Seleccione el botón Siguiente y asegúrese de abrir la hoja de propiedades de estilo del botón siguiente (no el texto del botón siguiente). Establezca las siguientes propiedades
* Dimensiones y posición
   * posición: absoluta superior 1px derecha 1px
* Borde
   * Radio de borde: 4 px (superior, derecha, inferior, izquierda)

Guarde los cambios

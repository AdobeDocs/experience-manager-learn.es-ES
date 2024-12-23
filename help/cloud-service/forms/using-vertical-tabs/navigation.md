---
title: Uso de pestañas verticales en AEM Forms as a Cloud Service
description: Crear un formulario adaptable mediante pestañas verticales
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 0%

---

# Navegación entre las pestañas

Puede desplazarse entre las pestañas haciendo clic en las pestañas individuales o utilizando los botones Anterior y Siguiente en el formulario.
Para desplazarse mediante los botones, agregue dos botones al formulario y asígneles el nombre Anterior y Siguiente. Asocie la siguiente función personalizada con el evento de clic del botón para desplazarse entre las pestañas.

A continuación se muestra la función personalizada para desplazarse entre las pestañas.



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

El siguiente es el editor de reglas para los botones Next y Previous

**Botón Siguiente**

![botón siguiente](assets/next-button.png)

**Botón Anterior**

![botón anterior](assets/prev-button.png)

---
title: Introducción al tema en Asset Share Commons
description: Materiales para la comprensión funcional y técnica de Assets Share Commons
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 1%

---

# Introducción al tema en Asset Share Commons {#asset-share-commons-theme}

Breve introducción al tema en Asset Share Commons. El vídeo recorre el proceso de creación de un nuevo tema con una combinación de colores personalizada.

>[!VIDEO](https://video.tv.adobe.com/v/20572/?quality=9&learn=on)

En este vídeo se crea un nuevo tema basado en el tema oscuro de Asset Share Commons. El esquema de colores coincidirá con un logotipo personalizado para que el sitio tenga una apariencia uniforme.

## Variables de tema

```java
/*******************************
         Theme Variables
*******************************/
 
/* Below is a condensed list of variables for easy updating of the theme */
 
/* Color Palette */
@primaryColor      : #560a63;
@secondaryColor    : #fc3096;
 
/* Text */
@fontName          : 'LatoWeb';
@fontSmoothing     : antialiased;
@headerFont        : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@pageFont          : @fontName, 'Helvetica Neue', Arial, Helvetica, sans-serif;
@textColor         : #D0D0D0;
 
 
/* Backgrounds */
@pageBackground      : #3C3C3C;
@sideBarBackground   : #363636;
 
/* Links */
@linkColor           : #FFFFFF;
@linkHoverColor      : @secondaryColor;
 
/* Buttons */
@buttonPrimaryColor      : #560a63; /* must be a value to prevent variable recursion*/
@buttonPrimaryColorHover : saturate(darken(@buttonPrimaryColor, 5), 10, relative);
 
/* Filters (Checkboxes,radio buttons, toggle, slider, dropdown, accordion colors)*/
@filterPrimaryColor      : @secondaryColor;
@filterPrimaryColorFocus : saturate(darken(@filterPrimaryColor, 5), 10, relative);
 
/* Label */
@labelPrimaryColor       : @primaryColor;
@labelPrimaryColorHover  : saturate(darken(@labelPrimaryColor, 5), 10, relative);
 
/* Menu */
@menuPrimaryColor        : @primaryColor;
 
/* Message */
@msgPositiveBackgroundColor : @secondaryColor;
@msgNegativeBackgroundColor : @red;
@msgInfoBackgroundColor: @midBlack;
@msgWarningBackgroundColor: @yellow;
```

Descargar [Tema personalizado de la biblioteca del cliente](assets/asc-theme-demo.zip)

## Recursos adicionales{#additional-resources}

* [Descargas de la versión de Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* [Descargas de la versión de ACS AEM Commons 3.11.0+](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)

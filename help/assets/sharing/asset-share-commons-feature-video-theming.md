---
title: Introducción a la temática en Asset Share Commons
description: Materiales para la comprensión funcional y técnica Assets Share Commons
version: 6.4, 6.5
topic: Content Management
feature: Asset Distribution
role: Developer
level: Intermediate
last-substantial-update: 2022-06-22T00:00:00Z
doc-type: Tutorial
exl-id: b7d0b6b1-145a-4987-a9dc-7263efa4d9fb
duration: 734
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 1%

---

# Introducción a la temática en Asset Share Commons {#asset-share-commons-theme}

Breve introducción a la temática en Asset Share Commons. El vídeo muestra el proceso de creación de una nueva temática con un esquema de colores personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/20572?quality=12&learn=on)

En este vídeo se crea un nuevo tema basado en el tema oscuro de Asset Share Commons. El esquema de colores coincidirá con un logotipo personalizado para dar al sitio un aspecto coherente.

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

Descargar [tema de biblioteca de cliente personalizado](assets/asc-theme-demo.zip)

## Recursos adicionales{#additional-resources}

* [Descargas de la versión de Asset Share Commons](https://github.com/Adobe-Marketing-Cloud/asset-share-commons/releases)
* AEM [Descargas de versiones de ACS Commons 3.11.0+](https://github.com/Adobe-Consulting-Services/acs-aem-commons/releases)

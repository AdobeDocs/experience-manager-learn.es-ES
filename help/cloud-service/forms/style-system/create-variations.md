---
title: Uso del sistema de estilos en AEM Forms
description: Defina las clases CSS para las variaciones
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 68030c25-1674-4a64-9f5f-c1a74a38e3ca
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Crear variaciones para el componente Botón

Una vez clonada la temática, abra el proyecto con el código de Visual Studio. Debería ver una vista similar
en el código de visual studio
![explorador de proyectos](assets/easel-theme.png)

Abra el archivo src->components->button->_button.scss. Definiremos nuestras variaciones personalizadas en este archivo.

## Variación corporativa

```css
.cmp-adaptiveform-button-corporate {
  @include container;
  .cmp-adaptiveform-button {
    &__widget {
      @include primary-button;
      background: $brand-red;
      text-transform: uppercase;
      border-radius: 0px;
      color: yellow;
    }
  }
}
```

## Explicación

* **cmp-adaptiveform-button-corporate**: esta es la clase de contenedor o contenedor principal para el componente &quot;cmp-adaptiveform-button-corporate&quot;.
Cualquier estilo o mezcla dentro de este bloque se aplicará a los elementos dentro de esta clase.
* **@include contenedor**: utiliza un mixin denominado contenedor, que se define en _mixins.scss. El contenedor de mezcla suele aplicar estilos relacionados con el diseño, como la configuración de márgenes, relleno u otros estilos estructurales para garantizar que el contenedor se comporta de forma coherente.
* **.cmp-adaptiveform-button**: dentro del bloque corporate-style-button, está dirigiendo el elemento secundario con la clase .cmp-adaptiveform-button.
* **&amp;__widget**: El símbolo &amp; hace referencia al selector principal, que en este caso es .cmp-adaptiveform-button.
Esto significa que la clase final de destino será .cmp-adaptiveform-button__widget, una clase de estilo BEM (Modificador de elemento de bloque) que representa un subcomponente (el elemento __widget) dentro del bloque .cmp-adaptiveform-button.
* **@include primary-button**: Esto incluye un mixin de botón principal, que se define en _mixin.scss y agrega estilos relacionados con el botón (como relleno, colores, efectos de desplazamiento, etc.). Se anulan las propiedades background, text-transform, border-radius y color definidas en el botón principal del mixin.

El archivo _mixins.scss se define en src->sitio como se muestra en la captura de pantalla siguiente

![mixin.scss](assets/mixins.png)

## Variación de marketing

```css
.cmp-adaptiveform-button--marketing {
  
  @include container;
  .cmp-adaptiveform-button {
  &__widget {
    @include primary-button;
    background-color: #3498db;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 50px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: all 0.3s ease;
    outline: none;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    &:hover:not([disabled]) {
      position: relative;
      scale: 102%;
      transition: box-shadow 0.1s ease-out, transform 0.1s ease-out;
      background-color: #2980b9;
      box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
      transform: translateY(-3px);
    }
  }
}
  
}
```

## Siguientes pasos

[Prueba de las variaciones](./build.md)

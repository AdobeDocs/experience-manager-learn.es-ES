---
title: Personalización de iconos de componente en Adobe Experience Manager Sites
description: Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaturas significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Personalización de iconos de componente {#developing-component-icons-in-aem-sites}

Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaturas significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/38291?quality=12&learn=on&captions=spa)

El navegador de componentes ahora se muestra en un tema gris coherente, que muestra lo siguiente:

* **[!UICONTROL Grupo de componentes]**
* **[!UICONTROL Título del componente]**
* **[!UICONTROL Descripción del componente]**
* **[!UICONTROL Icono de componente]**
   * Las dos primeras letras del título de componente *(predeterminado)*
   * Imagen PNG personalizada *(configurada por un desarrollador)*
   * Imagen de SVG personalizada *(configurada por un desarrollador)*
   * Icono de CoralUI *(configurado por un desarrollador)*

## Opciones de configuración del icono de componente {#component-icon-configuration-options}

### Abreviaciones {#abbreviations}

De manera predeterminada, se utilizan como abreviatura los dos primeros caracteres del título del componente (**[cq:Component]@jcr:title**). Por ejemplo, si **[cq:Component]@jcr:title=Article List**, la abreviatura se mostraría como &quot;**Ar**&quot;.

La abreviatura se puede personalizar mediante la propiedad **[cq:Component]@abbreviation**. Aunque este valor puede aceptar más de 2 caracteres, se recomienda limitar la abreviatura a 2 caracteres para evitar cualquier perturbación visual.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Iconos de CoralUI {#coralui-icons}

Los iconos de CoralUI, proporcionados por AEM, se pueden utilizar para los iconos de componente. Para configurar un icono de CoralUI, establezca una propiedad **[cq:Component]@cq:icon** en el valor de atributo icon de HTML del icono de CoralUI deseado (enumerado en la [documentación de CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imágenes PNG {#png-images}

Las imágenes PNG se pueden utilizar para los iconos de componente. Para configurar una imagen PNG como icono de componente, agregue la imagen deseada como **nt:file** con el nombre **cq:icon.png** en **[cq:Component]**.

El PNG debe tener un fondo transparente o un color de fondo establecido en **#707070**.

Las imágenes PNG se han escalado a **20px por 20px**. Sin embargo, es preferible que la retina se muestre **40px** por **40px**.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imágenes de SVG {#svg-images}

Las imágenes de SVG (basadas en vectores) se pueden utilizar para iconos de componentes. Para configurar una imagen de SVG como un icono de componente, agregue la SVG deseada como **nt:file** con el nombre **cq:icon.svg** en **[cq:Component]**.

Las imágenes de SVG deben tener un color de fondo establecido en **#707070** y un tamaño de **20 px por 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionales {#additional-resources}

* [Iconos de CoralUI disponibles](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)

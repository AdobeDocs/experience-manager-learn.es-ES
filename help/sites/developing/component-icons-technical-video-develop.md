---
title: Personalización de iconos de componentes en Adobe Experience Manager Sites
description: Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaturas significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 1%

---


# Personalización de iconos de componente {#developing-component-icons-in-aem-sites}

Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaturas significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

El navegador de componentes ahora se muestra en un tema gris coherente, mostrando lo siguiente:

* **[!UICONTROL Grupo de componentes]**
* **[!UICONTROL Título del componente]**
* **[!UICONTROL Descripción del componente]**
* **[!UICONTROL Icono de componente]**
   * Las dos primeras letras del Título del componente *(predeterminado)*
   * Imagen PNG personalizada *(configurada por un desarrollador)*
   * Imagen SVG personalizada *(configurada por un desarrollador)*
   * Icono de CoralUI *(configurado por un desarrollador)*

## Opciones de configuración de iconos de componente {#component-icon-configuration-options}

### Abreviaturas {#abbreviations}

De forma predeterminada, los dos primeros caracteres del título del componente (**[cq:Component]@jcr:title**) se utilizan como abreviatura. Por ejemplo, si **[cq:Component]@jcr:title=Lista del artículo** la abreviatura se mostraría como &quot;**Ar**&quot;.

La abreviatura se puede personalizar mediante la propiedad **[cq:Component]@abbreviation**. Aunque este valor puede aceptar más de 2 caracteres, se recomienda limitar la abreviatura a 2 caracteres para evitar cualquier perturbación visual.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Iconos de CoralUI {#coralui-icons}

Los iconos de CoralUI, proporcionados por AEM, se pueden utilizar para los iconos de componentes. Para configurar un icono de CoralUI, establezca una propiedad **[cq:Component]@cq:icon** en el valor de atributo del icono HTML del icono de CoralUI deseado (enumerado en la [documentación de CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imágenes PNG {#png-images}

Las imágenes PNG se pueden utilizar para los iconos de componentes. Para configurar una imagen PNG como un icono de componente, agregue la imagen deseada como **nt:file** con el nombre **cq:icon.png** en **[cq:Component]**.

El PNG debe tener un fondo transparente o un color de fondo definido como **#707070**.

Las imágenes PNG se escalarán a **20 px por 20 px**. Sin embargo, para dar cabida a pantallas de retina **40px** por **40px** puede ser preferible.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imágenes SVG {#svg-images}

Las imágenes SVG (basadas en vectores) se pueden utilizar para los iconos de los componentes. Para configurar una imagen SVG como un icono de componente, agregue el SVG deseado como **nt:file** con el nombre **cq:icon.svg** en **[cq:Component]**.

Las imágenes SVG deben tener un color de fondo establecido en **#707070** y un tamaño de **20 px por 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionales {#additional-resources}

* [Iconos CoralUI disponibles](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)

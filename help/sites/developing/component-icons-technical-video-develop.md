---
title: Personalización de iconos de componente en Adobe Experience Manager Sites
description: Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaturas significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Personalización de iconos de componente {#developing-component-icons-in-aem-sites}

Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaturas significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

El navegador de componentes ahora se muestra en un tema gris coherente, que muestra lo siguiente:

* **[!UICONTROL Grupo de componentes]**
* **[!UICONTROL Título del componente]**
* **[!UICONTROL Descripción del componente]**
* **[!UICONTROL Icono de componente]**
   * Las dos primeras letras del título del componente *(predeterminado)*
   * Imagen PNG personalizada *(configurado por un desarrollador)*
   * Imagen de SVG personalizada *(configurado por un desarrollador)*
   * Icono de CoralUI *(configurado por un desarrollador)*

## Opciones de configuración del icono de componente {#component-icon-configuration-options}

### Abreviaciones {#abbreviations}

De forma predeterminada, los dos primeros caracteres del título del componente (**[cq:Component]@jcr:título**) se utilizan como abreviatura. Por ejemplo, si **[cq:Component]@jcr:title=Lista de artículos** la abreviatura se mostraría como &quot;**Ar**&quot;.

La abreviatura se puede personalizar mediante la variable **[cq:Component]@abbreviation** propiedad. Aunque este valor puede aceptar más de 2 caracteres, se recomienda limitar la abreviatura a 2 caracteres para evitar cualquier perturbación visual.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Iconos de CoralUI {#coralui-icons}

AEM Los iconos de CoralUI, proporcionados por el usuario de la interfaz de usuario de, se pueden utilizar para iconos de componente. Para configurar un icono de CoralUI, establezca un **[cq:Component]@cq:icono** HTML al valor de atributo icon del icono de CoralUI deseado (enumerado en la [Documentación de CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imágenes PNG {#png-images}

Las imágenes PNG se pueden utilizar para los iconos de componente. Para configurar una imagen PNG como icono de componente, añada la imagen deseada como **nt:archivo** nombrado **cq:icon.png** en el **[cq:Component]**.

El PNG debe tener un fondo transparente o un color de fondo establecido como **#707070**.

Las imágenes PNG se escalan a **20 px por 20 px**. Sin embargo, para acomodar las pantallas de retina **40 px** por **40 px** podría ser preferible.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imágenes de SVG {#svg-images}

Las imágenes de SVG (basadas en vectores) se pueden utilizar para los iconos de componente. Para configurar una imagen de SVG como icono de componente, añada el SVG deseado como **nt:archivo** nombrado **cq:icon.svg** en el **[cq:Component]**.

Las imágenes de SVG deben tener un color de fondo establecido en **#707070** y un tamaño de **20 px por 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionales {#additional-resources}

* [Iconos de CoralUI disponibles](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)

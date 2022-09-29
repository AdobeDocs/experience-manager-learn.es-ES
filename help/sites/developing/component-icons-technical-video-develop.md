---
title: Personalización de iconos de componente en Adobe Experience Manager Sites
description: Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaciones significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# Personalización de iconos de componente {#developing-component-icons-in-aem-sites}

Los iconos de componente permiten a los autores identificar rápidamente un componente con iconos o abreviaciones significativas. Los autores ahora pueden encontrar los componentes necesarios para crear sus experiencias web más rápido que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

El navegador de componentes ahora se muestra en un tema gris coherente, mostrando lo siguiente:

* **[!UICONTROL Grupo de componentes]**
* **[!UICONTROL Título de componente]**
* **[!UICONTROL Descripción del componente]**
* **[!UICONTROL Icono de componente]**
   * Las dos primeras letras del título del componente *(predeterminado)*
   * Imagen PNG personalizada *(configurado por un desarrollador)*
   * Imagen de SVG personalizada *(configurado por un desarrollador)*
   * Icono de CoralUI *(configurado por un desarrollador)*

## Opciones de configuración de iconos de componente {#component-icon-configuration-options}

### Abreviaturas {#abbreviations}

De forma predeterminada, los dos primeros caracteres del título del componente (**[cq:Component]@jcr:title**) se utilizan como abreviatura. Por ejemplo, si **[cq:Component]@jcr:title=Lista de artículos** la abreviatura se mostraría como &quot;**Ar**&quot;.

La abreviatura se puede personalizar mediante la variable **[cq:Component]@abreviation** propiedad. Aunque este valor puede aceptar más de 2 caracteres, se recomienda limitar la abreviación a 2 caracteres para evitar cualquier perturbación visual.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Iconos de CoralUI {#coralui-icons}

Los iconos de CoralUI, proporcionados por AEM, se pueden utilizar para los iconos de los componentes. Para configurar un icono de CoralUI , establezca un **[cq:Component]@cq:icon** a la propiedad del valor de atributo del icono de HTML del icono de CoralUI deseado (enumerado en la variable [Documentación de CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imágenes PNG {#png-images}

Las imágenes PNG se pueden utilizar para los iconos de componentes. Para configurar una imagen PNG como icono de componente, añada la imagen deseada como **nt:file** named **cq:icon.png** en el **[cq:Component]**.

El PNG debe tener un fondo transparente o un color de fondo definido como **#707070**.

Las imágenes PNG se escalan a **20 px por 20 px**. Sin embargo, se pueden incluir pantallas de retina **40px** por **40px** puede ser preferible.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imágenes de SVG {#svg-images}

Las imágenes SVG (basadas en vectores) se pueden utilizar para los iconos de los componentes. Para configurar una imagen de SVG como un icono de componente, añada el SVG deseado como un **nt:file** named **cq:icon.svg** en el **[cq:Component]**.

Las imágenes SVG deben tener un color de fondo definido como **#707070** y un tamaño de **20 px por 20 px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionales {#additional-resources}

* [Iconos CoralUI disponibles](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)

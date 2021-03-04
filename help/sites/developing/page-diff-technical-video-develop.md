---
title: Desarrollo de la diferencia de páginas en AEM Sites
description: Este vídeo muestra cómo proporcionar estilos personalizados para la funcionalidad de diferencia de página de AEM Sites.
feature: 'Creación  '
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 3%

---


# Desarrollo de la diferencia de página {#developing-for-page-difference}

Este vídeo muestra cómo proporcionar estilos personalizados para la funcionalidad de diferencia de página de AEM Sites.

## Personalización de estilos de diferencia de página {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>Este vídeo agrega CSS personalizada a la biblioteca de cliente de we.Retail, donde como estos cambios deben realizarse en el proyecto de AEM Sites del cliente; en el código de ejemplo siguiente: `my-project`.

La diferencia de página de AEM obtiene el OOTB CSS a través de una carga directa de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Debido a esta carga directa de CSS en lugar de usar una categoría de biblioteca de cliente, debemos encontrar otro punto de inyección para los estilos personalizados, y este punto de inyección personalizado es la clientlib de creación del proyecto.

Esto tiene la ventaja de permitir que estas anulaciones de estilo personalizadas sean específicas del inquilino.

### Preparar la clientlib {#prepare-the-authoring-clientlib} de creación

Asegúrese de que existe una `authoring` clientlib para su proyecto en `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Proporcione el CSS personalizado {#provide-the-custom-css}

Agregue a la clientlib `authoring` del proyecto un `css.txt` que apunte al archivo menor que proporcionará los estilos principales. [](https://lesscss.org/) Lessis es preferible debido a sus muchas funciones prácticas, incluido el ajuste de clases que se aprovecha en este ejemplo.

```shell
base=./css

htmldiff.less
```

Cree el archivo `less` que contiene las anulaciones de estilo en `/apps/my-project/clientlibs/authoring/css/htmldiff.less` y proporcione los estilos de anulación que sean necesarios.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### Incluya el CSS clientlib de creación a través del componente de página {#include-the-authoring-clientlib-css-via-the-page-component}

Incluya la categoría clientlibs de creación en la `/apps/my-project/components/structure/page/customheaderlibs.html` página base del proyecto directamente antes de la etiqueta `</head>` para asegurarse de que se cargan los estilos.

Estos estilos deben limitarse a los modos [!UICONTROL Edit] y [!UICONTROL preview] WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

El resultado final de una página de comparación de diferencias con los estilos aplicados anteriores tendría este aspecto (HTML añadido y componente cambiado).

![Diferencia de página](assets/page-diff.png)

## Recursos adicionales {#additional-resources}

* [Descargue el sitio de muestra de We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Uso de bibliotecas de cliente de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Menos documentación de CSS](https://lesscss.org/)

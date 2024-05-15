---
title: Desarrollo para la diferencia de página en AEM Sites
description: Este vídeo muestra cómo proporcionar estilos personalizados para la funcionalidad Diferencia de páginas de AEM Sites.
feature: Authoring
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
duration: 281
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# Desarrollo para la diferencia de página {#developing-for-page-difference}

Este vídeo muestra cómo proporcionar estilos personalizados para la funcionalidad Diferencia de páginas de AEM Sites.

## Personalizar estilos de diferencia de página {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>Este vídeo agrega CSS personalizado a la biblioteca de cliente de we.Retail, donde como estos cambios deben realizarse en el proyecto AEM Sites del personalizador; en el siguiente código de ejemplo: `my-project`.

AEM diferencia de página obtiene el CSS de OOTB mediante una carga directa de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Debido a esta carga directa de CSS en lugar de utilizar una categoría de biblioteca de cliente, debemos encontrar otro punto de inyección para los estilos personalizados y este punto de inyección personalizado es la clientlib de creación del proyecto.

Esto tiene la ventaja de permitir que estas invalidaciones de estilo personalizadas sean específicas del inquilino.

### Preparar la clientlib de creación {#prepare-the-authoring-clientlib}

Garantizar la existencia de un `authoring` clientlib para su proyecto en `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Proporcionar el CSS personalizado {#provide-the-custom-css}

Añadir a la del proyecto `authoring` clientlib a `css.txt` que apunta al archivo Less que proporcionará los estilos de anulación. [Menos](https://lesscss.org/) se prefiere debido a sus muchas características convenientes, incluyendo el ajuste de clase que se aprovecha en este ejemplo.

```shell
base=./css

htmldiff.less
```

Cree el `less` que contiene las anulaciones de estilo en `/apps/my-project/clientlibs/authoring/css/htmldiff.less`y proporcione los estilos generales según sea necesario.

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

### Incluir el CSS clientlib de creación a través del componente de página {#include-the-authoring-clientlib-css-via-the-page-component}

Incluya la categoría clientlibs de creación en la página base del proyecto `/apps/my-project/components/structure/page/customheaderlibs.html` directamente antes de `</head>` para asegurarse de que se cargan los estilos.

Estos estilos deben limitarse a [!UICONTROL Editar] y [!UICONTROL previsualización] Modos WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

El resultado final de una página de comparación con los estilos anteriores aplicados tendría este aspecto (HTML añadido y Componente cambiado).

![Diferencia de página](assets/page-diff.png)

## Recursos adicionales {#additional-resources}

* [Descargue el sitio de muestra de we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [AEM Uso de bibliotecas de cliente](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Menos documentación de CSS](https://lesscss.org/)

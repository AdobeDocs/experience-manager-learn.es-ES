---
title: Desarrollo de la diferencia de página en AEM Sites
description: Este vídeo muestra cómo proporcionar estilos personalizados para la funcionalidad Diferencia de páginas de AEM Sites.
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 2%

---


# Desarrollo de la diferencia de página {#developing-for-page-difference}

Este vídeo muestra cómo proporcionar estilos personalizados para la funcionalidad Diferencia de páginas de AEM Sites.

## Personalización de estilos de diferencia de página {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>Este vídeo agrega CSS personalizada a la biblioteca de cliente we.Retail, donde estos cambios deben realizarse en el proyecto de AEM Sites del cliente; en el código de ejemplo siguiente: `my-project`.

AEM diferencia de página obtiene el CSS OOTB mediante una carga directa de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Debido a esta carga directa de CSS en lugar de utilizar una categoría de biblioteca de cliente, debemos encontrar otro punto de inyección para los estilos personalizados, y este punto de inyección personalizado es la clientlib de creación del proyecto.

Esto tiene la ventaja de permitir que estas anulaciones de estilo personalizado sean específicas del inquilino.

### Preparar la clientlib {#prepare-the-authoring-clientlib} de creación

Asegúrese de que exista una clientlib `authoring` para su proyecto en `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Proporcione la CSS personalizada {#provide-the-custom-css}

Añada a la clientlib `authoring` del proyecto un `css.txt` que apunta al archivo menos que proporcionará los estilos de reemplazo. [Las ](https://lesscss.org/) Lessis son preferibles debido a sus muchas características prácticas, incluido el ajuste de clases, que se aprovecha en este ejemplo.

```shell
base=./css

htmldiff.less
```

Cree el archivo `less` que contiene las anulaciones de estilo en `/apps/my-project/clientlibs/authoring/css/htmldiff.less` y proporcione los estilos de anulación según sea necesario.

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

### Incluya el CSS clientlib de creación mediante el componente de página {#include-the-authoring-clientlib-css-via-the-page-component}

Incluya la categoría clientlibs de creación en la etiqueta `/apps/my-project/components/structure/page/customheaderlibs.html` de la página base del proyecto directamente antes de la etiqueta `</head>` para asegurarse de que se cargan los estilos.

Estos estilos deben limitarse a los modos [!UICONTROL Editar] y [!UICONTROL previsualización] WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

El resultado final de una página de diferencias con los estilos anteriores aplicados sería el siguiente (HTML agregado y Componente modificado).

![Diferencia de página](assets/page-diff.png)

## Recursos adicionales {#additional-resources}

* [Descargar el sitio de muestra de We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Uso de AEM bibliotecas de cliente](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Menos documentación de CSS](https://lesscss.org/)

---
title: Depuración de una implementación de etiquetas
description: Introducción a algunas herramientas y técnicas comunes para depurar una implementación de etiquetas. Obtenga información sobre cómo utilizar la consola para desarrolladores del explorador y la extensión de Experience Platform Debugger para identificar y solucionar problemas relacionados con aspectos clave de una implementación de etiquetas.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# Depuración de una implementación de etiquetas {#debug-tags-implementation}

Introducción a las herramientas y técnicas comunes utilizadas para depurar una implementación de etiquetas. Obtenga información sobre cómo utilizar la consola para desarrolladores del explorador y la extensión de Experience Platform Debugger para identificar y solucionar problemas relacionados con aspectos clave de una implementación de etiquetas.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Depuración del lado del cliente mediante un objeto Satellite

La depuración del lado del cliente resulta útil para comprobar la carga de la regla de propiedad de etiquetas o el orden de ejecución. Cada vez que se añade una propiedad Tag al sitio web, la variable `_satellite` El objeto JavaScript está presente en el explorador para facilitar el seguimiento de datos y eventos del lado del cliente.

Para habilitar la depuración del lado del cliente, llame al método `setDebug(true)` en el `_satellite` objeto.

1. Abra la consola del explorador y ejecute el siguiente comando.

   ```javascript
       _satellite.setDebug(true);
   ```

1. AEM Vuelva a cargar la página del sitio de la y compruebe que el registro de la consola muestra _regla activada_ mensaje como el siguiente.

   ![Propiedad Tag en las páginas de creación y publicación](assets/satellite-object-debugging.png)

## Depuración mediante Adobe Experience Platform Debugger

El Adobe proporciona Adobe Experience Platform Debugger [Extensión de Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) y [Complemento de Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) para depurar, comprender y obtener información sobre la integración.

1. Abra la extensión de Adobe Experience Platform Debugger y abra la página del sitio en la instancia Publicar

1. En el **Adobe Experience Platform Debugger > Resumen > Etiquetas de Adobe Experience Platform** , compruebe los detalles de la propiedad de etiquetas, como Nombre, Versión, Fecha de compilación, Entorno y Extensiones.

   ![Detalles de propiedades de etiquetas y Adobes Experience Platform Debugger](assets/tag-property-details.png)

## Recursos adicionales {#additional-resources}

+ [Introducción al Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Referencia de objeto satélite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)

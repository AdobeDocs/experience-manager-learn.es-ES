---
title: Depuración de una implementación de etiquetas
description: Introducción a algunas herramientas y técnicas comunes para depurar una implementación de etiquetas. Aprenda a utilizar la consola de desarrollador del explorador y la extensión de Debugger de Experience Platform para identificar y solucionar problemas relacionados con aspectos clave de una implementación de etiquetas.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Depuración de una implementación de etiquetas {#debug-tags-implementation}

Introducción a las herramientas y técnicas comunes utilizadas para depurar una implementación de etiquetas. Aprenda a utilizar la consola de desarrollador del explorador y la extensión de Debugger de Experience Platform para identificar y solucionar problemas relacionados con aspectos clave de una implementación de etiquetas.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Depuración del lado del cliente mediante el objeto Satellite

La depuración del lado del cliente es útil para verificar la carga de reglas de la propiedad Tag o el orden de ejecución. Cada vez que se agrega una propiedad Tag al sitio web, la variable `_satellite` El objeto JavaScript está presente en el explorador para facilitar el seguimiento de datos y eventos del lado del cliente.

Para habilitar la depuración del lado del cliente, llame a la función `setDebug(true)` en el `_satellite` objeto.

1. Abra la consola del explorador y ejecute el comando siguiente.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Vuelva a cargar la página del sitio AEM y verifique los registros de la consola que se muestran _regla activada_ como a continuación.

   ![Etiqueta de propiedad en páginas de creación y publicación](assets/satellite-object-debugging.png)

## Depuración mediante Adobe Experience Platform Debugger

Adobe proporciona Adobe Experience Platform Debugger [Extensión de Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) y [Complemento de Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) para depurar, comprender y obtener información sobre la integración.

1. Abra la extensión de Adobe Experience Platform Debugger y abra la página del sitio en la instancia de publicación.

1. En el **Adobe Experience Platform Debugger > Resumen > Etiquetas de Adobe Experience Platform** , compruebe los detalles de la propiedad Tag como Nombre, Versión, Fecha de compilación, Entorno y Extensiones.

   ![Detalles de las propiedades de etiqueta y Adobe Experience Platform Debugger](assets/tag-property-details.png)

## Recursos adicionales {#additional-resources}

+ [Introducción a Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Referencia de objeto satélite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)

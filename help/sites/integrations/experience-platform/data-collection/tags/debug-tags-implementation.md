---
title: Depuración de una implementación de etiquetas
description: Introducción a algunas herramientas y técnicas comunes para depurar una implementación de etiquetas. Obtenga información sobre cómo utilizar la consola para desarrolladores del explorador y la extensión de Experience Platform Debugger para identificar y solucionar problemas relacionados con aspectos clave de una implementación de etiquetas.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 259
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# Depuración de una implementación de etiquetas {#debug-tags-implementation}

Introducción a las herramientas y técnicas comunes utilizadas para depurar una implementación de etiquetas. Obtenga información sobre cómo utilizar la consola para desarrolladores del explorador y la extensión de Experience Platform Debugger para identificar y solucionar problemas relacionados con aspectos clave de una implementación de etiquetas.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Depuración del lado del cliente mediante un objeto Satellite

La depuración del lado del cliente resulta útil para comprobar la carga de la regla de propiedad de etiquetas o el orden de ejecución. Cada vez que se agrega una propiedad Tag al sitio web, el objeto JavaScript `_satellite` está presente en el explorador para facilitar el seguimiento de datos y eventos del lado del cliente.

Para habilitar la depuración del lado del cliente, llame al método `setDebug(true)` en el objeto `_satellite`.

1. Abra la consola del explorador y ejecute el siguiente comando.

   ```javascript
       _satellite.setDebug(true);
   ```

1. AEM Vuelva a cargar la página del sitio de la y compruebe que el registro de la consola muestra el mensaje _regla activada_ como se muestra a continuación.

   ![Propiedad de etiquetas en páginas de autor y Publish](assets/satellite-object-debugging.png)

## Depuración mediante Adobe Experience Platform Debugger

El Adobe proporciona la extensión de Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) para depurar, comprender y obtener información sobre la integración.

1. Abra la extensión de Adobe Experience Platform Debugger y abra la página del sitio en la instancia de Publish

2. En la sección **Adobe Experience Platform Debugger > Resumen > Etiquetas de Adobe Experience Platform**, compruebe los detalles de la propiedad de etiquetas, como Nombre, Versión, Fecha de compilación, Entorno y Extensiones.

   ![Detalles de propiedades de etiquetas y Adobes Experience Platform Debugger](assets/tag-property-details.png)

## Recursos adicionales {#additional-resources}

+ [Introducción al Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Referencia de objeto satélite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)

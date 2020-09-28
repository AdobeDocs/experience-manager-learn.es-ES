---
title: Uso de visores de medios dinámicos con Adobe Analytics y Adobe Launch
seo-title: Uso de visores de medios dinámicos con Adobe Analytics y Adobe Launch
description: La extensión de visores de Dynamic Media para Adobe Launch, junto con la versión 5.13 de visores de Dynamic Media, permite a los clientes de Dynamic Media, Adobe Analytics y Adobe Launch utilizar eventos y datos específicos para los visores de Dynamic Media en su configuración de Adobe Launch.
seo-description: 'La extensión de visores de Dynamic Media para Adobe Launch, junto con la versión 5.13 de visores de Dynamic Media, permite a los clientes de Dynamic Media, Adobe Analytics y Adobe Launch utilizar eventos y datos específicos para los visores de Dynamic Media en su configuración de Adobe Launch. '
sub-product: Dynamic-media, análisis
feature: asset-insights, media-player
topics: images, videos, renditions, authoring, integrations, publishing
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 23%

---


# Using Dynamic Media Viewers with Adobe Analytics and Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Para los clientes con Dynamic Media y Adobe Analytics, ahora puede realizar un seguimiento del uso de los visores de Dynamic Media en su sitio web mediante Dynamic Media Viewer Extension.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Ejecute Adobe Experience Manager en el modo Scene7 de Dynamic Media para esta funcionalidad. También debe [integrar Adobe Experience Platform Launch con su instancia](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)de AEM.

Con la introducción de la extensión del visor de medios dinámicos, Adobe Experience Manager ahora oferta la compatibilidad de análisis avanzados con los recursos suministrados con los visores de medios dinámicos (5.13), lo que proporciona un control más granular del seguimiento de eventos cuando se utiliza un visor de medios dinámicos en una página de sitios.

Si ya tiene AEM Assets y Sitios, puede integrar la propiedad Launch con la instancia de creación de AEM. Una vez que la integración de inicio esté asociada con el sitio web, puede agregar componentes de medios dinámicos a la página con el seguimiento de eventos para los visores habilitados.

Para los clientes solo de AEM Assets o de Dynamic Media Classic, el usuario puede obtener el código incrustado de un visor y agregarlo a la página. Iniciar Las bibliotecas de secuencias de comandos se pueden agregar manualmente a la página para el seguimiento de eventos del visor.

La tabla siguiente lista eventos del visor de Dynamic Media y sus argumentos admitidos:

<table>
   <tbody>
      <tr>
         <td>Nombre del evento del visor</td>
         <td>Referencia de argumento</td>
      </tr>
      <tr>
         <td> FRECUENTES </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> PANCARTA <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> ELEMENTO </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> CARGAR </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.compañía% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> METADATOS </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> MILESTONE </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> PÁGINA </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> PAUSE </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> PLAY </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> GIRO </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> DETENER </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> MUESTRA </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> ZOOM </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## Recursos adicionales{#additional-resources}

* [Integración de Adobe Experience Manager con Adobe Launch](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Ejecución de Adobe Experience Manager en el modo Scene7 de Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integración de visualizadores de Dynamic Media con Adobe Analytics y Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)

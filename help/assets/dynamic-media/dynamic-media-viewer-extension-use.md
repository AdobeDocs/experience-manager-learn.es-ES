---
title: Uso de visualizadores de Dynamic Media con Adobe Analytics y Adobe Launch
description: La extensión de visores de Dynamic Media para Adobe Launch, junto con la versión 5.13 de visores de Dynamic Media, permite a los clientes de Dynamic Media, Adobe Analytics y Adobe Launch utilizar eventos y datos específicos para los visores de Dynamic Media en su configuración de Adobe Launch.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 17%

---


# Uso de visualizadores de Dynamic Media con Adobe Analytics y Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Para los clientes con Dynamic Media y Adobe Analytics, ahora puede realizar un seguimiento del uso de los visualizadores de Dynamic Media en su sitio web mediante la extensión del visualizador de Dynamic Media.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Ejecute Adobe Experience Manager en el modo Dynamic Media Scene7 para esta funcionalidad. También debe [integrar Adobe Experience Platform Launch con su instancia de AEM](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html).

Con la introducción de la extensión del visualizador de Dynamic Media, Adobe Experience Manager ahora ofrece compatibilidad avanzada de análisis para los recursos entregados con los visualizadores de Dynamic Media (5.13), lo que proporciona un control más granular sobre el seguimiento de eventos cuando se utiliza un visualizador de Dynamic Media en una página de Sites.

Si ya tiene AEM Assets y Sites, puede integrar la propiedad de Launch con la instancia de autor de AEM. Una vez que la integración de launch esté asociada con el sitio web, puede añadir componentes de Dynamic Media a la página con el seguimiento de eventos para los visualizadores habilitado.

Para los clientes solo de AEM Assets o los clientes de Dynamic Media Classic, el usuario puede obtener el código incrustado de un visor y añadirlo a la página. Las bibliotecas de script de Launch se pueden agregar manualmente a la página para el seguimiento de eventos del visor.

En la tabla siguiente se enumeran los eventos del visualizador de Dynamic Media y sus argumentos admitidos:

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
         <td> BANNER <br></td>
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
         <td> CARGA </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
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
         <td> PAUSA </td>
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
         <td> STOP </td>
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
* [Ejecución de Adobe Experience Manager en el modo Dynamic Media Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Integración de visualizadores de Dynamic Media con Adobe Analytics y Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)

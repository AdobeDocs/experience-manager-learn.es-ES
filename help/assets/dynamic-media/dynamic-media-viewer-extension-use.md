---
title: Uso de visores de Dynamic Media con Adobe Analytics y etiquetas
description: La extensión de visores de Dynamic Media para etiquetas, junto con la versión de visores de Dynamic Media 5.13, permite a los clientes de Dynamic Media, Adobe Analytics y las etiquetas utilizar eventos y datos específicos para los visores de Dynamic Media en su configuración de etiquetas.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 576
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 2%

---

# Uso de visores de Dynamic Media con Adobe Analytics y etiquetas{#using-dynamic-media-viewers-adobe-analytics-tags}

Para los clientes con Dynamic Media y Adobe Analytics, ahora puede realizar un seguimiento del uso de los visores de Dynamic Media en su sitio web mediante la extensión de visor de Dynamic Media.

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> Ejecute Adobe Experience Manager en el modo Dynamic Media Scene7 para esta funcionalidad. AEM También necesita [integrar etiquetas con su instancia de](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=es).

Con la introducción de la extensión del visualizador de Dynamic Media, Adobe Experience Manager ahora ofrece compatibilidad con análisis avanzados para los recursos entregados con visualizadores de Dynamic Media (5.13), lo que proporciona un control más granular sobre el seguimiento de eventos cuando se utiliza un visualizador de Dynamic Media en una página de Sites.

Si ya tiene AEM Assets AEM y Sites, puede integrar su propiedad de etiquetas con la instancia de autor de la. Una vez que la integración de Launch esté asociada al sitio web, puede añadir componentes de medios dinámicos a la página con el seguimiento de eventos para los visualizadores habilitado.

Para los clientes solo de AEM Assets o los clientes de Dynamic Media Classic, el usuario puede obtener código incrustado para un visor y agregarlo a la página. Las bibliotecas de scripts de etiquetas se pueden añadir manualmente a la página para el seguimiento de eventos del visor.

En la tabla siguiente se enumeran los eventos de visualizador de Dynamic Media y sus argumentos admitidos:

<table>
   <tbody>
      <tr>
         <td>Nombre del evento del visor</td>
         <td>Referencia de argumento</td>
      </tr>
      <tr>
         <td> COMÚN </td>
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
         <td> TITULAR <br></td>
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
         <td> %event.detail.dm.LOAD.applicationName% </td>
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
         <td> HITO </td>
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
         <td> REPRODUCIR </td>
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
         <td> INTERCAMBIAR </td>
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

* AEM [Integración de etiquetas con el en Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=es)
* [Ejecutando Adobe Experience Manager en el modo Dynamic Media Scene7](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [Integración de visores de Dynamic Media con Adobe Analytics mediante etiquetas](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)

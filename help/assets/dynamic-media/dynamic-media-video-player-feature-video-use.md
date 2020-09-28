---
title: Uso del reproductor de vídeo en AEM Dynamic Media
seo-title: Uso del reproductor de vídeo en AEM Dynamic Media
description: AEM reproductor de vídeo de Dynamic Media solía depender del tiempo de ejecución de Flash para admitir el flujo continuo de vídeo adaptable en los clientes de escritorio y los navegadores se volvían más dinámicos en el flujo de contenido basado en flash. Con la introducción de HLS (el protocolo de envío de vídeo HTTP Live Streaming de Apple), el contenido ahora se puede transmitir sin necesidad de utilizar flash.
seo-description: AEM reproductor de vídeo de Dynamic Media solía depender del tiempo de ejecución de Flash para admitir el flujo continuo de vídeo adaptable en los clientes de escritorio y los navegadores se volvían más dinámicos en el flujo de contenido basado en flash. Con la introducción de HLS (el protocolo de envío de vídeo HTTP Live Streaming de Apple), el contenido ahora se puede transmitir sin necesidad de utilizar flash.
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: Dynamic-media
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 5%

---


# Uso del reproductor de vídeo en AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM reproductor de vídeo de Dynamic Media solía depender del tiempo de ejecución de Flash para admitir el flujo continuo de vídeo adaptable en los clientes de escritorio y los navegadores se volvían más dinámicos en el flujo de contenido basado en flash. Con la introducción de HLS (el protocolo de envío de vídeo HTTP Live Streaming de Apple), el contenido ahora se puede transmitir sin necesidad de utilizar flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Búsqueda rápida en el reproductor de vídeo no Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

La compatibilidad con el navegador HLS es la siguiente: para los exploradores no admitidos, se realiza una alternativa al envío de vídeo progresivo

<table> 
 <thead> 
  <tr> 
   <th> <p>Dispositivo</p> </th>
   <th> <p>Explorador</p> </th>
   <th > <p>Modo de reproducción de vídeo</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Internet Explorer 9 y 10</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr>
   <td> <p>Escritorio</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
  <tr>
   <td> <p>Escritorio</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Firefox 45 o posterior</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Chrome (Android 6 o anterior)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Chrome (Android 7 o posterior)</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Android (navegador predeterminado)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>BlackBerry</p> </td>
   <td> <p>Flujo continuo de vídeo HLS</p> </td>
  </tr>
 </tbody>
</table>
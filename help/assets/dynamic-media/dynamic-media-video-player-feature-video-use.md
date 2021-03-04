---
title: Uso del reproductor de vídeo en AEM Dynamic Media
description: El reproductor de vídeo de AEM Dynamic Media solía confiar en el tiempo de ejecución de Flash para admitir la transmisión de vídeo adaptable en clientes de escritorio y los navegadores se volvieron más agresivos con la transmisión de contenido basada en Flash. Con la introducción de HLS (el protocolo de entrega de vídeo de transmisión en directo HTTP de Apple), el contenido ahora se puede transmitir sin depender del flash.
sub-product: dynamic-media
feature: Perfiles de vídeo
version: 6.3, 6.4, 6.5
topic: Administración de contenido
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 8%

---


# Uso del reproductor de vídeo en AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

El reproductor de vídeo de AEM Dynamic Media solía confiar en el tiempo de ejecución de Flash para admitir la transmisión de vídeo adaptable en clientes de escritorio y los navegadores se volvieron más agresivos con la transmisión de contenido basada en Flash. Con la introducción de HLS (el protocolo de entrega de vídeo de transmisión en directo HTTP de Apple), el contenido ahora se puede transmitir sin depender del flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Vista rápida en el reproductor de vídeo no Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

La compatibilidad con navegadores HLS es la siguiente: para exploradores no compatibles, volvemos a la entrega de vídeo progresivo

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
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr>
   <td> <p>Escritorio</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Firefox 45 o posterior</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Chrome (Android 6 o anterior)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Chrome (Android 7 o posterior)</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Android (explorador predeterminado)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Móvil</p> </td>
   <td> <p>BlackBerry</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
 </tbody>
</table>
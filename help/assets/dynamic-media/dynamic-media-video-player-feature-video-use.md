---
title: Uso del reproductor de vídeo en AEM Dynamic Media
description: AEM reproductor de vídeo de Dynamic Media solía confiar en el tiempo de ejecución del Flash para admitir el flujo de vídeo adaptable en los clientes de escritorio y los navegadores se volvieron más agresivos con el flujo de contenido basado en flash. Con la introducción de HLS (protocolo de entrega de vídeo de transmisión en directo HTTP de Apple), el contenido ahora se puede transmitir sin depender del flash.
sub-product: dynamic-media
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 6%

---


# Uso del reproductor de vídeo en AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM reproductor de vídeo de Dynamic Media solía confiar en el tiempo de ejecución del Flash para admitir el flujo de vídeo adaptable en los clientes de escritorio y los navegadores se volvieron más agresivos con el flujo de contenido basado en flash. Con la introducción de HLS (protocolo de entrega de vídeo de transmisión en directo HTTP de Apple), el contenido ahora se puede transmitir sin depender del flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Quick look to Non Flash Video Player {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

La compatibilidad con navegadores HLS es la siguiente: para exploradores no compatibles, volvemos a la entrega de vídeo progresivo

>[!NOTE]
>
> Dynamic Media Hybrid NO es compatible con la transmisión de vídeo en Internet Explorer 11 a partir del 15 de marzo de 2022. Actualice a la versión 6.5.12 o superior para volver a la reproducción progresiva en IE 11.

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
   <td> <p>Dynamic Media: modo Scene7: Flujo de vídeo HLS</p> 
        <p>Dynamic Media: modo híbrido: Descarga progresiva</p>
   </td>
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

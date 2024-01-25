---
title: AEM Uso del reproductor de vídeo en Dynamic Media
description: AEM El reproductor de vídeo de Dynamic Media, que antes dependía del tiempo de ejecución del Flash para admitir el flujo de vídeo adaptable en los clientes y exploradores de equipos de escritorio, se volvió más agresivo con el flujo de contenido basado en flash. Con la introducción de HLS (HTTP Live Streaming video delivery protocol de Apple), el contenido ahora se puede transmitir sin depender del flash.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 608
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# AEM Uso del reproductor de vídeo en Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM El reproductor de vídeo de Dynamic Media, que antes dependía del tiempo de ejecución del Flash para admitir el flujo de vídeo adaptable en los clientes y exploradores de equipos de escritorio, se volvió más agresivo con el flujo de contenido basado en flash. Con la introducción de HLS (HTTP Live Streaming video delivery protocol de Apple), el contenido ahora se puede transmitir sin depender del flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## Búsqueda rápida en el reproductor de vídeo sin Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

La compatibilidad con el explorador HLS es la siguiente: para los exploradores no compatibles, volvemos a la entrega de vídeo progresivo

>[!NOTE]
>
> Dynamic Media Hybrid NO admite la transmisión de vídeo en Internet Explorer 11 a partir del 15 de marzo de 2022. Actualice a 6.5.12 o superior para disfrutar de la reproducción progresiva en IE 11.

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
   <td> <p>Dynamic Media - Modo Scene7: flujo de vídeo HLS</p> 
        <p>Dynamic Media - Modo híbrido: descarga progresiva</p>
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
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 6 o anterior)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 7 o posterior)</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Android (navegador predeterminado)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>BlackBerry</p> </td>
   <td> <p>Flujo de vídeo HLS</p> </td>
  </tr>
 </tbody>
</table>

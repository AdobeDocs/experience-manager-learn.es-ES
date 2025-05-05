---
title: Uso del reproductor de vídeo en Dynamic Media de AEM
description: El reproductor de vídeo Dynamic Media de AEM, que antes dependía del tiempo de ejecución de Flash para admitir la transmisión de vídeo adaptable en clientes y exploradores de escritorio, se volvió más agresivo con la transmisión de contenido basada en Flash. Con la introducción de HLS (el protocolo de entrega de vídeo HTTP Live Streaming de Apple), el contenido ahora se puede transmitir sin depender de Flash.
feature: Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 568
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# Uso del reproductor de vídeo en Dynamic Media de AEM{#using-the-video-player-in-aem-dynamic-media}

El reproductor de vídeo Dynamic Media de AEM, que antes dependía del tiempo de ejecución de Flash para admitir la transmisión de vídeo adaptable en clientes y exploradores de escritorio, se volvió más agresivo con la transmisión de contenido basada en Flash. Con la introducción de HLS (el protocolo de entrega de vídeo HTTP Live Streaming de Apple), el contenido ahora se puede transmitir sin depender de Flash.

>[!VIDEO](https://video.tv.adobe.com/v/38008?quality=12&learn=on&captions=spa)

## Búsqueda rápida en el reproductor de vídeo no Flash {#quick-look-into-non-flash-video-player}

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
   <td> <p>Dynamic Media - Modo Scene7: flujo de vídeo de HLS</p> 
        <p>Dynamic Media: modo híbrido: descarga progresiva</p>
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
   <td> <p>Streaming de vídeo de HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>Streaming de vídeo de HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Escritorio</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Streaming de vídeo de HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo móvil</p> </td>
   <td> <p>Chrome (Android 6 o anterior)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo móvil</p> </td>
   <td> <p>Chrome (Android 7 o posterior)</p> </td>
   <td> <p>Streaming de vídeo de HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo móvil</p> </td>
   <td> <p>Android (navegador predeterminado)</p> </td>
   <td> <p>Descarga progresiva</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo móvil</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Streaming de vídeo de HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo móvil</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Streaming de vídeo de HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Dispositivo móvil</p> </td>
   <td> <p>BlackBerry</p> </td>
   <td> <p>Streaming de vídeo de HLS</p> </td>
  </tr>
 </tbody>
</table>

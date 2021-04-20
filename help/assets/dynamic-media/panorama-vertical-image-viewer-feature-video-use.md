---
title: Uso de Panorama y Visualizador de imágenes verticales con AEM Assets Dynamic Media
description: Las mejoras del visualizador de Dynamic Media en AEM 6.4 incluyen la adición del visualizador de imágenes panorámicas, el visualizador de imágenes de realidad virtual panorámicas y el visualizador de imágenes verticales. Panoramic Viewer ofrece una manera fácil de ofrecer una experiencia inmersiva y atractiva de la habitación, propiedad, ubicación o paisaje sin ningún desarrollo personalizado.
sub-product: dynamic-media
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 3%

---


# Uso de Panorama y Visualizador de imágenes verticales con AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Las mejoras del visualizador de Dynamic Media en AEM 6.4 incluyen la adición del visualizador de imágenes panorámicas, el visualizador de imágenes de realidad virtual panorámicas y el visualizador de imágenes verticales. Panoramic Viewer ofrece una manera fácil de ofrecer una experiencia inmersiva y atractiva de la habitación, propiedad, ubicación o paisaje sin ningún desarrollo personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>El vídeo supone que la instancia de AEM se está ejecutando en el modo Dynamic Media S7. [Las instrucciones sobre la configuración de AEM con Dynamic Media se encuentran aquí.](https://helpx.adobe.com/es/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visualizador de vídeo panorámico

Una imagen se considera panorámica según su relación de aspecto o palabras clave. De forma predeterminada, una imagen con una relación de aspecto de 2 se considerará una imagen panorámica. Los ajustes preestablecidos del visualizador de imágenes panorámicas estarán disponibles para una vista previa de la imagen si cumplen los criterios anteriores. El criterio de la relación de aspecto de la imagen panorámica se puede modificar en la configuración DMS7 de la empresa especificando la doble propiedad s7PanoramicAR en /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Las palabras clave se almacenan en la propiedad dc:keyword del nodo de metadatos del recurso. Si las palabras clave contienen cualquiera de las siguientes combinaciones :

* equirectangular,
* esférica + panorámica,
* esférica + panorámica,

se considerará un recurso de imagen panorámica independientemente de su proporción de aspecto.

## Visor de imágenes verticales

Con muestras horizontales, dependiendo del tamaño de la pantalla de escritorio del consumidor, a veces las muestras no eran visibles hasta que el usuario se desplazaba hacia abajo por la página. Mediante el uso del visor de imágenes vertical y la colocación de muestras verticales, se garantiza que las muestras sean visibles independientemente del tamaño de la pantalla. También maximiza el tamaño de la imagen principal. Con las muestras horizontales, era necesario reservar espacio en la página para garantizar que tengan una alta probabilidad de ser visibles y que reduzcan el tamaño de la imagen principal. Con un diseño vertical, no es necesario preocuparse por la asignación de este espacio y, por lo tanto, puede maximizar el tamaño de la imagen principal.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visualizador panorámico y VR</td>
   <td>Visor de imágenes verticales</td>
  </tr>
  <tr>
   <td>Modo de ejecución de Dynamic Media</td>
   <td>Solo modo de Dynamic Media Scene7</td>
   <td>DMS7 y Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso práctico   </td>
   <td><p>El visor panorámico y el visor de Realidad virtual proporcionan a los usuarios una experiencia más atractiva. Un usuario puede visitar una habitación de hotel incluso antes de hacer una reserva, o visitar una propiedad de alquiler sin tener que programar una cita. Un usuario puede comprobar una ubicación y muchas más posibilidades. El objetivo principal aquí es ofrecer al consumidor una mejor experiencia cuando visita su sitio web y, finalmente, aumentar su tasa de conversión.</p> <p> </p> </td> 
   <td><p>El visor de imágenes verticales ayuda a maximizar la experiencia de visualización de imágenes del producto para ofrecer a los consumidores la mejor representación del producto, lo que impulsa la conversión y minimiza los retornos.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponible </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configuración de Dynamic Media en el modo Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Configuración de Dynamic Media en modo híbrido](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Los visualizadores panorámicos trabajan con imágenes panorámicas y no están pensados para utilizarse con imágenes normales.

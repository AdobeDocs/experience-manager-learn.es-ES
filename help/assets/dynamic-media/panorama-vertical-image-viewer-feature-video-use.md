---
title: Uso de panoramas y visores de imágenes verticales con AEM Assets Dynamic Media
seo-title: Uso de panoramas y visores de imágenes verticales con AEM Assets Dynamic Media
description: Las mejoras del visor de medios dinámicos de AEM 6.4 incluyen la incorporación del visor de imágenes panorámicas, el visor de imágenes panorámicas de realidad virtual y el visor de imágenes verticales. El visor panorámico ofrece una forma sencilla de ofrecer una experiencia envolvente y atractiva de la habitación, la propiedad, la ubicación o el paisaje sin ningún desarrollo personalizado.
seo-description: Las mejoras del visor de medios dinámicos de AEM 6.4 incluyen la incorporación del visor de imágenes panorámicas, el visor de imágenes panorámicas de realidad virtual y el visor de imágenes verticales. El visor panorámico ofrece una forma sencilla de ofrecer una experiencia envolvente y atractiva de la habitación, la propiedad, la ubicación o el paisaje sin ningún desarrollo personalizado.
sub-product: Dynamic-media
feature: video-profiles, video-profiles, vr-360
topics: videos, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# Uso de Panorama y Visor de imágenes verticales con AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Las mejoras del visor de medios dinámicos de AEM 6.4 incluyen la incorporación del visor de imágenes panorámicas, el visor de imágenes panorámicas de realidad virtual y el visor de imágenes verticales. El visor panorámico ofrece una forma sencilla de ofrecer una experiencia envolvente y atractiva de la habitación, la propiedad, la ubicación o el paisaje sin ningún desarrollo personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>Video supone que la instancia de AEM se está ejecutando en el modo de Dynamic Media S7. [Las instrucciones para configurar AEM con Dynamic Media se pueden encontrar aquí.](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visor panorámico VR

Una imagen se considera panorámica según su proporción de aspecto o palabras clave. De forma predeterminada, una imagen con una proporción de aspecto de 2 se considerará una imagen panorámica. Los ajustes preestablecidos del visor de imágenes panorámicas estarán disponibles para una previsualización de imagen si cumplen los criterios anteriores. El criterio de la relación de aspecto de la imagen panorámica se puede modificar en la configuración de DMS7 de la compañía especificando la propiedad de doble s7PanoramicAR en /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Las palabras clave se almacenan en la propiedad dc:keyword del nodo de metadatos del recurso. Si las palabras clave contienen alguna de las siguientes combinaciones:

* equirectangular,
* esférico + panorámico,
* esférico + panorámica,

se considerará un recurso de imagen panorámica independientemente de su proporción de aspecto.

## Visor de imágenes vertical

Con las muestras horizontales, en función del tamaño de la pantalla del escritorio del consumidor, a veces las muestras no serían visibles hasta que el usuario se desplazara por la página. Al utilizar el visor de imágenes vertical y colocar muestras verticales, garantiza que las muestras sean visibles independientemente del tamaño de la pantalla. También maximiza el tamaño de la imagen principal. Con las muestras horizontales, era necesario reservar espacio en la página para garantizar que tengan una alta probabilidad de ser visibles y que disminuyan el tamaño de la imagen principal. Con un diseño vertical, no es necesario preocuparse de asignar este espacio y, por lo tanto, puede maximizar el tamaño de la imagen principal.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visor panorámico y VR</td>
   <td>Visor de imágenes vertical</td>
  </tr>
  <tr>
   <td>Modo de ejecución de Dynamic Media</td>
   <td>Solo en el modo Scene7 de Dynamic Media</td>
   <td>DMS7 y Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso práctico   </td>
   <td><p>El visor panorámico y el visor de Realidad virtual proporcionan a los usuarios una experiencia más atractiva. Un usuario puede visitar una habitación de hotel incluso antes de hacer una reserva, o visitar una propiedad de alquiler sin tener que programar una cita. Un usuario puede comprobar una ubicación y muchas más posibilidades. El objetivo principal aquí es proporcionar al consumidor una mejor experiencia cuando visita su sitio web y, finalmente, aumentar su tasa de conversión.</p> <p> </p> </td> 
   <td><p>El visor de imágenes verticales ayuda a maximizar la experiencia de visualización de imágenes de productos para ofrecer a los consumidores la mejor representación del producto, lo que genera conversión y minimiza los retornos.</p> <p> </p> </td>
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
>Los visores panorámicos funcionan con imágenes panorámicas y no están pensados para utilizarse con imágenes normales.

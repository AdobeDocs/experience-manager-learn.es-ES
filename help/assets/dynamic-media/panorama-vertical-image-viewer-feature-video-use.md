---
title: Uso del visualizador de imágenes panorámicas y verticales con Dynamic Media para AEM Assets
description: Las mejoras del visualizador de Dynamic Media en AEM 6.4 incluyen la adición del visualizador de imágenes panorámicas, el visualizador de imágenes de realidad virtual panorámica y el visualizador de imágenes verticales. El visor panorámico es una forma sencilla de ofrecer una experiencia atractiva e inmersiva de la habitación, la propiedad, la ubicación o el paisaje sin ningún desarrollo personalizado.
feature: Video Profiles, Video Profiles, 360 VR Video
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
duration: 535
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Uso del visualizador de imágenes panorámicas y verticales con Dynamic Media para AEM Assets{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Las mejoras del visualizador de Dynamic Media en AEM 6.4 incluyen la adición del visualizador de imágenes panorámicas, el visualizador de imágenes de realidad virtual panorámica y el visualizador de imágenes verticales. El visor panorámico es una forma sencilla de ofrecer una experiencia atractiva e inmersiva de la habitación, la propiedad, la ubicación o el paisaje sin ningún desarrollo personalizado.

>[!VIDEO](https://video.tv.adobe.com/v/40212?quality=12&learn=on&captions=spa)

>[!NOTE]
>
>El vídeo supone que la instancia de AEM se está ejecutando en el modo Dynamic Media S7. [Aquí puede encontrar instrucciones para configurar AEM con Dynamic Media.](https://helpx.adobe.com/es/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visor de RV panorámico y panorámico

Una imagen se considera panorámica en función de su proporción de aspecto o palabras clave. De forma predeterminada, una imagen con una proporción de aspecto de 2 se considera una imagen panorámica. Los ajustes preestablecidos del visualizador de imágenes panorámicas están disponibles para una previsualización de imagen si cumplen los criterios anteriores. El criterio de la proporción de aspecto de imagen panorámica se puede modificar en la configuración DMS7 de la empresa especificando la propiedad doble s7PanoramicAR en /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Las palabras clave se almacenan en la propiedad dc:keyword del nodo de metadatos del recurso. Si las palabras clave contienen cualquiera de las siguientes combinaciones:

* equirectangular,
* esférico + panorámico,
* esférico + panorama,

se considera un recurso de imagen panorámica independientemente de su relación de aspecto.

## Visor de imágenes vertical

Con las muestras horizontales, según el tamaño de pantalla de escritorio del consumidor, a veces las muestras no eran visibles hasta que el usuario se desplazaba hacia abajo por la página. Al utilizar el visualizador de imágenes verticales y colocar las muestras verticales, se garantiza que las muestras sean visibles independientemente del tamaño de pantalla. También maximiza el tamaño de la imagen principal. Con las muestras horizontales, era necesario reservar espacio en la página para asegurarse de que tuvieran una alta probabilidad de ser visibles y reducirían el tamaño de la imagen principal. Con un diseño vertical, no es necesario preocuparse por asignar este espacio y, por lo tanto, puede maximizar el tamaño de la imagen principal.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visor panorámico y de realidad virtual</td>
   <td>Visor de imágenes vertical</td>
  </tr>
  <tr>
   <td>Modo de ejecución de Dynamic Media</td>
   <td>Solo modo Scene7 de Dynamic Media</td>
   <td>DMS7 y Dynamic Media</td>
  </tr>
  <tr>
   <td>Caso práctico</td>
   <td><p>El visor panorámico y el visor de realidad virtual proporcionan a los usuarios una experiencia más atractiva. Un usuario puede revisar una habitación de hotel incluso antes de hacer una reserva, o revisar una propiedad de alquiler sin tener que programar una cita. Un usuario puede comprobar una ubicación y muchas más posibilidades. El objetivo principal aquí es proporcionar al consumidor una mejor experiencia cuando visita su sitio web y, finalmente, aumentar la tasa de conversión.</p> <p> </p> </td> 
   <td><p>El visualizador de imágenes verticales maximiza la experiencia de visualización de imágenes del producto para ofrecer a los consumidores la mejor representación del producto, lo que impulsa la conversión y minimiza los retornos.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponible </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configuración de Dynamic Media en el modo Scene7](https://helpx.adobe.com/es/experience-manager/6-5/assets/using/config-dms7.html)

[Configuración de Dynamic Media en modo híbrido](https://helpx.adobe.com/es/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Los visores panorámicos trabajan con imágenes panorámicas y no están pensados para utilizarse con imágenes normales.

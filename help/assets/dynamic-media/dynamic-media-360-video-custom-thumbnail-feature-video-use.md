---
title: Uso de vídeos y miniaturas de vídeo personalizadas de Dynamic Media 360 con AEM Assets
seo-title: Uso de vídeos y miniaturas de vídeo personalizadas de Dynamic Media 360 con AEM Assets
description: Las mejoras del visor de medios dinámicos de AEM 6.5 incluyen la compatibilidad con la representación de vídeo de 360, los visores de medios de 360 (video360Social y video360VR) y la posibilidad de seleccionar miniaturas de vídeo personalizadas.
seo-description: Las mejoras del visor de medios dinámicos de AEM 6.5 incluyen la compatibilidad con la representación de vídeo de 360, los visores de medios de 360 (video360Social y video360VR) y la posibilidad de seleccionar miniaturas de vídeo personalizadas.
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: Dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# Uso de vídeos y miniaturas de vídeo personalizadas de Dynamic Media 360 con AEM Assets

Las mejoras del visor de medios dinámicos de AEM 6.5 incluyen la compatibilidad con la representación de vídeo de 360, los visores de medios de 360 (video360Social y video360VR) y la posibilidad de seleccionar miniaturas de vídeo personalizadas.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>Video supone que la instancia de AEM se está ejecutando en el modo de Dynamic Media S7.  [Las instrucciones para configurar AEM con Dynamic Media se pueden encontrar aquí](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Al cargar un vídeo, de forma predeterminada, Dynamic Media procesa el material de archivo como un vídeo de 360, si tiene una proporción de aspecto de 2:1. es decir, la relación entre la anchura y la altura es de 2:1.

>[!NOTE]
>
>Los componentes de Dynamic Media 360 Media solo admiten 360 vídeos.

## Vídeos de Dynamic Media 360

Los vídeos de 360 grados, también conocidos como vídeos esféricos, son grabaciones de vídeo en las que se graba una vista en todas las direcciones al mismo tiempo, grabadas con una cámara omnidireccional o una colección de cámaras. Durante la reproducción en una pantalla plana, el usuario tiene control de la dirección de visualización y la reproducción en dispositivos móviles suele aprovechar el control integrado del giroscopio.  Le permite expandirse más allá de los límites de la fotografía única. Los especialistas en marketing pueden ofrecer a los usuarios una experiencia atractiva con la ayuda de 360 vídeos.  Empecemos. El criterio de la relación de aspecto de la imagen panorámica se puede modificar en la configuración de DMS7 de la compañía especificando la propiedad de doble s7PanoramicAR en /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vídeos de Dynamic Media 360

El vídeo de Dynamic Media ahora admite la posibilidad de seleccionar una miniatura personalizada para el vídeo. Un usuario puede seleccionar un recurso existente de AEM Assets o un fotograma de vídeo como miniatura.

## Visores de medios dinámicos 360

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Visor social**</td>
      <td>**Visor Video360VR**</td>
   </tr>
   <tr>
      <td>Modo de ejecución de Dynamic Media</td>
      <td>Solo en el modo Scene7 de Dynamic Media</td>
      <td>Solo el modo Scene7 de Dynamic Media<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Caso práctico   </td>
      <td>
         <p>Para sitios web y dispositivos que no admiten el giroscopio</p>
         <p> </p>
      </td>
      <td>
         <p>Proporciona una experiencia de realidad virtual para un dispositivo que admite giroscopio </p>
      </td>
   </tr>
   <tr>
      <td>Audio - Modo estéreo</td>
      <td>No</td>
      <td>Sí</td>
   </tr>
   <tr>
      <td>Reproducción de vídeo</td>
      <td>Sí</td>
      <td>Sí</td>
   </tr>
   <tr>
      <td>Navegación por puntos de vista</td>
      <td>
         <ul>
            <li>Arrastre del ratón (en sistemas de escritorio)</li>
            <li>Barrido (dispositivos táctiles)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Las opciones de ratón y arrastre están desactivadas</li>
            <li>Utiliza un giroscopio integrado</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Reproductor HTML5</td>
      <td>Sí</td>
      <td>Sí</td>
   </tr>
   <tr>
      <td>Opciones de uso compartido en redes sociales</td>
      <td>Sí</td>
      <td>No</td>
   </tr>
</tbody>
</table>

## Recursos adicionales{#additional-resources}

[Configuración de Dynamic Media en el modo Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

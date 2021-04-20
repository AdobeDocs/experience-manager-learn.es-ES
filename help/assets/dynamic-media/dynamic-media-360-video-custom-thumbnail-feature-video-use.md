---
title: Uso de vídeos de Dynamic Media 360 y miniaturas de vídeo personalizadas con AEM Assets
description: Las mejoras del visualizador de Dynamic Media en AEM 6.5 incluyen la compatibilidad con la representación de vídeo 360, 360 visualizadores de medios (video360Social y video360VR) y la capacidad de seleccionar miniaturas de vídeo personalizadas.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 5%

---


# Uso de vídeos de Dynamic Media 360 y miniaturas de vídeo personalizadas con AEM Assets

Las mejoras del visualizador de Dynamic Media en AEM 6.5 incluyen la compatibilidad con la representación de vídeo 360, 360 visualizadores de medios (video360Social y video360VR) y la capacidad de seleccionar miniaturas de vídeo personalizadas.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>El vídeo supone que la instancia de AEM se está ejecutando en el modo Dynamic Media S7.  [Las instrucciones sobre la configuración de AEM con Dynamic Media se encuentran aquí](https://helpx.adobe.com/es/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Al cargar un vídeo, de forma predeterminada, Dynamic Media procesa el material de archivo como un vídeo de 360, si tiene una proporción de aspecto de 2:1. es decir, la relación ancho-alto es 2:1.

>[!NOTE]
>
>Los componentes de Dynamic Media 360 Media solo admiten 360 vídeos.

## Vídeos de Dynamic Media 360

Los vídeos de 360 grados, también conocidos como vídeos esféricos, son grabaciones de vídeo en las que se graba una vista en todas las direcciones al mismo tiempo, grabadas con una cámara omnidireccional o una colección de cámaras. Durante la reproducción en una pantalla plana, el usuario tiene control de la dirección de visualización y la reproducción en dispositivos móviles suele utilizar el control integrado de giroscopio.  Permite expandir más allá de los límites de la fotografía única. Los especialistas en marketing pueden ofrecer a los usuarios una experiencia atractiva con la ayuda de 360 vídeos.  Empecemos. El criterio de la relación de aspecto de la imagen panorámica se puede modificar en la configuración DMS7 de la empresa especificando la doble propiedad s7PanoramicAR en /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vídeos de Dynamic Media 360

El vídeo de Dynamic Media ahora admite la posibilidad de seleccionar una miniatura personalizada para el vídeo. Un usuario puede seleccionar un recurso existente de AEM Assets o un fotograma de vídeo como miniatura.

## Visualizadores de medios dinámicos 360

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Visor de vídeo360Social**</td>
      <td>**Visor de Video360VR**</td>
   </tr>
   <tr>
      <td>Modo de ejecución de Dynamic Media</td>
      <td>Solo modo de Dynamic Media Scene7</td>
      <td>Solo en modo de Dynamic Media Scene7<br>
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
         <p>Proporciona una experiencia de realidad virtual para un dispositivo compatible con giroscopio </p>
      </td>
   </tr>
   <tr>
      <td>Audio: modo estéreo</td>
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
            <li>Arrastrar el ratón (en sistemas de escritorio)</li>
            <li>Deslizar (dispositivos táctiles)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Las opciones de mouse y arrastre están desactivadas</li>
            <li>Utiliza el ensanchamiento integrado</li>
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

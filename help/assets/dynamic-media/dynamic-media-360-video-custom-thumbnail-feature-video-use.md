---
title: Uso de vídeos de Dynamic Media 360 y miniaturas de vídeo personalizadas con AEM Assets
description: Las mejoras de Dynamic Media AEM Viewer en la versión 6.5 de incluyen la adición de compatibilidad con el procesamiento de vídeo 360, 360 visores de medios (video360Social y video360VR) y la capacidad de seleccionar miniaturas de vídeo personalizadas.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 4%

---

# Uso de vídeos de Dynamic Media 360 y miniaturas de vídeo personalizadas con AEM Assets

Las mejoras de Dynamic Media AEM Viewer en la versión 6.5 de incluyen la adición de compatibilidad con el procesamiento de vídeo 360, 360 visores de medios (video360Social y video360VR) y la capacidad de seleccionar miniaturas de vídeo personalizadas.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>AEM En el vídeo se da por hecho que la instancia de se está ejecutando en el modo Dynamic Media S7.  [AEM Puede encontrar instrucciones sobre la configuración de la con Dynamic Media aquí](https://helpx.adobe.com/es/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). De forma predeterminada, al cargar un vídeo, Dynamic Media procesa el material de archivo como un vídeo de 360, si tiene una relación de aspecto de 2:1. es decir, la relación entre anchura y altura es de 2:1.

>[!NOTE]
>
>Los componentes multimedia de Dynamic Media 360 solo admiten vídeos de 360.

## Vídeos de Dynamic Media 360

Los vídeos de 360 grados, también conocidos como vídeos esféricos, son grabaciones de vídeo en las que se graba una vista en todas las direcciones al mismo tiempo, grabadas con una cámara omnidireccional o una colección de cámaras. Durante la reproducción en una pantalla plana, el usuario tiene control de la dirección de visualización y la reproducción en dispositivos móviles suele aprovechar el control de giroscopio integrado.  Le permite expandirse más allá de los límites de la fotografía individual. Los especialistas en marketing pueden proporcionar a los usuarios una experiencia atractiva con la ayuda de 360 vídeos.  Vamos a empezar... El criterio de la proporción de aspecto de imagen panorámica se puede modificar en la configuración DMS7 de la empresa especificando la propiedad doble s7PanoramicAR en /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vídeos de Dynamic Media 360

El vídeo de Dynamic Media ahora admite la capacidad de seleccionar una miniatura personalizada para el vídeo. Un usuario puede seleccionar un recurso existente de AEM Assets o seleccionar un fotograma de vídeo como miniatura.

## Visores de Dynamic 360 Media

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Visor social**</td>
      <td>**Visor Video360VR**</td>
   </tr>
   <tr>
      <td>Modo de ejecución de Dynamic Media</td>
      <td>Solo modo Scene7 de Dynamic Media</td>
      <td>Solo modo Scene7 de Dynamic Media<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Caso práctico</td>
      <td>
         <p>Para sitios web y dispositivos que no admiten giroscopio</p>
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
      <td>Navegación por punto de vista</td>
      <td>
         <ul>
            <li>Arrastre con ratón (en sistemas de escritorio)</li>
            <li>Deslizar (dispositivos táctiles)</li>
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
      <td>Reproductor de HTML 5</td>
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

[Configuración de Dynamic Media en modo Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

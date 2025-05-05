---
title: Uso de vídeos de Dynamic Media 360 y miniaturas de vídeo personalizadas con AEM Assets
description: Las mejoras del visualizador de Dynamic Media en AEM 6.5 incluyen la adición de compatibilidad con el procesamiento de vídeo 360, 360 visualizadores de medios (video360Social y video360VR) y la capacidad de seleccionar miniaturas de vídeo personalizadas.
feature: Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 656
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 3%

---

# Uso de vídeos de Dynamic Media 360 y miniaturas de vídeo personalizadas con AEM Assets

Las mejoras del visualizador de Dynamic Media en AEM 6.5 incluyen la adición de compatibilidad con el procesamiento de vídeo 360, 360 visualizadores de medios (video360Social y video360VR) y la capacidad de seleccionar miniaturas de vídeo personalizadas.

>[!VIDEO](https://video.tv.adobe.com/v/34533?quality=12&learn=on&captions=spa)

>[!NOTE]
>
>El vídeo supone que la instancia de AEM se está ejecutando en el modo Dynamic Media S7.  [Aquí encontrará instrucciones para configurar AEM con Dynamic Media](https://helpx.adobe.com/es/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Al cargar un vídeo, Dynamic Media procesa el material de archivo como un vídeo de 360 de forma predeterminada si tiene una relación de aspecto de 2:1. es decir, la relación entre anchura y altura es de 2:1.

>[!NOTE]
>
>Los componentes de Dynamic Media 360 Media solo admiten vídeos de 360.

## Vídeos de Dynamic Media 360

Los vídeos de 360 grados, también conocidos como vídeos esféricos, son grabaciones de vídeo en las que se graba una vista en todas las direcciones al mismo tiempo, grabadas con una cámara omnidireccional o una colección de cámaras. Durante la reproducción en una pantalla plana, el usuario tiene control de la dirección de visualización y la reproducción en dispositivos móviles suele aprovechar el control de giroscopio integrado.  Le permite expandirse más allá de los límites de la fotografía individual. Los especialistas en marketing pueden proporcionar a los usuarios una experiencia atractiva con la ayuda de 360 vídeos.  Vamos a empezar... El criterio de la proporción de aspecto de imagen panorámica se puede modificar en la configuración DMS7 de la empresa especificando la propiedad doble s7PanoramicAR en /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vídeos de Dynamic Media 360

El vídeo de Dynamic Media ahora admite la capacidad de seleccionar una miniatura personalizada para el vídeo. Un usuario puede seleccionar un recurso existente de los AEM Assets o seleccionar un fotograma de vídeo como miniatura.

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
      <td>Solo modo Scene7 de medios dinámicos<br>
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
      <td>Reproductor de HTML5</td>
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

[Configuración de Dynamic Media en el modo Scene7](https://helpx.adobe.com/es/experience-manager/6-5/assets/using/config-dms7.html)

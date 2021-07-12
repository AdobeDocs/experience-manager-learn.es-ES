---
title: Uso del Recorte inteligente con AEM Assets Dynamic Media
description: Smart Crop utiliza Adobe Sensei para eliminar las costosas y largas tareas de recorte de contenido para un diseño interactivo.
sub-product: dynamic-media
feature: Recorte inteligente, perfiles de imagen
version: 6.4, 6.5
topic: Administración de contenido
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 3%

---


# Uso del Recorte inteligente con AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop utiliza Adobe Sensei para eliminar las costosas y largas tareas de recorte de contenido para un diseño interactivo.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>El vídeo supone que la instancia de AEM se está ejecutando en el modo Dynamic Media S7. [Las instrucciones para configurar AEM con Dynamic Media se encuentran aquí.](https://helpx.adobe.com/es/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## La capacidad de recorte inteligente de Dynamic Media de Adobe Experience Manager incluye

* AEM administradores de recursos pueden crear fácilmente perfiles de imagen para el recorte inteligente en función del ancho y la altura del dispositivo.
* El recorte inteligente se puede realizar para un recurso individual o para todos los recursos de una carpeta.
* Se puede cambiar el tamaño del diseño de edición de recorte inteligente para mejorar la visibilidad.
* El componente Dynamic Media de AEM Sites es compatible con el recorte inteligente.
* La URL publicada para el recurso recortado inteligente está disponible para su uso con aplicaciones de terceros que acepten recursos alojados.

>[!NOTE]
>
>Las coordenadas de recorte inteligentes dependen de la relación de aspecto. Es decir, para los distintos ajustes de recorte inteligente de un perfil de imagen, si la proporción de aspecto es la misma para las dimensiones agregadas en el perfil de imagen, se envía la misma proporción de aspecto a Dynamic Media. Debido a esto, se sugerirá el mismo área de recorte en el Editor de recorte inteligente. Por ejemplo, si se configura un recorte de 100 x 100 y 200 x 200, el sistema generará el mismo recorte inteligente.

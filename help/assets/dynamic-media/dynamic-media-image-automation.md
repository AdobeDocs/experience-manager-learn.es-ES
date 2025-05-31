---
title: Dynamic Media para el procesamiento por lotes de transparencia y automatización de contenido
description: Aprenda a utilizar Dynamic Media en AEM para crear representaciones virtuales, administrar la transparencia y automatizar el procesamiento de imágenes para la reutilización de contenido escalable.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 6%

---


# Dynamic Media para el procesamiento por lotes de transparencia y automatización de contenido

Aprenda a utilizar Dynamic Media en AEM para crear representaciones virtuales, administrar la transparencia y automatizar el procesamiento de imágenes para la reutilización de contenido escalable.

>[!VIDEO](https://video.tv.adobe.com/v/3463048/?learn=on&enablevpops&captions=spa)


## Ejemplo de recursos de Dynamic Media

A continuación se muestran algunos ejemplos de recursos de Dynamic Media y sus direcciones URL utilizadas en el vídeo.

>[!BEGINTABS]

>[!TAB ejemplos de transparencia de imagen]

A continuación se muestran las direcciones URL de ejemplo del servidor de imágenes de Dynamic Media utilizadas en el vídeo.

| Vista previa | Descripción | URL de Dynamic Media |
|-----------|------------------|---------|
| ![Predeterminado](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Predeterminado | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![Compuesto con capa de imagen de fondo sin fisuras](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Compuesto con capa de imagen de fondo sin fisuras | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Fondo rojo](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Fondo rojo | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Recortado a ruta ovalada](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | Recortado a trazado ovalado | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB Ejemplos de ruta de acceso de imagen]

A continuación se muestran las direcciones URL de ejemplo del servidor de imágenes de Dynamic Media utilizadas en el vídeo.

| Vista previa | Descripción | URL de Dynamic Media |
|-----------|------------------|---------|
| ![Normalizado a 80 píxeles de ancho (sin transparencia)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Normalizado a 80 píxeles de ancho (sin transparencia){width="250"} | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![Recortar a ruta](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | Recortar a ruta | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![Recorte en ruta](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | Recortar a trazado | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![Recortar a ruta y recortar a ruta](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | Recortar a trazado y Recortar a trazado | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![Recortar a otra ruta](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | Recortar a otra ruta | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![Recortar a otra ruta y hacer fondo rojo](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | Recortar a otra ruta y hacer fondo rojo | [Vincular](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## API del servidor de imágenes de Dynamic Media

* [API de servicio y procesamiento de imágenes de Dynamic Media](https://experienceleague.adobe.com/es/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Vista previa de instantánea de Dynamic Media](https://snapshot.scene7.com/)

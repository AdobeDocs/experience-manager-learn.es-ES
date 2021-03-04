---
title: Explicación de la gestión de color con AEM Dynamic Media
description: En este vídeo analizamos la administración de color de Dynamic Media y cómo se puede utilizar para proporcionar funciones de previsualización de corrección de color en para AEM Assets.
sub-product: dynamic-media
feature: Perfiles de imagen, Perfiles de vídeo
version: 6.3, 6.4, 6.5
topic: Administración de contenido
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 15%

---


# Explicación de la gestión de color con AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

En este vídeo analizamos la administración de color de Dynamic Media y cómo se puede utilizar para proporcionar funciones de previsualización de corrección de color en para AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Habilite Dynamic ](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) Media AEM para utilizar esta función.

Esta función está disponible para las versiones 6.1 y 6.2 de AEM as a Feature Pack.

## Plantilla XML para el nodo de configuración {#xml-template-for-the-color-management-configuration-node} de Gestión de color

A continuación se muestra la plantilla XML para el nodo de configuración Gestión de color. Esta plantilla XML se puede copiar en el proyecto de desarrollo de AEM y configurar con las configuraciones adecuadas para el proyecto.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### La lista de perfiles de color predeterminados de Adobe se muestra a continuación {#list-of-default-adobe-color-profiles-are-listed-below}

| Nombre | Espacio color | Descripción |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Recubierto FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | Recubierto FOGRA39 (ISO 12647-2:2004) |
| CoatedGraCol | CMYK | Recubierto GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europa ISO Coated FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncovered | CMYK | Euroscale Uncovered v2 |
| JapanColorCoated | CMYK | Color japonés 2001 recubierto |
| Diario JapanColor | CMYK | Diario Japan Color 2002 |
| JapanColorUncovered | CMYK | Color japonés 2001 sin recubrir |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japón Web Coated (anuncio) |
| NewsprintSNAP2007 | CMYK | US Newsprint (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| PAL | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | CMYK predeterminado de Photoshop 4 |
| PS5Default | CMYK | CMYK predeterminado de Photoshop 5 |
| Coated | CMYK | U.S. Sheetfeed Coated v2 |
| SheetedUncovered | CMYK | U.S. Sheetfeed Uncovered v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| FograNoCubierta29 | CMYK | FOGRA29 no recubierto (ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Papel Web Coated SWOP 2006 Grado 3 |
| WebCoatedGrade5 | CMYK | Papel Web Coated SWOP 2006 Grado 5 |
| WebUncovered | CMYK | U.S. Web Uncovered v2 |
| WideGamutRGB | RGB | RGB de gama amplia |

## Recursos adicionales{#additional-resources}

* [Configuración de la administración de color de Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)

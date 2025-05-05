---
title: Explicación de la administración de color con AEM Dynamic Media
description: En este vídeo exploramos la administración de color de Dynamic Media y cómo se puede utilizar para proporcionar funciones de previsualización de corrección de color en para AEM Assets.
feature: Image Profiles, Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Feature Video
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
duration: 274
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 11%

---

# Explicación de la administración de color con AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

En este vídeo exploramos la administración de color de Dynamic Media y cómo se puede utilizar para proporcionar funciones de previsualización de corrección de color en para AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[Habilite Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es) en AEM para utilizar esta función.

Esta función está disponible para las versiones de AEM 6.1 y 6.2 como paquete de funciones.

## Plantilla XML para el nodo de configuración de gestión de color {#xml-template-for-the-color-management-configuration-node}

A continuación se muestra la plantilla XML para el nodo de configuración de gestión de color. Esta plantilla XML se puede copiar en el proyecto de desarrollo de AEM y configurarse con las configuraciones adecuadas al proyecto.

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

### A continuación, se muestra una lista de los perfiles de color predeterminados de Adobe {#list-of-default-adobe-color-profiles-are-listed-below}

| Nombre | Espacio color | Descripción |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | FOGRA27 recubierto (ISO 12647-2:2004) |
| CoatedFogra39 | CMYK | FOGRA39 recubierto (ISO 12647-2:2004) |
| CoatedGraCol | CMYK | Revestido GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europa ISO Coated FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncovered | CMYK | Euroscale Uncovered v2 |
| JapanColorCoated | CMYK | Japón Color 2001 Revestido |
| JapanColorNewspaper | CMYK | Japón Color 2002 Periódico |
| JapanColorUncovered | CMYK | Japón Color 2001 Sin recubrimiento |
| JapanColorWebCoated | CMYK | Japón Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japón Web Coated (Ad) |
| NewsprintSNAP2007 | CMYK | Boletín de Estados Unidos (SNAP 2007) |
| NTSC | RGB | NTSC (1953) |
| AMIGO | RGB | PAL/SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Predeterminado | CMYK | CMYK predeterminado de Photoshop 4 |
| PS5Default | CMYK | CMYK predeterminado de Photoshop 5 |
| Revestido Con Hojas | CMYK | U.S. Sheetfed Coated v2 |
| Con hojasSin recubrir | CMYK | U.S. Sheetfed Uncovered v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| Fogra29 sin recubrimiento | CMYK | FOGRA29 sin recubrimiento (ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Revestimiento Web FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Papel SWOP 2006 Grado 3 Revestido por Web |
| WebCoatedGrade5 | CMYK | Papel SWOP 2006 Grado 5 Revestido por Web |
| WebUncovered | CMYK | U.S. Web Uncovered v2 |
| WideGamutRGB | RGB | RGB de gama amplia |

## Recursos adicionales{#additional-resources}

* [Configuración de la administración de color de Dynamic Media](https://helpx.adobe.com/es/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)

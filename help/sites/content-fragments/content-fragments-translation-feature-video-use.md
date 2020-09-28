---
title: Uso de la traducción con fragmentos de contenido AEM
description: AEM 6.3 introduce la capacidad de traducir fragmentos de contenido. Los recursos de medios mixtos y las colecciones de recursos asociadas a un fragmento de contenido también son elegibles para extraerse y traducirse.
sub-product: sitios, recursos, servicios de contenido
feature: content-fragments, multi-site-manager
topics: localization, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 6%

---


# Uso de la traducción con fragmentos de contenido AEM{#using-translation-with-aem-content-fragments}

AEM 6.3 introduce la capacidad de traducir fragmentos de contenido. Los recursos de medios mixtos y las colecciones de recursos asociadas a un fragmento de contenido también son elegibles para extraerse y traducirse.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=9&learn=on)

## Casos de uso de la traducción de fragmentos de contenido {#content-fragment-translation-use-cases}

Los fragmentos de contenido son un tipo de contenido reconocido que AEM extraer para enviarse a un servicio de traducción externo. Se admiten varios casos de uso de forma predeterminada:

1. Se puede seleccionar un fragmento de contenido directamente en la consola Recursos para la traducción y la copia del idioma
2. Los fragmentos de contenido a los que se hace referencia en una página Sitios se copiarán en la carpeta de idioma correspondiente y se extraerán para su traducción cuando se seleccione la página Sitios para la copia de idioma
3. Los recursos de medios en línea incrustados dentro de un fragmento de contenido pueden extraerse y traducirse.
4. Las colecciones de recursos asociadas a un fragmento de contenido pueden extraerse y traducirse

## Opciones de configuración de traducción {#translation-config-options}

La configuración de traducción lista para usar admite varias opciones para traducir fragmentos de contenido. De forma predeterminada, los recursos de medios en línea y las colecciones de recursos asociadas NO se traducen. Para actualizar la configuración de traducción, vaya a [http://localhost:4502/etc/cloudservices/translation/default_translation.html](http://localhost:4502/etc/cloudservices/translation/default_translation.html).

Existen cuatro opciones para traducir recursos de fragmento de contenido:

1. **No traducir (predeterminado)**
2. **Solo recursos de medios en línea**
3. **Solo colecciones de recursos asociados**
4. **Recursos de medios en línea y colecciones asociadas**

![Configuración de traducción](assets/classic-ui-dialog.png)

---
title: Compatibilidad de traducción para fragmentos de contenido AEM
description: Descubra cómo se pueden localizar y traducir fragmentos de contenido con Adobe Experience Manager. Los recursos de medios mixtos asociados con un fragmento de contenido también son elegibles para extraerse y traducirse.
feature: Fragmentos de contenido, Administrador de varios sitios
topics: Localization
role: Profesional del negocio
level: Intermedio
version: 6.3, 6.4, 6.5, cloud-service
kt: 201
thumbnail: 18131.jpg
translation-type: tm+mt
source-git-commit: 4620acc18a08d71994753903b79247a8ed3fd8f5
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 2%

---


# Compatibilidad de traducción para fragmentos de contenido AEM {#translation-support-content-fragments}

Descubra cómo se pueden localizar y traducir fragmentos de contenido con Adobe Experience Manager. Los recursos de medios mixtos asociados con un fragmento de contenido también son elegibles para extraerse y traducirse.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Casos de uso de traducción de fragmentos de contenido {#content-fragment-translation-use-cases}

Los fragmentos de contenido son un tipo de contenido reconocido que AEM los extractos para enviarlos a un servicio de traducción externo. Se admiten varios casos de uso de forma predeterminada:

1. Se puede seleccionar un fragmento de contenido directamente en la consola Recursos para la traducción y la copia del idioma
2. Los fragmentos de contenido a los que se hace referencia en una página Sitios se copian en la carpeta de idioma correspondiente y se extraen para su traducción cuando se selecciona la página Sitios para la copia de idioma
3. Los recursos de medios en línea incrustados dentro de un fragmento de contenido pueden extraerse y traducirse.
4. Las colecciones de recursos asociadas a un fragmento de contenido pueden extraerse y traducirse

## Editor de reglas de traducción {#translation-rules-editor}

El comportamiento de traducción de Experience Manager se puede actualizar mediante el **Editor de reglas de traducción**. Para actualizar la traducción, vaya a **Herramientas** > **General** > **Configuración de traducción** en [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Las configuraciones predeterminadas hacen referencia a Fragmentos de contenido en `fragmentPath` con un tipo de recurso de `core/wcm/components/contentfragment/v1/contentfragment`. Todos los componentes que heredan del `v1/contentfragment` se reconocen en la configuración predeterminada.

![Editor de reglas de traducción](assets/translation-configuration.png)

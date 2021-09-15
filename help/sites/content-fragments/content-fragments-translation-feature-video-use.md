---
title: Compatibilidad de traducción para fragmentos de contenido AEM
description: Descubra cómo se pueden localizar y traducir los fragmentos de contenido con Adobe Experience Manager. Los recursos de medios mixtos asociados a un fragmento de contenido también pueden extraerse y traducirse.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: 6.3, 6.4, 6.5, Cloud Service
kt: 201
thumbnail: 18131.jpg
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 2%

---

# Compatibilidad de traducción para fragmentos de contenido AEM {#translation-support-content-fragments}

Descubra cómo se pueden localizar y traducir los fragmentos de contenido con Adobe Experience Manager. Los recursos de medios mixtos asociados a un fragmento de contenido también pueden extraerse y traducirse.

>[!VIDEO](https://video.tv.adobe.com/v/18131/?quality=12&learn=on)

## Casos de uso de traducción de fragmentos de contenido {#content-fragment-translation-use-cases}

Los fragmentos de contenido son un tipo de contenido reconocido que AEM extractos para enviarlos a un servicio de traducción externa. Se admiten varios casos de uso predeterminados:

1. Se puede seleccionar un fragmento de contenido directamente en la consola Recursos para la copia y traducción de idiomas
2. Los fragmentos de contenido a los que se hace referencia en una página Sitios se copian en la carpeta de idioma correspondiente y se extraen para su traducción cuando se selecciona la página Sitios para la copia de idioma
3. Los recursos de medios en línea incrustados dentro de un fragmento de contenido pueden extraerse y traducirse.
4. Las colecciones de recursos asociadas a un fragmento de contenido pueden extraerse y traducirse

## Editor de reglas de traducción {#translation-rules-editor}

El comportamiento de traducción del Experience Manager se puede actualizar mediante el **Editor de reglas de traducción**. Para actualizar la traducción, vaya a **Tools** > **General** > **Translation Configuration** en [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Las configuraciones predeterminadas hacen referencia a los fragmentos de contenido en `fragmentPath` con un tipo de recurso `core/wcm/components/contentfragment/v1/contentfragment`. La configuración predeterminada reconoce todos los componentes que heredan del `v1/contentfragment`.

![Editor de reglas de traducción](assets/translation-configuration.png)

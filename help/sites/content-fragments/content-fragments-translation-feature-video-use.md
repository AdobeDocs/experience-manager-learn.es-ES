---
title: Compatibilidad de traducción con fragmentos de contenido de AEM
description: Descubra cómo se pueden localizar y traducir los fragmentos de contenido con Adobe Experience Manager. Los recursos de medios mixtos asociados a un fragmento de contenido también pueden extraerse y traducirse.
feature: Content Fragments, Multi Site Manager
topic: Localization
role: User
level: Intermediate
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
jira: KT-201
thumbnail: 18131.jpg
doc-type: Feature Video
exl-id: cc4ffbd0-207a-42e4-bfcb-d6c83fb97237
duration: 223
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 2%

---

# Compatibilidad de traducción con fragmentos de contenido de AEM {#translation-support-content-fragments}

Descubra cómo se pueden localizar y traducir los fragmentos de contenido con Adobe Experience Manager. Los recursos de medios mixtos asociados a un fragmento de contenido también pueden extraerse y traducirse.

>[!VIDEO](https://video.tv.adobe.com/v/3410323?quality=12&learn=on&captions=spa)

## Casos de uso de traducción de fragmentos de contenido {#content-fragment-translation-use-cases}

Los fragmentos de contenido son un tipo de contenido reconocido que AEM extrae para enviarlo a un servicio de traducción externo. Se admiten varios casos de uso predeterminados:

1. Se puede [seleccionar un fragmento de contenido directamente en la consola de Assets para la copia y traducción de idioma](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=es).
2. Los fragmentos de contenido a los que se hace referencia en una página de Sites se copian en la carpeta de idioma correspondiente y se extraen para su traducción cuando la página de Sites se selecciona para la copia de idioma.
3. Los recursos de medios en línea incrustados dentro de un fragmento de contenido pueden extraerse y traducirse.
4. Las colecciones de recursos asociadas a un fragmento de contenido pueden extraerse y traducirse.

## Editor de reglas de traducción {#translation-rules-editor}

El comportamiento de la traducción de Experience Manager se puede actualizar usando el **Editor de reglas de traducción**. Para actualizar la traducción, vaya a **Herramientas** > **General** > **Configuración de traducción** en [http://localhost:4502/libs/cq/translation/translationrules/contexts.html](http://localhost:4502/libs/cq/translation/translationrules/contexts.html).

Las configuraciones predeterminadas hacen referencia a fragmentos de contenido en `fragmentPath` con un tipo de recurso de `core/wcm/components/contentfragment/v1/contentfragment`. La configuración predeterminada reconoce todos los componentes que heredan de `v1/contentfragment`.

![Editor de reglas de traducción](assets/translation-configuration.png)

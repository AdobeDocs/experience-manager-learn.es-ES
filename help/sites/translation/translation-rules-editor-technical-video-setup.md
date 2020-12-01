---
title: Configurar reglas de traducción en AEM
description: La interfaz de usuario de configuración de traducción permite al usuario administrar las reglas para traducir contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.
feature: language-copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---


# Configurar reglas de traducción {#set-up-translation-rules-in-aem}

La interfaz de usuario de configuración de traducción permite al usuario administrar las reglas para traducir contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.

>[!NOTE]
>
> El siguiente video fue grabado en el AEM 6.3. AEM 6.4+ presenta una nueva estructura de repositorio para almacenar el archivo XML de reglas de traducción. Al utilizar la interfaz de usuario de configuración de traducción en AEM 6.4 o posterior, las reglas se guardan en la ubicación `/conf/global/settings/translation/rules/translation_rules.xml`. Consulte [Identificación del contenido para traducir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) para obtener más detalles.

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

Las reglas de traducción identifican el contenido de AEM que se va a extraer para la traducción. Las reglas de traducción integradas cubren casos de uso comunes, como componentes de texto y texto alternativo para componentes de imagen. Dependiendo de los requisitos de traducción de los proyectos, puede que se necesiten reglas adicionales. En general, las reglas de traducción permiten a los usuarios especificar:

1. Propiedades que deben traducirse según la ruta o el tipo de recurso
2. Filtros para propiedades que NO deben traducirse
3. Contenido al que se hace referencia y que debe traducirse (es decir, imágenes o fragmentos de contenido)

El editor de reglas de traducción que actualizará el archivo xml de traducción. La interfaz de usuario de configuración de traducción facilita la administración de varias reglas de traducción y protege contra errores tipográficos al editar XML directamente.

Acceda a la interfaz de usuario de configuración de traducción:

* **[!UICONTROL Menú]  Inicio AEM>  [!UICONTROL Herramientas] >  [!UICONTROL General] > Configuración  [[!UICONTROL de traducción]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Antes de AEM 6.3 {#prior-to-aem}

En versiones anteriores AEM las reglas de traducción se actualizaban manualmente editando un archivo XML ubicado en el flujo de trabajo de traducción: `/etc/workflow/models/translation/translation_rules.xml`.

## Recursos adicionales {#additional-resources}

* [Identificación del contenido para traducir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traducción de contenido para sitios multilingües](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Prácticas recomendadas de traducción](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)

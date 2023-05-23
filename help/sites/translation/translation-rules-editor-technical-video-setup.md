---
title: AEM Configuración de reglas de traducción en la
description: La interfaz de usuario de configuración de traducción permite al usuario administrar las reglas para traducir contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.
feature: Language Copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
exl-id: 359da531-839c-4680-abf9-c880cc700159
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 6%

---

# Configuración de reglas de traducción {#set-up-translation-rules-in-aem}

La interfaz de usuario de configuración de traducción permite al usuario administrar las reglas para traducir contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.

>[!NOTE]
>
> AEM El siguiente vídeo se grabó en el 6.3 de. AEM La versión 6.4 o posterior presenta una nueva estructura de repositorio para almacenar el archivo XML de reglas de traducción. AEM Cuando se utiliza la interfaz de usuario de configuración de traducción en la versión 6.4 (o posterior), las reglas se guardan en la ubicación de `/conf/global/settings/translation/rules/translation_rules.xml`. Consulte [Identificación del contenido para traducir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) para obtener más información.

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

AEM Las reglas de traducción identifican el contenido en el que se va a extraer el contenido para su traducción. Las reglas de traducción listas para usar abarcan casos de uso comunes, como componentes de texto y texto alternativo para componentes de imagen. Según los requisitos de traducción de un proyecto, pueden ser necesarias reglas adicionales. En general, las reglas de traducción permiten a los usuarios especificar:

1. Propiedades que deben traducirse según la ruta o el tipo de recurso
2. Filtra por propiedades que NO deben traducirse
3. Contenido de referencia que debe traducirse (es decir, imágenes o fragmentos de contenido)

El editor de reglas de traducción que actualizará el archivo xml de traducción. La interfaz de usuario de configuración de traducción facilita la administración de varias reglas de traducción y protege contra errores tipográficos al editar XML directamente.

Acceda a la interfaz de usuario de configuración de traducción:

* **[!UICONTROL AEM Menú Inicio] > [!UICONTROL Herramientas] > [!UICONTROL General] > [[!UICONTROL Configuración de traducción]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM Antes de la versión 6.3 {#prior-to-aem}

AEM En versiones anteriores de la versión del archivo, las reglas de traducción se actualizaban manualmente editando un archivo XML ubicado en el flujo de trabajo de traducción: `/etc/workflow/models/translation/translation_rules.xml`.

## Recursos adicionales {#additional-resources}

* [Identificación del contenido a traducir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traducción de contenido para sitios multilingües](https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Prácticas recomendadas de traducción](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)

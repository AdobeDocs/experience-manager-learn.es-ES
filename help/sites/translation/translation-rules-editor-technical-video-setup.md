---
title: AEM Configuración de reglas de traducción en la
description: La interfaz de usuario de configuración de traducción permite al usuario administrar las reglas para traducir contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.
feature: Language Copy
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 542
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Configuración de reglas de traducción {#set-up-translation-rules-in-aem}

La interfaz de usuario de configuración de traducción permite al usuario administrar las reglas para traducir contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.

>[!NOTE]
>
> AEM El siguiente vídeo se grabó en el 6.3 de. AEM La versión 6.4 o posterior presenta una nueva estructura de repositorio para almacenar el archivo XML de reglas de traducción. AEM Al utilizar la interfaz de usuario de configuración de traducción en la versión 6.4 (o posterior), las reglas se guardan en la ubicación `/conf/global/settings/translation/rules/translation_rules.xml`. Consulte [Identificación del contenido para traducir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) para obtener más información.

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

AEM Las reglas de traducción identifican el contenido en el que se va a extraer el contenido para su traducción. Las reglas de traducción listas para usar abarcan casos de uso comunes, como componentes de texto y texto alternativo para componentes de imagen. Según los requisitos de traducción de un proyecto, pueden ser necesarias reglas adicionales. En general, las reglas de traducción permiten a los usuarios especificar:

1. Propiedades que deben traducirse según la ruta o el tipo de recurso
2. Filtra por propiedades que NO deben traducirse
3. Contenido de referencia que debe traducirse (es decir, imágenes o fragmentos de contenido)

El editor de reglas de traducción que actualizará el archivo xml de traducción. La interfaz de usuario de configuración de traducción facilita la administración de varias reglas de traducción y protege contra errores tipográficos al editar XML directamente.

Acceda a la interfaz de usuario de configuración de traducción:

* AEM **[!UICONTROL Menú Inicio de la] > [!UICONTROL Herramientas] > [!UICONTROL General] > [[!UICONTROL Configuración de la traducción]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM Antes de la versión 6.3 {#prior-to-aem}

AEM En versiones anteriores de la versión de la versión de la traducción se actualizaban manualmente editando un archivo XML ubicado en el flujo de trabajo de traducción: `/etc/workflow/models/translation/translation_rules.xml`.

## Recursos adicionales {#additional-resources}

* [Identificando contenido para traducir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traducción de contenido para sitios multilingües](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Prácticas recomendadas de traducción](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)

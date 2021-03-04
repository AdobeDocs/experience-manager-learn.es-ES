---
title: Configuración de reglas de traducción en AEM
description: La interfaz de usuario de Configuración de traducción permite al usuario administrar las reglas para la traducción de contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.
feature: Copiar idioma
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localización
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 3%

---


# Configuración de reglas de traducción {#set-up-translation-rules-in-aem}

La interfaz de usuario de Configuración de traducción permite al usuario administrar las reglas para la traducción de contenido en AEM Sites. Este vídeo detalla la creación de una nueva regla de traducción para un componente personalizado.

>[!NOTE]
>
> El siguiente vídeo se grabó en AEM 6.3. AEM 6.4+ presenta una nueva estructura de repositorios para almacenar el archivo XML de reglas de traducción. Al utilizar la interfaz de usuario de Configuración de traducción en AEM 6.4+, las reglas se guardan en la ubicación `/conf/global/settings/translation/rules/translation_rules.xml`. Consulte [Identificación del contenido a la traducción](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) para obtener más información.

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

Las reglas de traducción identifican el contenido de AEM que se extraerá para su traducción. Las reglas de traducción predeterminadas abarcan casos de uso comunes, como componentes de texto y texto alternativo para componentes de imagen. Según los requisitos de traducción de un proyecto, pueden ser necesarias reglas adicionales. En las reglas generales de traducción, los usuarios pueden especificar:

1. Propiedades que deben traducirse en función de la ruta o el tipo de recurso
2. Filtros para propiedades que NO deben traducirse
3. Contenido al que se hace referencia y que debe traducirse (p. ej. Imágenes o fragmentos de contenido)

El editor de reglas de traducción que actualizará el archivo xml de traducción. La interfaz de usuario de Configuración de traducción facilita la administración de varias reglas de traducción y la protección contra errores tipográficos al editar XML directamente.

Acceda a la IU de configuración de traducción:

* **[!UICONTROL Menú]  Inicio de AEM >  [!UICONTROL Herramientas]  >  [!UICONTROL General]  > Configuración de la  [[!UICONTROL traducción]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Antes de AEM 6.3 {#prior-to-aem}

En versiones anteriores de AEM, las reglas de traducción se actualizaban manualmente editando un archivo XML ubicado en el flujo de trabajo de traducción: `/etc/workflow/models/translation/translation_rules.xml`.

## Recursos adicionales {#additional-resources}

* [Identificación del contenido para traducir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traducción de contenido para sitios multilingües](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Prácticas recomendadas de traducción](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)

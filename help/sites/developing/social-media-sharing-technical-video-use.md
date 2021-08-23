---
title: Uso compartido en medios sociales en AEM Sites
description: Explore la configuración y el uso del componente Uso compartido de medios sociales .
feature: Componentes principales
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: Administración de contenido
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 8%

---


# Uso compartido en medios sociales {#using-social-media-sharing-in-aem-sites}

Explore la configuración y el uso del componente Uso compartido de medios sociales .

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Este vídeo explora las siguientes instalaciones del componente de uso compartido de medios sociales (parte de [AEM componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)) mediante el sitio web de muestra de [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - Adición y configuración del componente Uso compartido de medios sociales
* 1:00 - Compartir con Facebook
* 3:10 - Uso compartido con Pinterest
* 6:25 - Uso del componente Uso compartido de medios sociales en una página de producto

## Configuración del externalizador {#externalizer-setup}

![Externalizador de vínculos de CQ de día](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) externalizador debe configurarse tanto en AEM Author como en AEM Publish para asignar el modo de ejecución de publicación al dominio accesible al público utilizado para acceder a AEM Publish.

En este vídeo utilizamos `/etc/hosts` para parpadear *www.example.com* para resolver en localhost, y utilizamos una [configuración básica AEM de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) para permitir que www.example.com enfrente AEM Publish.

## Materiales de apoyo {#supporting-materials}

* [Descargar los componentes principales AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Descargar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalación de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)

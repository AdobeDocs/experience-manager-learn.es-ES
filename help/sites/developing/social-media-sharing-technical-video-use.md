---
title: Uso compartido de medios sociales en AEM Sites
description: Explore la configuración y el uso del componente Compartir en redes sociales.
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 7%

---


# Uso compartido de medios sociales {#using-social-media-sharing-in-aem-sites}

Explore la configuración y el uso del componente Compartir en redes sociales.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

Este vídeo explora las siguientes funciones del componente Compartir en redes sociales (parte de [Componentes principales de AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)) mediante el sitio web de muestra [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00 - Añadir y configurar el componente Compartir en medios sociales
* 1:00 - Uso compartido en Facebook
* 3:10 - Compartir con Pinterest
* 6:25 - Uso del componente Compartir en redes sociales en una página de producto

## Configuración del externalizador {#externalizer-setup}

![Externalizador de vínculos de CQ de día](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) externalizador debe configurarse tanto en AEM Author como en AEM Publish para asignar el modo de ejecución de publicación al dominio público utilizado para acceder a AEM Publish.

En este vídeo, utilizamos `/etc/hosts` para parodiar *www.example.com* para resolver en localhost y utilizar una [configuración básica de AEM Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) para permitir que www.example.com envíe AEM Publish.

## Materiales de apoyo {#supporting-materials}

* [Descargar los componentes principales AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Descargar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalación de Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)

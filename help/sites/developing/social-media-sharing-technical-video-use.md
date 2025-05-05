---
title: Uso compartido en medios sociales en AEM Sites
description: Explore la configuración y el uso del componente Compartir en redes sociales.
feature: Core Components
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 2%

---

# Uso compartido en medios sociales {#using-social-media-sharing-in-aem-sites}

Explore la configuración y el uso del componente Compartir en redes sociales.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Este vídeo explora las siguientes funciones del componente Compartir en redes sociales (parte de [Componentes principales de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)) mediante el sitio web de muestra [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00: Agregar y configurar el componente Compartir en redes sociales
* 1:00: Compartir en Facebook
* 3:10: uso compartido en Pinterest
* 6:25: uso del componente Compartir en redes sociales en una página de producto

## Configuración del externalizador {#externalizer-setup}

![Externalizador de vínculos CQ por día](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

El externalizador de [AEM](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/externalizer.html) debe configurarse tanto en AEM Author como en AEM Publish para asignar el modo de ejecución de publicación al dominio de acceso público que se usa para acceder a AEM Publish.

En este vídeo utilizamos `/etc/hosts` para suplantar *www.example.com* y resolver en localhost, y usamos una [configuración básica de AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=es) para permitir que www.example.com inicie AEM Publish.

## Materiales de apoyo {#supporting-materials}

* [Descargar los componentes principales de AEM](https://github.com/adobe/aem-core-wcm-components/releases)
* [Descargar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalación de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=es)

---
title: Uso compartido en medios sociales en AEM Sites
description: Explore la configuración y el uso del componente Compartir en redes sociales.
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 6%

---

# Uso compartido en medios sociales {#using-social-media-sharing-in-aem-sites}

Explore la configuración y el uso del componente Compartir en redes sociales.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

Este vídeo explora las siguientes instalaciones del componente Compartir en redes sociales (parte de [AEM Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)) usando el [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) sitio web de ejemplo.

* 0:00: Agregar y configurar el componente Compartir en redes sociales
* 1:00: uso compartido en Facebook
* 3:10: uso compartido en Pinterest
* 6:25: uso del componente Compartir en redes sociales en una página de producto

## Configuración del externalizador {#externalizer-setup}

![Externalizador de vínculos CQ de día](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizador de](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) AEM AEM AEM debe configurarse tanto en el modo de ejecución de autor como en el modo de ejecución de publicación para asignar el modo de ejecución de publicación al dominio de acceso público utilizado para acceder a la publicación de la.

En este vídeo utilizamos `/etc/hosts` para falsificar *www.example.com* para resolver en localhost y utilice un [AEM configuración básica de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) AEM para permitir que www.example.com inicie la publicación de la.

## Materiales de apoyo {#supporting-materials}

* [AEM Descarga de los componentes principales de la](https://github.com/adobe/aem-core-wcm-components/releases)
* [Descargar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalación de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)

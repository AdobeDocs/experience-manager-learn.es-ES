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
duration: 511
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 2%

---

# Uso compartido en medios sociales {#using-social-media-sharing-in-aem-sites}

Explore la configuración y el uso del componente Compartir en redes sociales.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

AEM Este vídeo explora los siguientes servicios del componente Compartir en redes sociales (parte de [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)) mediante el sitio web de muestra [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail).

* 0:00: Agregar y configurar el componente Compartir en redes sociales
* 1:00: uso compartido en Facebook
* 3:10: uso compartido en Pinterest
* 6:25: uso del componente Compartir en redes sociales en una página de producto

## Configuración del externalizador {#externalizer-setup}

![Externalizador de vínculos CQ por día](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

AEM AEM AEM [El externalizador ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) de la publicación debe configurarse tanto en el Autor de la publicación como en el Publish AEM de la para asignar el modo de ejecución de la publicación al dominio de acceso público utilizado para acceder a la Publish de la publicación de la publicación de la publicación de la publicación de la publicación de la publicación de acceso a la aplicación de acceso público que se utiliza para acceder a la aplicación de acceso a la página de acceso a la aplicación de la.

AEM En este vídeo usamos `/etc/hosts` para suplantar *www.example.com* y resolver en localhost, y usamos una [configuración básica de Dispatcher AEM](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) para permitir que www.example.com enfrente a Publish..

## Materiales de apoyo {#supporting-materials}

* AEM [Descargar los componentes principales de la](https://github.com/adobe/aem-core-wcm-components/releases)
* [Descargar We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Instalación de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)

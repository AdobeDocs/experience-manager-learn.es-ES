---
title: 'Capítulo 1: Configuración y descargas del tutorial: Servicios de contenido'
description: AEM AEM El capítulo 1 del tutorial sin encabezado de la configuración de línea de base para la instancia de la del tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 1%

---

# Configuración del tutorial

AEM AEM Siempre se recomienda la versión más reciente de los componentes principales de WCM de y.

* AEM.5 o posterior
* AEM Componentes principales de WCM 2.4.0 o posterior
   * AEM Incluido en el [paquete de contenido de la aplicación WKND Mobile a continuación](#wknd-mobile-application-packages)

AEM Antes de iniciar este tutorial, asegúrese de que las siguientes instancias de la estén [instaladas y ejecutándose en el equipo local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* AEM **Autor de** en el **puerto 4502**
* AEM **Publish** en el **puerto 4503**

## Paquetes de aplicación móvil de WKND{#wknd-mobile-application-packages}

AEM AEM AEM Instale los siguientes paquetes de contenido de la en **tanto** como en Autor de la y Publish de la aplicación, usando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * AEM Componente proxy [!DNL WKND Mobile] para componentes principales de WCM de la
   * AEM [!DNL WKND Mobile]: CSS de las páginas de servicios de contenido de la (para un estilo menor)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] estructura del sitio
   * Estructura de carpetas DAM [!DNL WKND Mobile]
   * [!DNL WKND Mobile] recursos de imagen

En el [Capítulo 7](./chapter-7.md) ejecutaremos la aplicación móvil de Android [!DNL WKND Mobile] usando [Android Studio](https://developer.android.com/studio) y el APK proporcionado (Paquete de aplicación de Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## AEM Paquetes de contenido de capítulo

Este conjunto de paquetes de contenido crea el contenido y la configuración descritos en el capítulo asociado, así como todos los capítulos anteriores. Estos paquetes son opcionales, pero pueden acelerar la creación de contenido.

* [Contenido del capítulo 2: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenido del capítulo 3: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenido del capítulo 4: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Contenido del capítulo 5: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código fuente

AEM El código fuente tanto del proyecto de como de [!DNL Android Mobile App] está disponible en [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). No es necesario crear o modificar el código fuente para este tutorial, se proporciona para permitir una total transparencia en la forma en que se crean todos los aspectos del tutorial.

Si encuentra algún problema con el tutorial o el código, deje un [problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Pasar al final

AEM AEM Publish Para saltar al final del tutorial, el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) se puede instalar en **tanto en** Author como en . AEM AEM Tenga en cuenta que el contenido y la configuración no se mostrarán como publicados en Autor, sin embargo, debido a la implementación manual, todo el contenido y la configuración necesarios están disponibles en Publish, lo que permite que [!DNL WKND Mobile App] acceda al contenido.


## Siguiente paso

* [Capítulo 2: Definición de modelos de fragmentos de contenido de evento](./chapter-2.md)

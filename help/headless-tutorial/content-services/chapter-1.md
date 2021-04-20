---
title: Capítulo 1 - Configuración y descargas de tutoriales - Servicios de contenido
seo-title: Introducción a los servicios de contenido de AEM - Capítulo 1 - Configuración de tutoriales
description: Capítulo 1 del tutorial AEM Headless en la configuración de línea de base de la instancia AEM para el tutorial.
seo-description: Capítulo 1 del tutorial AEM Headless en la configuración de línea de base de la instancia AEM para el tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Configuración del tutorial

Siempre se recomienda la versión más reciente de los componentes principales de AEM y AEM WCM.

* AEM 6.5 o posterior
* Componentes principales de AEM WCM 2.4.0 o posterior
   * Incluido en el [Paquete de contenido de aplicación móvil de AEM de WKND a continuación](#wknd-mobile-application-packages)

Antes de iniciar este tutorial, asegúrese de que las siguientes instancias de AEM estén [instaladas y se ejecuten en su equipo local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **Puerto** de autorización de AEM  **4502**
* **Puerto de AEM** Publishing  **4503**

## Paquetes de aplicaciones móviles WKND{#wknd-mobile-application-packages}

Instale los siguientes paquetes de contenido de AEM en **tanto** AEM Author como AEM Publish, mediante [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy para los componentes principales de WCM de AEM
   * [!DNL WKND Mobile] CSS de las páginas de servicios de contenido de AEM (para estilos menores)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estructura del sitio
   * [!DNL WKND Mobile] Estructura de carpetas DAM
   * [!DNL WKND Mobile] recursos de imagen

En [Capítulo 7](./chapter-7.md) ejecutaremos la aplicación móvil de [!DNL WKND Mobile] Android mediante [Android Studio](https://developer.android.com/studio) y la APK (paquete de aplicación de Android) proporcionada:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Paquetes de contenido de AEM de capítulo

Este conjunto de paquetes de contenido crea contenido y configuración descritos en el capítulo asociado, y todos los capítulos anteriores. Estos paquetes son opcionales, pero pueden acelerar la creación de contenido.

* [Capítulo 2 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 3 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 4 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 5 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código fuente

El código fuente tanto para el proyecto AEM como para el [!DNL Android Mobile App] están disponibles en el [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). No es necesario crear o modificar el código fuente para este tutorial, se proporciona para permitir una total transparencia en la forma en que se crean todos los aspectos del tutorial.

Si encuentra algún problema con el tutorial o el código, deje un [problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Pasar al final

Para poder pasar al final del tutorial, el paquete de contenido [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) se puede instalar en **tanto** AEM Author como AEM Publish. Tenga en cuenta que el contenido y la configuración no se mostrarán como publicados en AEM Author; sin embargo, debido a la implementación manual, todo el contenido y la configuración necesarios estarán disponibles en AEM Publish, lo que permite que [!DNL WKND Mobile App] acceda al contenido.


## Paso siguiente

* [Capítulo 2 - Definición de modelos de fragmentos de contenido de eventos](./chapter-2.md)

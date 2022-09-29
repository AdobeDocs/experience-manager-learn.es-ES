---
title: Capítulo 1 - Configuración y descargas de tutoriales - Servicios de contenido
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: Capítulo 1 del tutorial AEM sin encabezado , la configuración de línea de base para la instancia de AEM del tutorial.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 1%

---

# Configuración del tutorial

Siempre se recomienda la versión más reciente de AEM y AEM componentes principales de WCM.

* AEM 6.5 o posterior
* AEM Componentes principales de WCM 2.4.0 o posterior
   * Incluido en el [Paquete de contenido de aplicación de AEM móvil WKND a continuación](#wknd-mobile-application-packages)

Antes de iniciar este tutorial, asegúrese de que las siguientes instancias de AEM son [instalado y en ejecución en el equipo local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **Autor de AEM** en **puerto 4502**
* **AEM Publish** en **puerto 4503**

## Paquetes de aplicaciones móviles WKND{#wknd-mobile-application-packages}

Instale los siguientes paquetes de contenido AEM en **both** AEM Author y AEM Publish, utilizando [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy para AEM componentes principales de WCM
   * [!DNL WKND Mobile] CSS de las páginas de servicios de contenido de AEM (para estilos menores)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estructura del sitio
   * [!DNL WKND Mobile] Estructura de carpetas DAM
   * [!DNL WKND Mobile] recursos de imagen

En [Capítulo 7](./chapter-7.md) ejecutaremos el [!DNL WKND Mobile] Aplicación móvil de Android que utiliza [Android Studio](https://developer.android.com/studio) y el APK proporcionado (Android Application Package):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Paquetes de contenido AEM capítulo

Este conjunto de paquetes de contenido crea contenido y configuración descritos en el capítulo asociado, y todos los capítulos anteriores. Estos paquetes son opcionales, pero pueden acelerar la creación de contenido.

* [Capítulo 2 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 3 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 4 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 5 Contenido: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código fuente

El código fuente tanto para el proyecto de AEM como para el [!DNL Android Mobile App] están disponibles en la [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). No es necesario crear o modificar el código fuente para este tutorial, se proporciona para permitir una total transparencia en la forma en que se crean todos los aspectos del tutorial.

Si encuentra algún problema con el tutorial o el código, deje un [Problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Pasar al final

Para saltar al final del tutorial, la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) el paquete de contenido se puede instalar en **both** Autor de AEM y AEM Publish. Tenga en cuenta que el contenido y la configuración no se mostrarán como publicados en AEM Author; sin embargo, debido a la implementación manual, todo el contenido y la configuración necesarios están disponibles en AEM Publish, lo que permite que [!DNL WKND Mobile App] para acceder al contenido.


## Paso siguiente

* [Capítulo 2 - Definición de modelos de fragmentos de contenido de eventos](./chapter-2.md)

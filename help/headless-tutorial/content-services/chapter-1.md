---
title: 'Capítulo 1: Configuración y descargas del tutorial: Servicios de contenido'
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: AEM AEM El capítulo 1 del tutorial sin encabezado de la configuración de línea de base para la instancia de la del tutorial.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 2%

---

# Configuración del tutorial

AEM AEM Siempre se recomienda la versión más reciente de los componentes principales de WCM de y.

* AEM 6.5 o posterior
* AEM Componentes principales de WCM 2.4.0 o posterior
   * Incluido en la [AEM Paquete de contenido de la aplicación WKND Mobile a continuación](#wknd-mobile-application-packages)

AEM Antes de iniciar este tutorial, asegúrese de que las siguientes instancias de la sean [instalado y en ejecución en el equipo local](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM Author** el **puerto 4502**
* **AEM Publish** el **puerto 4503**

## Paquetes de aplicación móvil de WKND{#wknd-mobile-application-packages}

AEM Instale los siguientes paquetes de contenido de la en **ambos** AEM Author y AEM Publish, con [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM Componente proxy para los componentes principales de WCM de
   * [!DNL WKND Mobile] AEM CSS de las páginas de servicios de contenido (para estilo menor)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estructura del sitio
   * [!DNL WKND Mobile] Estructura de carpetas DAM
   * [!DNL WKND Mobile] recursos de imagen

Entrada [Capítulo 7](./chapter-7.md) vamos a ejecutar el [!DNL WKND Mobile] Aplicación móvil de Android con [Android Studio](https://developer.android.com/studio) y el APK proporcionado (paquete de aplicación de Android):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## AEM Paquetes de contenido de capítulo

Este conjunto de paquetes de contenido crea el contenido y la configuración descritos en el capítulo asociado, así como todos los capítulos anteriores. Estos paquetes son opcionales, pero pueden acelerar la creación de contenido.

* [Capítulo 2 Contenido: GitHub > Recursos > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 3 Contenido: GitHub > Recursos > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 4 Contenido: GitHub > Recursos > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 5 Contenido: GitHub > Recursos > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código fuente

AEM El código fuente tanto del proyecto de la como del [!DNL Android Mobile App] están disponibles en la [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). No es necesario crear o modificar el código fuente para este tutorial, se proporciona para permitir una total transparencia en la forma en que se crean todos los aspectos del tutorial.

Si encuentra algún problema con el tutorial o el código, deje un [Problema de GitHub](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Pasar al final

Para saltar al final del tutorial, la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) el paquete de contenido se puede instalar en **ambos** AEM Author y AEM Publish. Tenga en cuenta que el contenido y la configuración no se mostrarán como se publican en AEM Author; sin embargo, debido a la implementación manual, todo el contenido y la configuración necesarios están disponibles en AEM Publish, lo que permite que [!DNL WKND Mobile App] para acceder al contenido.


## Siguiente paso

* [Capítulo 2: Definición de modelos de fragmentos de contenido de evento](./chapter-2.md)

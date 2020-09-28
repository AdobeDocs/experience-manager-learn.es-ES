---
title: Capítulo 1 - Configuración y descargas de tutoriales
seo-title: Introducción a los servicios de contenido de AEM - Capítulo 1 - Configuración de tutoriales
description: Capítulo 1 del tutorial sin encabezado de AEM la configuración de la línea de base de la instancia de AEM para el tutorial.
seo-description: Capítulo 1 del tutorial sin encabezado de AEM la configuración de la línea de base de la instancia de AEM para el tutorial.
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---


# Configuración de tutoriales

Siempre se recomienda la versión más reciente de AEM y AEM componentes principales de WCM.

* AEM 6.5 o posterior
* Componentes principales de AEM WCM 2.4.0 o posterior
   * Incluido en el paquete de contenido de la aplicación de [WKND Mobile AEM a continuación](#wknd-mobile-application-packages)

Antes de iniciar este tutorial, asegúrese de que las siguientes instancias de AEM están [instaladas y ejecutándose en el equipo](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)local:

* **AEM Author** en el **puerto 4502**
* **AEM Publish** en el **puerto 4503**

## Paquetes de aplicaciones móviles WKND{#wknd-mobile-application-packages}

Instale los siguientes paquetes de contenido AEM en AEM Author **y AEM Publish, mediante** [!DNL AEM Package Manager].

* [ui.apps: GitHub > Recursos > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Componente proxy para componentes principales AEM WCM
   * [!DNL WKND Mobile] CSS de las páginas de Servicios de contenido de AEM (para estilos menores)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Estructura del sitio
   * [!DNL WKND Mobile] Estructura de carpetas DAM
   * [!DNL WKND Mobile] recursos de imagen

En el [capítulo 7](./chapter-7.md) ejecutaremos la aplicación móvil de [!DNL WKND Mobile] Android mediante [Android Studio](https://developer.android.com/studio) y la APK proporcionada (Paquete de aplicaciones de Android):

* [Aplicación móvil de [!DNL Android: GitHub > Recursos > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Capítulo AEM Paquetes de contenido

Este conjunto de paquetes de contenido crea el contenido y la configuración descritos en el capítulo asociado y en todos los capítulos anteriores. Estos paquetes son opcionales pero pueden acelerar la creación de contenido.

* [Capítulo 2 Contenido: GitHub > Recursos > com.adobe.aem.guide.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 3 Contenido: GitHub > Recursos > com.adobe.aem.guide.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 4 Contenido: GitHub > Recursos > com.adobe.aem.guide.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Capítulo 5 Contenido: GitHub > Recursos > com.adobe.aem.guide.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Código de origen

El código fuente tanto para el proyecto AEM como para el [!DNL Android Mobile App] están disponibles en las [[!DNL AEM Guías - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). El código fuente no necesita crearse ni modificarse para este tutorial, se proporciona para permitir una total transparencia en la forma en que se crean todos los aspectos del tutorial.

Si encuentras algún problema con el tutorial o el código, por favor deja un problema [con](https://github.com/adobe/aem-guides-wknd-mobile/issues)GitHub.

## Saltar al final

Para pasar al final del tutorial, el paquete de contenido [com.adobe.aem.guide.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) se puede instalar en AEM Author **y AEM Publish tanto** como en AEM Publish. Tenga en cuenta que el contenido y la configuración no se mostrarán tal y como se publicaron en AEM Author; sin embargo, debido a la implementación manual, todo el contenido y la configuración necesarios estarán disponibles en AEM Publish, lo que permitirá [!DNL WKND Mobile App] que el usuario tenga acceso al contenido.


## Paso siguiente

* [Capítulo 2 - Definición de modelos de fragmentos de contenido de Evento](./chapter-2.md)

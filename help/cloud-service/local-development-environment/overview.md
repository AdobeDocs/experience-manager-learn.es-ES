---
title: Entorno de desarrollo local para AEM as a Cloud Service
description: Resumen del entorno de desarrollo local de Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 2%

---

# Configuración del entorno de desarrollo local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Información general"
>abstract="La configuración del entorno de desarrollo local para AEM as a Cloud Service incluye las herramientas de desarrollo necesarias para desarrollar, crear y compilar AEM Proyectos, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service mediante Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=es" text="Directrices de desarrollo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Conceptos básicos de desarrollo"

Este tutorial explica la configuración de un entorno de desarrollo local para Adobe Experience Manager (AEM) mediante el SDK as a Cloud Service de AEM. Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar AEM Proyectos, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas para AEM as a Cloud Service mediante Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM pila de tecnología de entorno de desarrollo local as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

El entorno de desarrollo local para AEM puede dividirse en tres grupos lógicos:

+ La variable __AEM proyecto__ contiene el código personalizado, la configuración y el contenido que es la aplicación de AEM personalizada.
+ La variable __Tiempo de ejecución de AEM local__ que ejecuta una versión local de los servicios de AEM Author y Publish localmente.
+ La variable __Tiempo de ejecución de Dispatcher local__ que ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

Este tutorial explica cómo instalar y configurar los elementos resaltados en el diagrama anterior, proporcionando un entorno de desarrollo local estable para AEM desarrollo.

## Organización del sistema de archivos

Este tutorial estableció la ubicación de los artefactos as a Cloud Service AEM SDK y AEM código de proyecto de la siguiente manera:

+ `~/aem-sdk` es una carpeta organizativa que contiene las distintas herramientas proporcionadas por el SDK as a Cloud Service de AEM
+ `~/aem-sdk/author` contiene el servicio de autor de AEM
+ `~/aem-sdk/publish` contiene el servicio de publicación de AEM
+ `~/aem-sdk/dispatcher` contiene las herramientas de Dispatcher
+ `~/code/<project name>` contiene el código fuente del proyecto AEM personalizado

Tenga en cuenta que `~` es abreviatura del Directorio del usuario. En Windows, es el equivalente de `%HOMEPATH%`;

## Herramientas de desarrollo para proyectos AEM

El proyecto AEM es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager para AEM as a Cloud Service. La estructura del proyecto de línea de base se genera mediante la variable [Tipo de archivo Maven del proyecto AEM](https://github.com/adobe/aem-project-archetype).

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configuración de herramientas de desarrollo para proyectos AEM](./development-tools.md)

## Tiempo de ejecución de AEM local

El SDK as a Cloud Service AEM proporciona un [!DNL QuickStart Jar] que ejecuta una versión local de AEM. La variable [!DNL QuickStart Jar] se puede utilizar para ejecutar el servicio de autor de AEM o el servicio de publicación de AEM localmente. Tenga en cuenta que mientras que la variable [!DNL QuickStart Jar] proporciona una experiencia de desarrollo local, no todas las funciones disponibles en AEM as a Cloud Service están incluidas en la variable [!DNL QuickStart Jar].

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Descargar el SDK de AEM
+ Ejecute el [!DNL AEM Author Service]
+ Ejecute el [!DNL AEM Publish Service]

[Configuración del tiempo de ejecución de Local AEM](./aem-runtime.md)

## Local [!DNL Dispatcher] Tiempo de ejecución

AEM herramientas de Dispatcher del SDK as a Cloud Service proporciona todo lo necesario para configurar el [!DNL Dispatcher] tiempo de ejecución. [!DNL Dispatcher] Las herramientas [!DNL Docker]basado en y proporciona herramientas de línea de comandos para transpilar [!DNL Apache HTTP] Servidor web y [!DNL Dispatcher] archivos de configuración en formatos compatibles e implementarlos en [!DNL Dispatcher] que se ejecuta en la variable [!DNL Docker] contenedor.

Esta sección del tutorial muestra cómo:

+ Descargar el SDK de AEM
+ Instalar [!DNL Dispatcher] Herramientas
+ Ejecute el [!DNL Dispatcher] tiempo de ejecución

[Configure el [!DNL Dispatcher] Tiempo de ejecución](./dispatcher-tools.md)

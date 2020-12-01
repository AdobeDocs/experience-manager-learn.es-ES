---
title: Entorno de desarrollo local para AEM como Cloud Service
description: Información general del entorno de desarrollo local de Adobe Experience Manager (AEM).
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---


# Configuración del Entorno de desarrollo local

Este tutorial explica cómo configurar un entorno de desarrollo local para Adobe Experience Manager (AEM) mediante el uso del AEM como SDK de Cloud Service. Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar AEM proyectos, así como los tiempos de ejecución locales, que permiten a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas para AEM como Cloud Service mediante Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM como Cloud Service de tecnología de Entorno de desarrollo local](./assets/overview/aem-sdk-technology-stack.png)

El entorno de desarrollo local para AEM puede dividirse en tres grupos lógicos:

+ El __proyecto de AEM__ contiene el código personalizado, la configuración y el contenido que es la aplicación AEM personalizada.
+ El __AEM local en tiempo de ejecución__ que ejecuta una versión local de los servicios de AEM Author y Publish localmente.
+ __Local Dispatcher Runtime__ que ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

Este tutorial explica cómo instalar y configurar los elementos resaltados en el diagrama anterior, proporcionando un entorno de desarrollo local estable para AEM desarrollo.

## Organización del sistema de archivos

Este tutorial estableció la ubicación del AEM como artefactos del SDK Cloud Service y AEM código de proyecto de la siguiente manera:

+ `~/aem-sdk` es una carpeta de organización que contiene las distintas herramientas proporcionadas por el AEM como SDK de Cloud Service
+ `~/aem-sdk/author` contiene el servicio AEM Author
+ `~/aem-sdk/publish` contiene el servicio de AEM Publish
+ `~/aem-sdk/dispatcher` contiene las herramientas de Dispatcher
+ `~/code/<project name>` contiene el código fuente AEM Project personalizado

Tenga en cuenta que `~` es la abreviatura del Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`;

## Herramientas de desarrollo para proyectos AEM

El proyecto AEM es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager para AEM como Cloud Service. La estructura del proyecto de base se genera mediante el [AEM Arquetipo Maven del proyecto](https://github.com/adobe/aem-project-archetype).

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar herramientas de desarrollo para proyectos AEM](./development-tools.md)

## AEM en tiempo de ejecución local

El SDK de AEM como Cloud Service proporciona una [!DNL QuickStart Jar] que ejecuta una versión local de AEM. El [!DNL QuickStart Jar] se puede utilizar para ejecutar el servicio AEM Author o AEM Publish Service localmente. Tenga en cuenta que si bien el [!DNL QuickStart Jar] proporciona una experiencia de desarrollo local, no todas las funciones disponibles en AEM como Cloud Service se incluyen en el [!DNL QuickStart Jar].

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Descargar el SDK de AEM
+ Ejecute el [!DNL AEM Author Service]
+ Ejecute el [!DNL AEM Publish Service]

[Configuración del tiempo de ejecución de AEM local](./aem-runtime.md)

## Tiempo de ejecución local [!DNL Dispatcher]

AEM como herramientas de despachante del SDK de un Cloud Service proporciona todo lo necesario para configurar el tiempo de ejecución [!DNL Dispatcher] local. [!DNL Dispatcher] Las herramientas están  [!DNL Docker]basadas en y proporcionan herramientas de línea de comandos para convertir  [!DNL Apache HTTP] Web Server y archivos  [!DNL Dispatcher] de configuración en formatos compatibles e implementarlos para  [!DNL Dispatcher] ejecutarlos en el  [!DNL Docker] contenedor.

Esta sección del tutorial muestra cómo:

+ Descargar el SDK de AEM
+ Instalar [!DNL Dispatcher] herramientas
+ Ejecutar el tiempo de ejecución local [!DNL Dispatcher]

[Configuración de  [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)

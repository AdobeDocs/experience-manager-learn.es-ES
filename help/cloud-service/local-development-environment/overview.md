---
title: Entorno de desarrollo local para AEM as a Cloud Service
description: Información general sobre el entorno de desarrollo local de Adobe Experience Manager (AEM).
feature: Developer Tools
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 2%

---


# Configuración del entorno de desarrollo local

Este tutorial explica la configuración de un entorno de desarrollo local para Adobe Experience Manager (AEM) mediante el SDK de AEM as a Cloud Service. Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar proyectos AEM, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service a través de Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![Apilamiento de tecnología de entorno de desarrollo local de AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

El entorno de desarrollo local para AEM se puede dividir en tres grupos lógicos:

+ El __proyecto AEM__ contiene el código, la configuración y el contenido personalizados que es la aplicación AEM personalizada.
+ __Tiempo de ejecución local de AEM__ que ejecuta una versión local de los servicios de AEM Author y Publish localmente.
+ __Local Dispatcher Runtime__ que ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

Este tutorial explica cómo instalar y configurar los elementos resaltados en el diagrama anterior, proporcionando un entorno de desarrollo local estable para el desarrollo de AEM.

## Organización del sistema de archivos

Este tutorial estableció la ubicación de los artefactos del SDK de AEM as a Cloud Service y el código del proyecto de AEM de la siguiente manera:

+ `~/aem-sdk` es una carpeta de organización que contiene las distintas herramientas proporcionadas por el SDK de AEM as a Cloud Service
+ `~/aem-sdk/author` contiene el servicio de autor de AEM
+ `~/aem-sdk/publish` contiene el servicio de publicación de AEM
+ `~/aem-sdk/dispatcher` contiene las herramientas de Dispatcher
+ `~/code/<project name>` contiene el código fuente del proyecto AEM personalizado

Tenga en cuenta que `~` es abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`;

## Herramientas de desarrollo para proyectos de AEM

El proyecto de AEM es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager en AEM as a Cloud Service. La estructura del proyecto de línea de base se genera mediante el [Tipo de archivo Maven del proyecto AEM](https://github.com/adobe/aem-project-archetype).

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configuración de herramientas de desarrollo para proyectos de AEM](./development-tools.md)

## Tiempo de ejecución local de AEM

El SDK de AEM as a Cloud Service proporciona una [!DNL QuickStart Jar] que ejecuta una versión local de AEM. El [!DNL QuickStart Jar] se puede utilizar para ejecutar el servicio de AEM Author o el servicio de AEM Publish localmente. Tenga en cuenta que aunque [!DNL QuickStart Jar] proporciona una experiencia de desarrollo local, no todas las funciones disponibles en AEM as a Cloud Service se incluyen en [!DNL QuickStart Jar].

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Descargar el SDK de AEM
+ Ejecute el [!DNL AEM Author Service]
+ Ejecute el [!DNL AEM Publish Service]

[Configuración del tiempo de ejecución de AEM local](./aem-runtime.md)

## Tiempo de ejecución [!DNL Dispatcher] local

Dispatcher Tools del SDK de AEM as a Cloud Service proporciona todo lo necesario para configurar el tiempo de ejecución local [!DNL Dispatcher]. [!DNL Dispatcher] Las herramientas están  [!DNL Docker]basadas en y proporcionan herramientas de línea de comandos para transformar archivos de servidor  [!DNL Apache HTTP] web y de  [!DNL Dispatcher] configuración en formatos compatibles e implementarlos para  [!DNL Dispatcher] ejecutarlos en el  [!DNL Docker] contenedor.

Esta sección del tutorial muestra cómo:

+ Descargar el SDK de AEM
+ Instalar herramientas [!DNL Dispatcher]
+ Ejecute el tiempo de ejecución local [!DNL Dispatcher]

[Configuración de  [!DNL Dispatcher] LocalRuntime](./dispatcher-tools.md)

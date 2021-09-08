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
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 1%

---


# Configuración del entorno de desarrollo local

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Información general"
>abstract="La configuración del entorno de desarrollo local para AEM as a Cloud Service incluye las herramientas de desarrollo necesarias para desarrollar, crear y compilar AEM Proyectos, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM como Cloud Service a través de Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Directrices de desarrollo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Conceptos básicos de desarrollo"

Este tutorial explica la configuración de un entorno de desarrollo local para Adobe Experience Manager (AEM) mediante el SDK de AEM as a Cloud Service. Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar AEM Proyectos, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas para AEM como Cloud Service a través de Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM como Cloud Service de desarrollo local, pila de tecnología ambiental](./assets/overview/aem-sdk-technology-stack.png)

El entorno de desarrollo local para AEM puede dividirse en tres grupos lógicos:

+ El __AEM proyecto__ contiene el código, la configuración y el contenido personalizados que es la aplicación de AEM personalizada.
+ __Local AEM Runtime__ que ejecuta una versión local de AEM Author y Publish Services localmente.
+ __Local Dispatcher Runtime__ que ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

Este tutorial explica cómo instalar y configurar los elementos resaltados en el diagrama anterior, proporcionando un entorno de desarrollo local estable para AEM desarrollo.

## Organización del sistema de archivos

Este tutorial estableció la ubicación del AEM como artefactos de SDK de Cloud Service y AEM código de proyecto de la siguiente manera:

+ `~/aem-sdk` es una carpeta organizativa que contiene las distintas herramientas proporcionadas por el SDK de AEM as a Cloud Service
+ `~/aem-sdk/author` contiene el servicio de autor de AEM
+ `~/aem-sdk/publish` contiene el servicio de publicación de AEM
+ `~/aem-sdk/dispatcher` contiene las herramientas de Dispatcher
+ `~/code/<project name>` contiene el código fuente del proyecto AEM personalizado

Tenga en cuenta que `~` es abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`;

## Herramientas de desarrollo para proyectos AEM

El proyecto AEM es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager para AEM como Cloud Service. La estructura del proyecto de línea de base se genera mediante el [AEM tipo de archivo Maven del proyecto](https://github.com/adobe/aem-project-archetype).

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configuración de herramientas de desarrollo para proyectos AEM](./development-tools.md)

## Tiempo de ejecución de AEM local

El SDK de AEM as a Cloud Service proporciona un [!DNL QuickStart Jar] que ejecuta una versión local de AEM. El [!DNL QuickStart Jar] se puede utilizar para ejecutar el servicio de AEM Author o el servicio de AEM Publish localmente. Tenga en cuenta que aunque [!DNL QuickStart Jar] proporciona una experiencia de desarrollo local, no todas las funciones disponibles en AEM como Cloud Service se incluyen en [!DNL QuickStart Jar].

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Descargar el SDK de AEM
+ Ejecute el [!DNL AEM Author Service]
+ Ejecute el [!DNL AEM Publish Service]

[Configuración del tiempo de ejecución de Local AEM](./aem-runtime.md)

## Tiempo de ejecución [!DNL Dispatcher] local

AEM as a Cloud Service SDK&#39;s Dispatcher Tools proporciona todo lo necesario para configurar el tiempo de ejecución local [!DNL Dispatcher]. [!DNL Dispatcher] Las herramientas están  [!DNL Docker]basadas en y proporcionan herramientas de línea de comandos para transformar archivos de servidor  [!DNL Apache HTTP] web y de  [!DNL Dispatcher] configuración en formatos compatibles e implementarlos para  [!DNL Dispatcher] ejecutarlos en el  [!DNL Docker] contenedor.

Esta sección del tutorial muestra cómo:

+ Descargar el SDK de AEM
+ Instalar herramientas [!DNL Dispatcher]
+ Ejecute el tiempo de ejecución local [!DNL Dispatcher]

[Configuración del tiempo de ejecución local [!DNL Dispatcher] ](./dispatcher-tools.md)

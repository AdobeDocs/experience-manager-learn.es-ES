---
title: AEM Entorno de desarrollo local para el as a Cloud Service de la
description: Información general sobre el entorno de desarrollo local de Adobe Experience Manager AEM ().
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 15%

---

# Configuración del entorno de desarrollo local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Información general"
>abstract="La configuración del entorno de desarrollo local para AEM as a Cloud Service incluye las herramientas de desarrollo necesarias para desarrollar, crear y compilar Proyectos AEM, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service mediante Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=es" text="Directrices de desarrollo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=es" text="Conceptos básicos de desarrollo"

En este tutorial se explica la configuración de un entorno de desarrollo local para Adobe Experience Manager AEM AEM as a Cloud Service () mediante el SDK de. AEM AEM Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar proyectos de, así como los tiempos de ejecución locales que permiten a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en el as a Cloud Service mediante Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM Paquete de tecnología de entorno de desarrollo local as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

AEM El entorno de desarrollo local para los se puede dividir en tres grupos lógicos:

+ El __AEM Proyecto de__ AEM contiene el código personalizado, la configuración y el contenido que es la aplicación de la aplicación de la personalizada.
+ El __AEM Tiempo de ejecución de__ AEM que ejecuta una versión local de los servicios de Autor y Publicación de manera local.
+ El __Dispatcher Runtime local__ que ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

AEM Este tutorial explica cómo instalar y configurar los elementos resaltados en el diagrama anterior, lo que proporciona un entorno de desarrollo local estable para el desarrollo de la.

## Organización del sistema de archivos

AEM En este tutorial se ha establecido la ubicación de los artefactos as a Cloud Service AEM del SDK y el código de proyecto de la forma siguiente:

+ `~/aem-sdk` AEM es una carpeta organizativa que contiene las distintas herramientas proporcionadas por el SDK as a Cloud Service de
+ `~/aem-sdk/author` AEM contiene el servicio de autor de
+ `~/aem-sdk/publish` AEM contiene el servicio de publicación de
+ `~/aem-sdk/dispatcher` contiene las herramientas de Dispatcher
+ `~/code/<project name>` AEM contiene el código fuente del proyecto de la aplicación personalizado

Tenga en cuenta que `~` es la abreviatura de Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`;

## AEM Herramientas de desarrollo para proyectos de

AEM AEM El proyecto de es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager para que los usuarios se sientan as a Cloud Service en el proceso de implementación de la aplicación. La estructura del proyecto de línea de base se genera mediante la variable [AEM Arquetipo del proyecto Maven](https://github.com/adobe/aem-project-archetype).

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[AEM Configuración de herramientas de desarrollo para proyectos de](./development-tools.md)

## Tiempo de ejecución local de AEM

AEM El SDK as a Cloud Service proporciona un [!DNL QuickStart Jar] AEM que ejecuta una versión local de. El [!DNL QuickStart Jar] AEM AEM se puede utilizar para ejecutar el servicio de autor de la o el servicio de publicación de la aplicación de forma local. Tenga en cuenta que mientras que la variable [!DNL QuickStart Jar] AEM proporciona una experiencia de desarrollo local, pero no todas las funciones disponibles en el as a Cloud Service se incluyen en la [!DNL QuickStart Jar].

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ AEM Descarga del SDK de
+ Ejecute el [!DNL AEM Author Service]
+ Ejecute el [!DNL AEM Publish Service]

[AEM Configuración del tiempo de ejecución de Local](./aem-runtime.md)

## Local [!DNL Dispatcher] Runtime

AEM Las herramientas de Dispatcher del SDK as a Cloud Service proporcionan todo lo necesario para configurar el SDK local [!DNL Dispatcher] runtime. [!DNL Dispatcher] Las herramientas son [!DNL Docker]basado en y proporciona herramientas de línea de comandos para transformar [!DNL Apache HTTP] Servidor web y [!DNL Dispatcher] archivos de configuración en formatos compatibles e implementarlos en [!DNL Dispatcher] ejecución en [!DNL Docker] contenedor.

Esta sección del tutorial muestra cómo:

+ AEM Descarga del SDK de
+ Instalar [!DNL Dispatcher] Herramientas
+ Ejecutar el local [!DNL Dispatcher] runtime

[Configure el Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)

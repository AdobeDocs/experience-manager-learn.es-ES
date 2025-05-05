---
title: Entorno de desarrollo local para AEM as a Cloud Service
description: Información general sobre el entorno de desarrollo local de Adobe Experience Manager (AEM).
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 12%

---

# Configuración del entorno de desarrollo local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Información general"
>abstract="La configuración del entorno de desarrollo local para AEM as a Cloud Service incluye las herramientas de desarrollo necesarias para desarrollar, crear y compilar Proyectos AEM, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service mediante Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=es" text="Directrices de desarrollo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=es" text="Conceptos básicos de desarrollo"

Este tutorial explica la configuración de un entorno de desarrollo local para Adobe Experience Manager (AEM) mediante AEM as a Cloud Service SDK. Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar proyectos de AEM, así como los tiempos de ejecución locales que permiten a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service a través de Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/36463?quality=12&learn=on&captions=spa)

![Pila de tecnología del entorno de desarrollo local de AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

El entorno de desarrollo local de AEM se puede dividir en tres grupos lógicos:

+ El __proyecto AEM__ contiene código, configuración y contenido personalizados que son la aplicación AEM personalizada.
+ El __tiempo de ejecución local de AEM__ que ejecuta una versión local de los servicios de AEM Author y Publish localmente.
+ __Tiempo de ejecución local de Dispatcher__ que ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

Este tutorial explica cómo instalar y configurar los elementos destacados en el diagrama anterior, lo que proporciona un entorno de desarrollo local estable para el desarrollo de AEM.

## Organización del sistema de archivos

En este tutorial se ha establecido la ubicación de los artefactos de AEM as a Cloud Service SDK y el código de proyecto de AEM de la siguiente manera:

+ `~/aem-sdk` es una carpeta organizativa que contiene las distintas herramientas proporcionadas por AEM as a Cloud Service SDK
+ `~/aem-sdk/author` contiene el servicio de AEM Author
+ `~/aem-sdk/publish` contiene el servicio de publicación de AEM
+ `~/aem-sdk/dispatcher` contiene Dispatcher Tools
+ `~/code/<project name>` contiene el código fuente personalizado del proyecto AEM

Tenga en cuenta que `~` es la abreviatura del directorio del usuario. En Windows, es el equivalente de `%HOMEPATH%`;

## Herramientas de desarrollo para proyectos de AEM

El proyecto de AEM es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager en AEM as a Cloud Service. La estructura del proyecto de línea de base se genera mediante el [Arquetipo Maven del proyecto AEM](https://github.com/adobe/aem-project-archetype).

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar las herramientas de desarrollo para proyectos de AEM](./development-tools.md)

## Tiempo de ejecución local de AEM

AEM as a Cloud Service SDK proporciona un [!DNL QuickStart Jar] que ejecuta una versión local de AEM. [!DNL QuickStart Jar] se puede usar para ejecutar el servicio de autor de AEM o el servicio de publicación de AEM localmente. Tenga en cuenta que aunque [!DNL QuickStart Jar] proporciona una experiencia de desarrollo local, no todas las características disponibles en AEM as a Cloud Service se incluyen en [!DNL QuickStart Jar].

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Descargar AEM SDK
+ Ejecutar [!DNL AEM Author Service]
+ Ejecutar [!DNL AEM Publish Service]

[Configuración del tiempo de ejecución de AEM local](./aem-runtime.md)

## Tiempo de ejecución [!DNL Dispatcher] local

Las herramientas Dispatcher de AEM as a Cloud Service SDK proporcionan todo lo necesario para configurar el tiempo de ejecución local de [!DNL Dispatcher]. Las herramientas de [!DNL Dispatcher] están basadas en [!DNL Docker] y proporcionan herramientas de línea de comandos para transformar archivos de configuración de [!DNL Apache HTTP] servidor web y [!DNL Dispatcher] en formatos compatibles e implementarlos en [!DNL Dispatcher] que se ejecuta en el contenedor de [!DNL Docker].

Esta sección del tutorial muestra cómo:

+ Descargar AEM SDK
+ Instalar [!DNL Dispatcher] herramientas
+ Ejecutar el tiempo de ejecución [!DNL Dispatcher] local

[Configurar el tiempo de ejecución de Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)

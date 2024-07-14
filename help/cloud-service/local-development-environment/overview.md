---
title: Entorno de desarrollo local para AEM as a Cloud Service
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
duration: 835
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

Este tutorial explica la configuración de un entorno de desarrollo local para Adobe Experience Manager AEM () mediante el SDK de AEM as a Cloud Service. AEM Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar proyectos de, así como los tiempos de ejecución locales que permiten a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service a través de Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![Pila de tecnología del entorno de desarrollo local de AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

AEM El entorno de desarrollo local para los se puede dividir en tres grupos lógicos:

+ AEM AEM El __Proyecto de__ contiene el código, la configuración y el contenido personalizados que es la aplicación personalizada.
+ AEM AEM __Tiempo de ejecución de la local__ que ejecuta una versión local de los servicios de Publish y Autor de manera local.
+ __Tiempo de ejecución local de Dispatcher__ que ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

AEM Este tutorial explica cómo instalar y configurar los elementos resaltados en el diagrama anterior, lo que proporciona un entorno de desarrollo local estable para el desarrollo de la.

## Organización del sistema de archivos

En este tutorial se ha establecido la ubicación de los artefactos del SDK de AEM as a Cloud Service AEM y el código del proyecto de la siguiente manera:

+ `~/aem-sdk` es una carpeta organizativa que contiene las distintas herramientas proporcionadas por el SDK de AEM as a Cloud Service
+ AEM `~/aem-sdk/author` contiene el servicio de autor de
+ AEM `~/aem-sdk/publish` contiene el servicio de Publish de la
+ `~/aem-sdk/dispatcher` contiene Dispatcher Tools
+ AEM `~/code/<project name>` contiene el código de origen personalizado del proyecto de

Tenga en cuenta que `~` es la abreviatura del directorio del usuario. En Windows, es el equivalente de `%HOMEPATH%`;

## AEM Herramientas de desarrollo para proyectos de

AEM El proyecto de es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager en AEM as a Cloud Service. AEM La estructura del proyecto de línea de base se genera a través del [Arquetipo Maven del proyecto de](https://github.com/adobe/aem-project-archetype).

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[AEM Configuración de herramientas de desarrollo para proyectos de](./development-tools.md)

## Tiempo de ejecución local de AEM

El SDK de AEM as a Cloud Service AEM proporciona un [!DNL QuickStart Jar] que ejecuta una versión local de la aplicación de la. AEM AEM [!DNL QuickStart Jar] se puede usar para ejecutar el servicio de autor de o el servicio de Publish de forma local. Tenga en cuenta que aunque [!DNL QuickStart Jar] proporciona una experiencia de desarrollo local, no todas las características disponibles en AEM as a Cloud Service se incluyen en [!DNL QuickStart Jar].

Esta sección del tutorial muestra cómo:

+ Instalar [!DNL Java]
+ AEM Descarga del SDK de
+ Ejecutar [!DNL AEM Author Service]
+ Ejecutar [!DNL AEM Publish Service]

[AEM Configuración del tiempo de ejecución de Local](./aem-runtime.md)

## Tiempo de ejecución [!DNL Dispatcher] local

Las herramientas Dispatcher del SDK de AEM as a Cloud Service proporcionan todo lo necesario para configurar el tiempo de ejecución de [!DNL Dispatcher] local. Las herramientas de [!DNL Dispatcher] están basadas en [!DNL Docker] y proporcionan herramientas de línea de comandos para transformar archivos de configuración de [!DNL Apache HTTP] servidor web y [!DNL Dispatcher] en formatos compatibles e implementarlos en [!DNL Dispatcher] que se ejecuta en el contenedor de [!DNL Docker].

Esta sección del tutorial muestra cómo:

+ AEM Descarga del SDK de
+ Instalar [!DNL Dispatcher] herramientas
+ Ejecutar el tiempo de ejecución [!DNL Dispatcher] local

[Configurar el tiempo de ejecución de Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)

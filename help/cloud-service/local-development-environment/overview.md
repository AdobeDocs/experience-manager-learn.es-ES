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
workflow-type: ht
source-wordcount: '530'
ht-degree: 100%

---

# Configuración del entorno de desarrollo local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Información general"
>abstract="La configuración del entorno de desarrollo local para AEM as a Cloud Service incluye las herramientas de desarrollo necesarias para desarrollar, crear y compilar Proyectos AEM, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service mediante Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=es" text="Directrices de desarrollo"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=es" text="Conceptos básicos de desarrollo"

En este tutorial se explica cómo configurar un entorno de desarrollo local para Adobe Experience Manager (AEM) mediante el SDK de AEM as a Cloud Service. Se incluyen las herramientas de desarrollo necesarias para desarrollar, crear y compilar proyectos AEM, así como los tiempos de ejecución locales, lo que permite a los desarrolladores validar rápidamente las nuevas funciones localmente antes de implementarlas en AEM as a Cloud Service mediante Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![Pila tecnológica del entorno de desarrollo local de AEM as a Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

El entorno de desarrollo local de AEM se puede dividir en tres grupos lógicos:

+ El __proyecto AEM__ contiene el código, la configuración y el contenido personalizados que conforman la aplicación AEM personalizada.
+ __AEM Runtime local__ ejecuta una versión local de los servicios AEM Author y Publish.
+ __Dispatcher Runtime local__ ejecuta una versión local de Apache HTTP Web Server y Dispatcher.

Este tutorial explica cómo instalar y configurar los elementos destacados en el diagrama anterior, proporcionando un entorno de desarrollo local estable para el desarrollo de AEM.

## Organización del sistema de archivos

En este tutorial se ha establecido la ubicación de los artefactos del SDK de AEM as a Cloud Service y el código de proyecto de AEM de la siguiente manera:

+ `~/aem-sdk` es una carpeta organizativa que contiene las distintas herramientas proporcionadas por el SDK de AEM as a Cloud Service
+ `~/aem-sdk/author` contiene el servicio AEM Author
+ `~/aem-sdk/publish` contiene el servicio AEM Publish
+ `~/aem-sdk/dispatcher` contiene las herramientas de Dispatcher
+ `~/code/<project name>` contiene el código fuente personalizado del proyecto AEM

Tenga en cuenta que `~` es la abreviatura de directorio del usuario. En Windows, equivale a `%HOMEPATH%`;

## Herramientas de desarrollo para proyectos AEM

El proyecto de AEM es la base de código personalizado que contiene el código, la configuración y el contenido que se implementa mediante Cloud Manager en AEM as a Cloud Service. La estructura del proyecto de línea de base se genera mediante el [Arquetipo Maven del proyecto AEM](https://github.com/adobe/aem-project-archetype).

En esta sección del tutorial se muestra cómo hacer lo siguiente:

+ Instalar [!DNL Java]
+ Instalar [!DNL Node.js] (y npm)
+ Instalar [!DNL Maven]
+ Instalar [!DNL Git]

[Configurar las herramientas de desarrollo para proyectos AEM](./development-tools.md)

## Tiempo de ejecución local de AEM

El SDK de AEM as a Cloud Service proporciona el archivo [!DNL QuickStart Jar] que ejecuta una versión local de AEM. [!DNL QuickStart Jar] se puede usar para ejecutar el servicio AEM Author o el servicio AEM Publish localmente. Tenga en cuenta que aunque [!DNL QuickStart Jar] proporciona una experiencia de desarrollo local, no todas las funciones disponibles en AEM as a Cloud Service se incluyen en [!DNL QuickStart Jar].

En esta sección del tutorial se muestra cómo hacer lo siguiente:

+ Instalar [!DNL Java]
+ Descargar el SDK de AEM
+ Ejecutar [!DNL AEM Author Service]
+ Ejecutar [!DNL AEM Publish Service]

[Configurar AEM Runtime local](./aem-runtime.md)

## [!DNL Dispatcher] Runtime local

Las herramientas de Dispatcher del SDK de AEM as a Cloud Service proporcionan todo lo necesario para configurar [!DNL Dispatcher] Runtime local. Las herramientas de [!DNL Dispatcher] se basan en [!DNL Docker] y proporcionan herramientas de línea de comandos para convertir archivos de configuración de [!DNL Apache HTTP] del servidor web y [!DNL Dispatcher] en formatos compatibles e implementarlos en [!DNL Dispatcher] que se ejecuta en el contenedor de [!DNL Docker].

En esta sección del tutorial se muestra cómo hacer lo siguiente:

+ Descargar el SDK de AEM
+ Instalar las herramientas de [!DNL Dispatcher]
+ Ejecutar [!DNL Dispatcher] Runtime local

[Configurar  [!DNL Dispatcher]  Runtime local](./dispatcher-tools.md)

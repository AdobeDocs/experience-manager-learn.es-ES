---
title: Introducción al Editor de SPA de AEM y React
description: Cree la primera aplicación de una sola página (SPA) en React que se pueda editar en Adobe Experience Manager AEM con la SPA de WKND. Aprenda a crear una SPA con el marco de trabajo de React JS con AEM SPA Editor. Este tutorial en varias partes explica la implementación de una aplicación de React para una marca ficticia de estilo de vida, WKND. El tutorial cubre la creación de extremo a extremo del SPA y la integración con AEM.
version: Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 100
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 30%

---

# SPA AEM Creación de su primer React en el {#overview}

{{edge-delivery-services}}

Bienvenido a un tutorial de varias partes diseñado para desarrolladores nuevos en la **SPA Editor de** función en Adobe Experience Manager AEM (). Este tutorial explora la implementación de una aplicación de React para una marca ficticia de estilo de vida, WKND. AEM SPA AEM La aplicación React se ha desarrollado y diseñado para implementarse con el Editor de, que asigna los componentes React a los componentes de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de la aplicación de. SPA AEM AEM Los segmentos completados, implementados para la creación de informes de forma dinámica, se pueden crear con las herramientas de edición en línea tradicionales de la creación de informes de la versión en tiempo de ejecución de la versión en tiempo de ejecución de la.

![SPA Implementación final de](assets/wknd-spa-implementation.png)

*SPA Implementación de WKND*

## Acerca de

El tutorial está diseñado para trabajar con **AEM as a Cloud Service** y es compatible con **AEM.5.4+** y **AEM.4.8+**.

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

El [última base de código](https://github.com/adobe/aem-guides-wknd-spa/releases) AEM está disponible como paquetes de descarga de la aplicación de descarga de.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Conocimientos básicos de HTML, CSS y JavaScript
* Familiaridad básica con [Reaccionar](https://reactjs.org/tutorial/tutorial.html)

*Si bien no es necesario, es beneficioso tener una comprensión básica de [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. AEM Las capturas de pantalla y los vídeos se capturan mediante el SDK as a Cloud Service de que se ejecuta en un entorno de sistema operativo Mac con [Código de Visual Studio](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

* [AEM SDK as a Cloud Service de](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) o [AEM.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> **AEM ¿Nuevo en la versión 6.5 de?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Pasos siguientes {#next-steps}

¿Qué estás esperando?! Inicie el tutorial navegando hasta [Crear proyecto](create-project.md) SPA AEM y aprenda a generar un proyecto habilitado para el Editor de mediante el Tipo de archivo del proyecto de.

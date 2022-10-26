---
title: 'Introducción a AEM Sites: Tipo de archivo del proyecto'
description: 'Introducción a AEM Sites: Tipo de archivo del proyecto. El tutorial de WKND es un tutorial de varias partes diseñado para desarrolladores que no han llegado a Adobe Experience Manager. El tutorial recorre la implementación de un sitio AEM para una marca ficticia de estilo de vida, la WKND. El tutorial cubre temas fundamentales como la configuración del proyecto, los arquetipos de maven, los componentes principales, las plantillas editables, las bibliotecas de cliente y el desarrollo de componentes.'
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 24%

---

# Introducción a AEM Sites: Tipo de archivo del proyecto {#project-archetype}

Le damos la bienvenida a un tutorial de varias partes diseñado para desarrolladores que utilicen Adobe Experience Manager (AEM) por primera vez. Este tutorial recorre la implementación de un sitio AEM para una marca ficticia de estilo de vida WKND.

Este tutorial comienza con el uso de [Tipo de archivo del proyecto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es) para generar un nuevo proyecto.

El tutorial está diseñado para funcionar con **AEM as a Cloud Service** y es compatible con **AEM 6.5.10+**. El sitio se implementa mediante:

* [Tipo de archivo del proyecto Maven AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Modelos Sling
* [Plantillas editables](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=es)
* [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Calcule entre 1 y 2 horas para pasar por cada parte del tutorial.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan mediante el SDK as a Cloud Service de AEM que se ejecuta en un entorno del sistema operativo Mac con [Código de Visual Studio](https://code.visualstudio.com/) como el IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

Lo siguiente debe instalarse de manera local:

* [AEM local **Autor** instancia](https://experience.adobe.com/#/downloads) (Cloud Service SDK, 6.5.10+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) (LTS: compatibilidad a largo plazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Código de Visual Studio](https://code.visualstudio.com/) o IDE equivalente
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Herramienta utilizada a través del tutorial

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> **¿Es nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Github {#github}

Todo el código del proyecto se puede encontrar en Github en el repositorio de la Guía de AEM:

**[GitHub: Proyecto WKND Sites](https://github.com/adobe/aem-guides-wknd)**

Además, cada parte del tutorial tiene su propia rama en GitHub. Un usuario puede iniciar el tutorial en cualquier momento simplemente comprobando la rama que corresponde a la parte anterior.

## Pasos siguientes {#next-steps}

¡¿Qué estás esperando?! Inicie el tutorial navegando hasta el [Configuración del proyecto](project-setup.md) y aprenda a generar un nuevo proyecto de Adobe Experience Manager con el tipo de archivo del proyecto AEM.

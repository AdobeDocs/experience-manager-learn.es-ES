---
title: 'Introducción a AEM Sites: Arquetipo de proyecto'
description: 'Introducción a AEM Sites: Arquetipo de proyecto. El tutorial de WKND es un tutorial de varias partes diseñado para desarrolladores que utilicen Adobe Experience Manager por primera vez. AEM El tutorial recorre la implementación de un sitio de trabajo para una marca ficticia de estilo de vida, WKND. El tutorial abarca temas fundamentales como la configuración del proyecto, los tipos de archivo Maven, los componentes principales, las plantillas editables, las bibliotecas de cliente y el desarrollo de componentes.'
version: 6.5, Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 96
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 18%

---

# Introducción a AEM Sites: Arquetipo de proyecto {#project-archetype}

{{edge-delivery-services-and-page-editor}}

Bienvenido a un tutorial de varias partes diseñado para desarrolladores nuevos en Adobe Experience Manager AEM (). AEM Este tutorial explora la implementación de un sitio de trabajo para una marca ficticia de estilo de vida: WKND.

Este tutorial comienza con el [AEM Tipo de archivo del proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es) para generar un nuevo proyecto.

El tutorial está diseñado para trabajar con **AEM as a Cloud Service** y es compatible con **AEM.5.14+**. El sitio se implementa mediante lo siguiente:

* [Arquetipo del proyecto Maven de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es)
* [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Modelos de Sling](https://sling.apache.org/documentation/bundles/models.html)
* [Plantillas editables](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=es)
* [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=es)

*Calcule entre 1 y 2 horas para completar cada parte del tutorial.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. AEM Las capturas de pantalla y los vídeos se capturan mediante el SDK as a Cloud Service de que se ejecuta en un entorno de macOS con [Código de Visual Studio](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

Lo siguiente debe instalarse de manera local:

* [AEM Local **Autor** instancia](https://experience.adobe.com/#/downloads) (SDK de Cloud Service o 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) (LTS - Soporte a largo plazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Código de Visual Studio](https://code.visualstudio.com/) o IDE equivalente
   * [AEM Sincronización de VSCode con la](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Herramienta utilizada en todo el tutorial

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> **AEM ¿Nuevo en la versión 6.5 de?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## GitHub {#github}

AEM El código de este tutorial se encuentra en GitHub, en el repositorio de la Guía del usuario de la Guía de la aplicación de la:

**[GitHub: proyecto de WKND Sites](https://github.com/adobe/aem-guides-wknd)**

Además, cada parte del tutorial tiene su propia rama en GitHub. Un usuario puede comenzar el tutorial en cualquier momento simplemente desprotegiendo la rama que corresponde a la parte anterior.

## Pasos siguientes {#next-steps}

¿Qué estás esperando? Inicie el tutorial navegando hasta [Configuración del proyecto](project-setup.md) y aprenda a generar un nuevo proyecto de Adobe Experience Manager AEM utilizando el tipo de archivo del proyecto de.

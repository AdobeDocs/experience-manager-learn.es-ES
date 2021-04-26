---
title: 'Introducción a AEM Sites: Tipo de archivo del proyecto'
description: 'Introducción a AEM Sites: Tipo de archivo del proyecto. El tutorial de WKND es un tutorial de varias partes diseñado para desarrolladores que no han llegado a Adobe Experience Manager. El tutorial recorre la implementación de un sitio AEM para una marca ficticia de estilo de vida, la WKND. El tutorial cubre temas fundamentales como la configuración del proyecto, los arquetipos de maven, los componentes principales, las plantillas editables, las bibliotecas de cliente y el desarrollo de componentes.'
sub-product: sitios
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Componentes principales, Editor de páginas, Plantillas editables, AEM tipo de archivo del proyecto
topic: Gestión de contenido, desarrollo
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 20%

---


# Introducción a AEM Sites: Tipo de archivo del proyecto {#project-archetype}

Le damos la bienvenida a un tutorial de varias partes diseñado para desarrolladores que utilicen Adobe Experience Manager (AEM) por primera vez. Este tutorial recorre la implementación de un sitio AEM para una marca ficticia de estilo de vida WKND.

Este tutorial comienza con el [AEM tipo de archivo del proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) para generar un nuevo proyecto.

El tutorial está diseñado para funcionar con **AEM como Cloud Service** y es compatible con **AEM 6.5.5.0+** y **AEM 6.4.8.1+**. El sitio se implementa mediante:

* [Tipo de archivo del proyecto Maven AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelos Sling
* [Plantillas editables](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema de estilos](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Calcule entre 1 y 2 horas para pasar por cada parte del tutorial.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan usando el SDK de AEM as a Cloud Service que se ejecuta en un entorno de Mac OS con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

Lo siguiente debe instalarse de manera local:

* Instancia de AEM local **Autor** (SDK de Cloud Service, 6.5.5+ o 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/)  (LTS: compatibilidad a largo plazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Código ](https://code.visualstudio.com/) de Visual Studio o IDE equivalente
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) : herramienta utilizada en todo el tutorial

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://docs.adobe.com/content/help/es-ES/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es nuevo en AEM 6.5?** Consulte la  [siguiente guía para configurar un entorno](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desarrollo local.

## Github {#github}

Todo el código del proyecto se puede encontrar en Github en el repositorio de la Guía de AEM:

**[GitHub: Proyecto WKND Sites](https://github.com/adobe/aem-guides-wknd)**

Además, cada parte del tutorial tiene su propia rama en GitHub. Un usuario puede iniciar el tutorial en cualquier momento simplemente comprobando la rama que corresponde a la parte anterior.

## Pasos siguientes {#next-steps}

¡¿Qué estás esperando?! Inicie el tutorial navegando hasta el capítulo [Configuración del proyecto](project-setup.md) y aprenda a generar un nuevo proyecto de Adobe Experience Manager con el tipo de archivo del proyecto AEM.

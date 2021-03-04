---
title: 'Introducción a AEM Sites: Tutorial de WKND'
description: 'Introducción a AEM Sites: Tutorial de WKND. El tutorial de WKND es un tutorial de varias partes diseñado para desarrolladores que no hayan utilizado Adobe Experience Manager. El tutorial recorre la implementación de un sitio de AEM para una marca ficticia de estilo de vida, la WKND. El tutorial cubre temas fundamentales como la configuración del proyecto, los arquetipos de maven, los componentes principales, las plantillas editables, las bibliotecas de cliente y el desarrollo de componentes.'
sub-product: sitios
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: '"Componentes principales, editor de páginas, plantillas editables, tipo de archivo del proyecto AEM"'
topic: '"Gestión de contenido, desarrollo"'
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 14%

---


# Introducción a AEM Sites: Tutorial de WKND {#introduction}

Le damos la bienvenida a un tutorial en varias partes diseñado para desarrolladores que utilicen Adobe Experience Manager (AEM) por primera vez. Este tutorial recorre la implementación de un sitio de AEM para una marca ficticia de estilo de vida de WKND. El tutorial trata temas fundamentales como la configuración del proyecto, los componentes principales, las plantillas editables, las bibliotecas del lado del cliente y el desarrollo de componentes con Adobe Experience Manager Sites.

## Información general {#wknd-tutorial-overview}

El objetivo de este tutorial en varias partes es enseñar a los desarrolladores a implementar un sitio web utilizando los estándares y tecnologías más recientes en Adobe Experience Manager (AEM). Después de completar este tutorial, un desarrollador debe comprender la base básica de la plataforma y con conocimientos de patrones de diseño comunes en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

El tutorial está diseñado para funcionar con **AEM as a Cloud Service** y es compatible con **AEM 6.5.5.0+** y **AEM 6.4.8.1+**. El sitio se implementa mediante:

* [Tipo de archivo del proyecto AEM Maven](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelos Sling
* [Plantillas editables](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema de estilos](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Calcule entre 1 y 2 horas para pasar por cada parte del tutorial.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan utilizando el SDK de AEM as a Cloud Service que se ejecuta en un entorno Mac OS con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

Lo siguiente debe instalarse de manera local:

* Instancia local de AEM **Author** (SDK de Cloud Service, 6.5.5+ o 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/)  (LTS: compatibilidad a largo plazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Código ](https://code.visualstudio.com/) de Visual Studio o IDE equivalente
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) : herramienta utilizada a lo largo del tutorial

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://docs.adobe.com/content/help/es-ES/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es novato en AEM 6.5?** Consulte la  [siguiente guía para configurar un entorno](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desarrollo local.

## Acerca del tutorial {#about-tutorial}

WKND es una revista online ficticia y blog que se centra en la vida nocturna, actividades y eventos en varias ciudades internacionales.

### Kit de IU de Adobe XD

Para acercar este tutorial a un escenario real, los talentosos diseñadores de experiencia de usuario de Adobe crearon las maquetas para el sitio utilizando [Adobe XD](https://www.adobe.com/products/xd.html). A lo largo del tutorial, varias partes de los diseños se implementan en un sitio AEM totalmente creativo. Agradecimiento especial a **Lorenzo Buosi** y **Kilian Amendola** que crearon el hermoso diseño para el sitio WKND.

Descargue los kits de IU XD:

* [Kit de interfaz de usuario de los componentes principales de AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit de IU WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

El nombre WKND es adecuado porque esperamos que un desarrollador aproveche la mejor parte de un ***fin de semana*** para completar el tutorial.

### Github {#github}

Todo el código del proyecto se puede encontrar en Github en el repositorio de la Guía de AEM:

**[GitHub: Proyecto WKND Sites](https://github.com/adobe/aem-guides-wknd)**

Además, cada parte del tutorial tiene su propia rama en GitHub. Un usuario puede iniciar el tutorial en cualquier momento simplemente comprobando la rama que corresponde a la parte anterior.

>[!NOTE]
>
> Si estaba trabajando con la versión anterior de este tutorial, aún puede encontrar los [paquetes de soluciones](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) y [código](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) en GitHub.

## Sitio de referencia {#reference-site}

También está disponible como referencia una versión terminada del sitio WKND: [https://wknd.site/](https://wknd.site/)

El tutorial cubre las principales habilidades de desarrollo necesarias para un desarrollador de AEM, pero *no* creará todo el sitio de principio a fin. El sitio de referencia terminado es otro excelente recurso para explorar y ver más funciones de AEM listas para usar.

Para probar el código más reciente antes de entrar en el tutorial, descargue e instale la **[última versión de GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Con la tecnología de Adobe Stock

Muchas de las imágenes del sitio web de referencia de WKND provienen de [Adobe Stock](https://stock.adobe.com/) y son Material de terceros, tal como se define en los términos adicionales de Demo Asset en [https://www.adobe.com/legal/terms.html](https://www.adobe.com/es/legal/terms.html). Si desea utilizar una imagen de Adobe Stock para otros fines más allá de ver este sitio web de demostración, como incluirla en un sitio web o en materiales de marketing, puede adquirir una licencia en Adobe Stock.

Con Adobe Stock, tiene acceso a más de 140 millones de imágenes de alta calidad y sin derechos de autor, incluidas fotos, gráficos, vídeos y plantillas para iniciar sus proyectos creativos.

## Pasos siguientes {#next-steps}

¡¿Qué estás esperando?! Inicie el tutorial navegando hasta el capítulo [Configuración del proyecto](project-setup.md) y aprenda a generar un nuevo proyecto de Adobe Experience Manager utilizando el tipo de archivo del proyecto AEM.

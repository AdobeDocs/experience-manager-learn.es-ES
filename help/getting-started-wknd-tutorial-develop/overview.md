---
title: 'Introducción a AEM Sites: Tutorial de WKND'
description: 'Introducción a AEM Sites: Tutorial de WKND. El tutorial de WKND es un tutorial de varias partes diseñado para desarrolladores nuevos en Adobe Experience Manager. El tutorial avanza a través de la implementación de un sitio AEM para una marca ficticia de estilo de vida, la WKND. El tutorial trata temas fundamentales como la configuración de proyectos, los arquetipos creados, los componentes principales, las plantillas editables, las bibliotecas de clientes y el desarrollo de componentes.'
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
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 14%

---


# Introducción a AEM Sites: Tutorial de WKND {#introduction}

Bienvenido a un tutorial de varias partes diseñado para desarrolladores nuevos en Adobe Experience Manager (AEM). Este tutorial explora la implementación de un sitio AEM para un estilo de vida ficticio de la marca WKND. El tutorial trata temas fundamentales como la configuración del proyecto, los componentes principales, las plantillas editables, las bibliotecas del lado del cliente y el desarrollo de componentes con Adobe Experience Manager Sites.

## Información general {#wknd-tutorial-overview}

El objetivo de este tutorial en varias partes es enseñar a los desarrolladores cómo implementar un sitio web con los estándares y tecnologías más recientes en Adobe Experience Manager (AEM). Después de completar este tutorial, un desarrollador debe comprender los fundamentos básicos de la plataforma y conocer los patrones de diseño comunes en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

El tutorial está diseñado para funcionar con **AEM como Cloud Service** y es compatible con **AEM 6.5.5.0+** y **AEM 6.4.8.1+**. El sitio se implementa mediante:

* [Arquetipo del proyecto Maven AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/overview.html)
* [Componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Modelos Sling
* [Plantillas editables](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Sistema de estilos](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Calcule entre 1 y 2 horas para ver cada parte del tutorial.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan utilizando el AEM como SDK de Cloud Service que se ejecuta en un entorno de Mac OS con [código de Visual Studio](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

Lo siguiente debe instalarse de manera local:

* Instancia de AEM local **Autor** (SDK de Cloud Service, 6.5.5+ o 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) (LTS: compatibilidad a largo plazo)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://docs.adobe.com/content/help/es-ES/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es nuevo en AEM 6.5?** Consulte la  [siguiente guía para configurar un entorno](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desarrollo local.

## Acerca del tutorial {#about-tutorial}

WKND es una revista y blog ficticio en línea que se centra en la vida nocturna, actividades y eventos en varias ciudades internacionales.

### Adobe XD UI Kit

Para hacer que este tutorial se acerque más a un escenario real de los talentosos diseñadores UX de un Adobe han creado las maquetas para el sitio con [Adobe XD](https://www.adobe.com/products/xd.html). En el curso del tutorial se implementan varias partes de los diseños en un sitio de AEM totalmente creativo. Agradecimiento especial a **Lorenzo Buosi** y **Kilian Amendola** que crearon el hermoso diseño para el sitio WKND.

Descargue los kits de IU de XD:

* [Kit de interfaz de usuario de componentes principales de AEM](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [Kit de IU WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

El nombre WKND es adecuado porque esperamos que un desarrollador tome la mejor parte de un ***fin de semana*** para completar el tutorial.

### Github {#github}

Todo el código del proyecto se puede encontrar en Github en la repo de la Guía de AEM:

**[GitHub: Proyecto de sitios WKND](https://github.com/adobe/aem-guides-wknd)**

Además, cada parte del tutorial tiene su propia rama en GitHub. Un usuario puede comenzar el tutorial en cualquier momento simplemente desprotegiendo la rama que corresponde a la parte anterior.

>[!NOTE]
>
> Si estaba trabajando con la versión anterior de este tutorial, aún puede encontrar los [paquetes de soluciones](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) y [código](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) en GitHub.

## Sitio de referencia {#reference-site}

Una versión terminada del sitio WKND también está disponible como referencia: [https://wknd.site/](https://wknd.site/)

El tutorial cubre las principales habilidades de desarrollo necesarias para un desarrollador AEM, pero *no* generará todo el sitio de extremo a extremo. El sitio de referencia terminado es otro recurso bueno para explorar y ver más AEM de las funciones integradas.

Para probar el código más reciente antes de pasar al tutorial, descargue e instale la **[última versión de GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Equipado con Adobe Stock

Muchas de las imágenes del sitio web de referencia de WKND son de [Adobe Stock](https://stock.adobe.com/) y son Material de terceros, tal como se define en los términos adicionales de Demo Asset en [https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html). Si desea utilizar una imagen de Adobe Stock para otros fines además de ver este sitio web de demostración, como publicarla en un sitio web o en materiales de marketing, puede adquirir una licencia en Adobe Stock.

Con Adobe Stock, usted tiene acceso a más de 140 millones de imágenes de alta calidad y sin derechos de autor, incluyendo fotos, gráficos, vídeos y plantillas para dar un salto a sus proyectos creativos.

## Próximos pasos {#next-steps}

¡¿Qué estás esperando?! Inicio el tutorial navegando al capítulo [Configuración del proyecto](project-setup.md) y aprenda a generar un nuevo proyecto de Adobe Experience Manager usando el arquetipo del proyecto AEM.

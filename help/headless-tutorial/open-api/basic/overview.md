---
title: Tutorial de AEM Headless OpenAPI | Entrega de fragmentos de contenido
description: Un tutorial completo que ilustra cómo crear y exponer contenido mediante las API de entrega de fragmentos de contenido basadas en OpenAPI de AEM.
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 22%

---

# Introducción a la entrega de fragmentos de contenido de AEM con API de OpenAPI

Explore este tutorial que ilustra cómo crear y exponer contenido de AEM mediante la entrega de fragmentos de contenido de AEM con API de OpenAPI y consumido por una aplicación externa, en un escenario de CMS sin encabezado. Esto explora estos conceptos considerando la creación de una aplicación de React que muestra los equipos de WKND y los detalles de los miembros asociados. Los equipos y los miembros se modelan mediante modelos de fragmentos de contenido de AEM y la aplicación React los consume mediante la entrega de fragmentos de contenido de AEM con las API de OpenAPI.

![Aplicación de equipos WKND](./assets/overview/main.png)

Este tutorial abarca los siguientes temas:

* Crear una configuración del proyecto
* Crear modelos de fragmento de contenido para modelar datos
* Crear fragmentos de contenido basados en los modelos creados anteriormente
* Explore cómo se pueden consultar los fragmentos de contenido en AEM mediante la Entrega de fragmentos de contenido de AEM con la función &quot;Probar&quot; de la documentación de las API de OpenAPI
* Consumir datos de fragmentos de contenido mediante la entrega de fragmentos de contenido de AEM con llamadas de API OpenAPI desde una aplicación React de ejemplo
* Mejore la aplicación React para que pueda editarse en el editor universal

## Requisitos previos {#prerequisites}

Se requiere lo siguiente para seguir este tutorial:

* AEM Sites as a Cloud Service
* Aptitudes básicas de HTML y JavaScript
* Las siguientes herramientas deben estar instaladas localmente:
   * [Node.js v22+](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * Un IDE (por ejemplo, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### Entorno de AEM as a Cloud Service

Para completar este tutorial, se recomienda que **AEM Administrator** tenga acceso a un entorno de AEM as a Cloud Service. También se puede usar un entorno **Development**, **Entorno de desarrollo rápido** o un entorno en un **programa de espacio aislado**.

## ¡Empecemos!

Comience el tutorial con la [Definición de modelos de fragmento de contenido](1-content-fragment-models.md).

## Proyecto de GitHub

El código fuente y los paquetes de contenido están disponibles en el repositorio de GitHub [AEM Headless tutorials](https://github.com/adobe/aem-tutorials).

La rama [`main` contiene el código fuente final &#x200B;](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) para este tutorial.
Las instantáneas del código al final de cada paso están disponibles como etiquetas Git.

* Inicio del capítulo 4 - Aplicación React: [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* Fin del capítulo 4 - Aplicación React: [`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* Final del capítulo 5 - Editor universal: [`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

Si se encuentra un problema con el tutorial o el código, deje un [problema en GitHub](https://github.com/adobe/aem-tutorials/issues).

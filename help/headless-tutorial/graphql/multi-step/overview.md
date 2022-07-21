---
title: 'Tutorial de introducción a AEM sin encabezado: GraphQL'
description: Un tutorial completo que ilustra cómo crear y exponer contenido mediante las API de AEM GraphQL.
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: 328618.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 410eb23534e083940bf716194576e099d22ca205
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 4%

---

# Introducción a AEM sin encabezado: GraphQL

Un tutorial completo que ilustra cómo crear y exponer contenido mediante las API de GraphQL de AEM y consumido por una aplicación externa, en un escenario de CMS remoto.

Este tutorial explora cómo se pueden utilizar AEM API de GraphQL y capacidades sin encabezado para impulsar las experiencias que aparecen en una aplicación externa.

Este tutorial tratará los siguientes temas:

* Crear una nueva configuración de proyecto
* Crear nuevos modelos de fragmento de contenido para modelar datos
* Cree nuevos fragmentos de contenido basados en los modelos creados anteriormente.
* Explore cómo se pueden consultar los fragmentos de contenido en AEM mediante la herramienta de desarrollo integrada GraphiQL.
* Para almacenar o mantener las consultas de GraphQL en AEM
* Consumir consultas de GraphQL persistentes desde una aplicación React de muestra


## Requisitos previos {#prerequisites}

Se requieren las siguientes opciones para seguir este tutorial:

* Capacidades básicas de HTML y JavaScript
* Las siguientes herramientas deben instalarse localmente:
   * [Node.js v10+](https://nodejs.org/en/)
   * [npm 6+](https://www.npmjs.com/)
   * [Git](https://git-scm.com/)
   * Un IDE (por ejemplo, [Código Microsoft® Visual Studio](https://code.visualstudio.com/))

### Entorno AEM

Se requiere un entorno de Adobe Experience Manager para completar este tutorial. Se puede utilizar cualquiera de las siguientes opciones (las capturas de pantalla se registran desde un entorno as a Cloud Service AEM):

* AEM entorno as a Cloud Service con:
   * [Acceso a AEM as a Cloud Service y Cloud Manager](/help/cloud-service/accessing/overview.md)
      * **Administrador AEM** acceso a AEM as a Cloud Service

## ¡Empecemos!

1. Inicie el tutorial con [Definición de modelos de fragmento de contenido](content-fragment-models.md).

## Proyecto de GitHub

El código fuente y los paquetes de contenido están disponibles en la [Guías de AEM: proyecto de GitHub de WKND GraphQL](https://github.com/adobe/aem-guides-wknd-graphql).

Si encuentra algún problema con el tutorial o el código, deje un [Problema de GitHub](https://github.com/adobe/aem-guides-wknd-graphql/issues).

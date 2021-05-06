---
title: 'Introducción a Editor de SPA y SPA remoto: información general'
description: Le damos la bienvenida al tutorial en varias partes para desarrolladores que buscan aumentar un SPA remoto existente con contenido AEM editable mediante AEM Editor SPA.
topic: Sin cabeza, SPA, desarrollo
feature: Editor de SPA, componentes principales, API, desarrollo
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: kt-7630.jpg
translation-type: tm+mt
source-git-commit: b6f63110f14ede51fa2dd740aea7cbb623cbec60
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 6%

---


# Información general

Le damos la bienvenida al tutorial en varias partes para desarrolladores que buscan aumentar un SPA remoto basado en React (o Next.js) existente con contenido AEM editable mediante AEM editor SPA.

Este tutorial se basa en la aplicación [WKND GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html), una aplicación React que consume AEM contenido de fragmentos de contenido sobre AEM API de GraphQL, pero no proporciona ninguna creación en contexto de contenido SPA.

## Acerca del tutorial

El tutorial pretende ilustrar cómo se puede actualizar un SPA remoto o un SPA que se ejecute fuera del contexto de AEM para consumir y entregar contenido creado en AEM.

La mayoría de las actividades del tutorial se centran en el desarrollo de JavaScript; sin embargo, se tratan aspectos críticos que giran en torno a la AEM. Estos aspectos incluyen la definición de dónde se crea y almacena el contenido en AEM y la asignación SPA rutas a páginas AEM.

El tutorial está diseñado para funcionar con **AEM como Cloud Service** y consta de dos proyectos:

1. El __AEM proyecto__ contiene configuración y contenido que se deben implementar en AEM.
1. __WKND__ Appproject es el SPA que se integra con AEM SPA Editor

## Último código

+ El código de este tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphq) en la rama `feature/spa-editor`.

## Requisitos previos

Este tutorial requiere lo siguiente:

+ [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip o buenas](https://github.com/adobe/aem-guides-wknd/releases)
+ [código fuente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql)

Este tutorial supone:

+ [Código Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) como el IDE
+ Un directorio de trabajo de `~/Code/wknd-app`
+ Ejecución del SDK de AEM como servicio de creación en `http://localhost:4502`
+ Ejecución del SDK de AEM con la cuenta local `admin` con contraseña `admin`
+ Ejecución del SPA en `http://localhost:3000`

>[!NOTE]
>
> **¿Necesita ayuda para configurar su entorno de desarrollo local?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).


## Configuración rápida

La configuración rápida le permite poner en marcha la aplicación WKND SPA y AEM SPA Editor en 15 minutos. Esta configuración acelerada le lleva directamente al estado final del tutorial, lo que le permite explorar la creación de la SPA en AEM Editor SPA.

+ [Obtenga información sobre la configuración rápida](./quick-setup.md)

## 1. Configurar AEM para SPA Editor

AEM configuraciones son necesarias para integrar la SPA con AEM Editor SPA. Estas configuraciones se administran e implementan mediante un proyecto AEM. En este capítulo, conozca qué configuraciones son necesarias y cómo definirlas.

+ [Obtenga información sobre cómo configurar AEM para SPA Editor](./aem-configure.md)

## 2. Bootstrap del SPA

Para que AEM SPA Editor integre una SPA en su contexto de creación, se deben realizar algunas adiciones a la SPA.

+ [Aprenda a arrancar el SPA para AEM Editor SPA](./spa-bootstrap.md)

## 3. Componentes fijos editables

En primer lugar, explore la posibilidad de añadir un &quot;componente fijo&quot; editable al SPA. Esto ilustra cómo un desarrollador puede colocar un componente editable específico en la SPA. Aunque el autor puede cambiar el contenido del componente, no puede quitarlo ni cambiar su ubicación, posición o tamaño.

+ [Obtenga información sobre los componentes fijos editables](./spa-fixed-component.md)

## 4. Componentes de contenedor editables

A continuación, explore la posibilidad de agregar un &quot;componente contenedor&quot; editable al SPA. Esto ilustra cómo un desarrollador puede colocar un componente contenedor en el SPA. Los componentes de contenedor permiten a los autores colocar en él el componente permitido y ajustar el diseño de los componentes.

+ [Obtenga información sobre los componentes de contenedor editables](./spa-container-component.md)

## 5. Rutas dinámicas y componentes editables

Por último, utilice los conceptos descritos en capítulos anteriores para las rutas dinámicas; rutas que muestran contenido diferente en función del parámetro de ruta. Esto ilustra cómo se puede utilizar AEM editor SPA para crear contenido en rutas que se dirigen mediante programación y se derivan.

+ [Obtenga información sobre rutas dinámicas y componentes editables](./spa-dynamic-routes.md)

## Recursos adicionales

+ [Edición de un SPA externo dentro de documentos AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [Componentes de WCM de AEM: implementación principal de React](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [Componentes de WCM de AEM: editor de spa: implementación de React Core](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
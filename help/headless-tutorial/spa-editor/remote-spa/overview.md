---
title: 'SPA SPA Introducción a Editor de y Administración remota: Información general'
description: SPA AEM AEM SPA Bienvenido al tutorial de varias partes para desarrolladores que buscan aumentar un contenido remoto existente con contenido editable de la mediante el Editor de segmentos de la aplicación de la aplicación de la versión de.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 6%

---

# Información general

{{edge-delivery-services}}

SPA AEM AEM SPA Bienvenido al tutorial de varias partes para desarrolladores que buscan aumentar un contenido remoto basado en React (o Next.js) existente con contenido editable de la mediante el Editor de contenido de la aplicación.

Este tutorial se basa en [WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=es), una aplicación de React que consume contenido de fragmentos de contenido de la red de contenido (FDA) de la red de aplicaciones (API) de GraphQL SPA, pero que no proporciona ninguna creación de contenido en contexto de la red de aplicaciones (API) de la red de aplicaciones de AEM AEM de la red de.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Información sobre el tutorial

SPA SPA AEM AEM El tutorial tiene por objeto ilustrar cómo se puede actualizar un remoto, o una aplicación que se ejecuta fuera del contexto de la, para consumir y entregar contenido creado en la aplicación de la manera más rápida y sencilla. En este ejemplo, se puede crear un tutorial de.

La mayoría de las actividades del tutorial se centran en el desarrollo de JavaScript AEM; sin embargo, se cubren aspectos esenciales que giran en torno a la. AEM SPA AEM Estos aspectos incluyen la definición de dónde se crea el contenido y dónde se almacena en las rutas de acceso de la y la asignación de rutas de acceso a las páginas de la página de destino.

El tutorial está diseñado para funcionar con **AEM as a Cloud Service** y se compone de dos proyectos:

1. AEM AEM El __Proyecto de__ contiene configuración y contenido que deben implementarse para la implementación de los elementos de la interfaz de usuario de la interfaz de usuario de la interfaz de usuario de.
1. SPA AEM SPA El proyecto __WKND App__ es el proyecto que se va a integrar con el Editor de de la aplicación de

## Último código

+ El punto de partida del código de este tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial), en la carpeta `remote-spa-tutorial`.

## Requisitos previos

Este tutorial requiere lo siguiente:

+ [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip o superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [código fuente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Este tutorial supone lo siguiente:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Un directorio de trabajo de `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ AEM Ejecución del SDK de la como servicio de autor en `http://localhost:4502`
+ AEM Ejecutando el SDK de la con la cuenta `admin` local con la contraseña `admin`
+ SPA Ejecutando el recurso de la cuenta de usuario el `http://localhost:3000`

>[!NOTE]
>
> **¿Necesita ayuda para configurar su entorno de desarrollo local?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).

## AEM SPA 1. Configurar la para el Editor de

AEM SPA AEM SPA Se requieren configuraciones de para integrar el con el Editor de. AEM Estas configuraciones se administran e implementan mediante un proyecto de. En este capítulo, aprenderá qué configuraciones son necesarias y cómo definirlas.

+ [AEM SPA Obtenga información sobre cómo configurar la para el Editor de](./aem-configure.md)

## 2. Bootstrap SPA de la

AEM SPA SPA SPA Para que el Editor de integre un elemento de trabajo en el contexto de creación de un elemento de trabajo, se deben realizar algunas adiciones a la.

+ [SPA AEM SPA Obtenga información sobre cómo arrancar el Editor de reglas de la de la aplicación de inicio](./spa-bootstrap.md)

## 3. Componentes fijos editables

SPA En primer lugar, explore la adición de un &quot;componente fijo&quot; editable a la. SPA Esto ilustra cómo un desarrollador puede colocar un componente editable específico, en la lista de componentes de la lista de componentes de la. Aunque el autor puede cambiar el contenido del componente, no puede quitarlo ni cambiar su ubicación, posición o tamaño.

+ [Obtenga información acerca de los componentes fijos editables](./spa-fixed-component.md)

## 4. Componentes editables del contenedor

SPA A continuación, explore la adición de un &quot;componente contenedor&quot; editable a la. SPA Esto ilustra cómo un desarrollador puede colocar un componente contenedor en la interfaz de usuario de. Los componentes de contenedor permiten a los autores colocar el componente permitido en él y ajustar el diseño de los componentes.

+ [Obtenga información acerca de los componentes editables del contenedor](./spa-container-component.md)

## 5. Rutas dinámicas y componentes editables

Por último, utilice los conceptos explicados en capítulos anteriores para crear rutas dinámicas; rutas que muestren contenido diferente en función del parámetro de la ruta. AEM SPA Esto ilustra cómo se puede utilizar el Editor de código para crear contenido en rutas controladas y derivadas mediante programación.

+ [Obtenga información sobre las rutas dinámicas y los componentes editables](./spa-dynamic-routes.md)

## Recursos adicionales

+ AEM SPA [Componentes editables de React de](https://www.npmjs.com/package/@adobe/aem-react-editable-components)

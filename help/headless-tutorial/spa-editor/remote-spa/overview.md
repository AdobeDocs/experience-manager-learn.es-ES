---
title: Introducción e información general del editor de SPA y a la SPA remota
description: Bienvenido al tutorial de varias partes para desarrolladores que buscan aumentar las SPA remotas existentes con contenido editable de AEM mediante el editor de SPA de AEM.
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
workflow-type: ht
source-wordcount: '571'
ht-degree: 100%

---

# Información general

{{edge-delivery-services}}

Bienvenido al tutorial de varias partes para desarrolladores que buscan aumentar las SPA remotas basadas en una biblioteca React (o Next.js) existente con contenido editable de AEM mediante el editor de SPA de AEM.

Este tutorial se basa en la [aplicación WKND GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=es) React, esta consume contenido de fragmentos de contenido de AEM a través de las API de GraphQL de AEM; sin embargo, no proporciona ninguna creación contextual de contenido de SPA.

>[!VIDEO](https://video.tv.adobe.com/v/3444850?quality=12&learn=on&captions=spa)

## Información sobre el tutorial

El tutorial muestra cómo actualizar una SPA remota o una SPA que se ejecute fuera del contexto de AEM para consumir y enviar contenido creado en AEM.

La mayoría de las actividades del tutorial se centran en el desarrollo de JavaScript; también se tratan aspectos esenciales de AEM. Estos aspectos incluyen la definición de dónde se crea y almacena el contenido en AEM y la asignación de rutas de SPA a páginas de AEM.

El tutorial está diseñado para funcionar con **AEM as a Cloud Service**, y se compone de los dos proyectos siguientes:

1. El __proyecto de AEM__ contiene la configuración y el contenido que se debe implementar en AEM.
1. El proyecto __Aplicación WKND__ es la SPA que se integra con el editor de SPA de AEM

## Último código

+ El punto de partida del código de este tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial), en la carpeta `remote-spa-tutorial`.

## Requisitos previos

Este tutorial requiere lo siguiente:

+ El [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=es)
+ [Node.js, versión 18](https://nodejs.org/es/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip o superior](https://github.com/adobe/aem-guides-wknd/releases)
+ [código fuente aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Este tutorial utiliza lo siguiente:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) como IDE
+ Un directorio de trabajo de `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Ejecución del SDK de AEM como un servicio de Author en `http://localhost:4502`
+ Ejecución del SDK de AEM con la cuenta local `admin` con la contraseña `admin`
+ Ejecución de la SPA en `http://localhost:3000`

>[!NOTE]
>
> **¿Necesita ayuda para configurar su entorno de desarrollo local?** Consulte la[ siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).

## &#x200B;1. Configurar AEM para el editor de SPA

Se requieren configuraciones de AEM para integrar la SPA con el editor de SPA de AEM. Estas configuraciones se administran e implementan mediante un proyecto de AEM. En este capítulo, aprenderá qué configuraciones son necesarias y cómo definirlas.

+ [Obtenga información sobre cómo configurar AEM para el editor de SPA](./aem-configure.md)

## &#x200B;2. Arrancar la SPA

Para que el editor de SPA de AEM integre una SPA en su contexto de creación, se deben realizar algunas adiciones a la SPA.

+ [Obtenga información sobre cómo arrancar la SPA para el editor de SPA de AEM](./spa-bootstrap.md)

## &#x200B;3. Componentes fijos editables

En primer lugar, explore la adición de un “componente fijo” editable a la SPA. Esto ilustra cómo un desarrollador puede colocar un componente editable específico en la SPA. Aunque el autor puede cambiar el contenido del componente, no puede quitarlo ni cambiar su ubicación, posición o tamaño.

+ [Obtenga información sobre los componentes fijos editables](./spa-fixed-component.md)

## &#x200B;4. Componentes editables del contenedor

A continuación, explore la adición de un “componente del contenedor” editable a la SPA. Esto ilustra cómo un desarrollador puede colocar un componente del contenedor en la SPA. Los componentes del contenedor permiten a los autores colocar el componente permitido y ajustar su diseño.

+ [Obtenga información sobre los componentes del contenedor editables](./spa-container-component.md)

## &#x200B;5. Rutas dinámicas y componentes editables

Por último, utilice los conceptos que se explican en los capítulos anteriores para crear rutas dinámicas; estas rutas muestran contenido diferente en función del parámetro de la ruta. Esto ilustra cómo se puede utilizar el editor de SPA de AEM para crear contenido en rutas controladas y derivadas mediante programación.

+ [Obtenga información sobre las rutas dinámicas y los componentes editables](./spa-dynamic-routes.md)

## Recursos adicionales

+ [Componentes editables de React de SPA de AEM](https://www.npmjs.com/package/@adobe/aem-react-editable-components)

---
title: Introducción al Editor de SPA de AEM y reacción
description: Cree la primera aplicación de una sola página de reacción (SPA) que se pueda editar en Adobe Experience Manager AEM con el WKND SPA. Obtenga información sobre cómo crear un SPA mediante el marco de trabajo React JS con AEM editor de SPA. Este tutorial en varias partes explica la implementación de una aplicación React para una marca de estilo de vida ficticia, la WKND. El tutorial cubre la creación de extremo a extremo del SPA y la integración con AEM.
sub-product: sitios
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 13%

---


# Cree su primer SPA de Reacción en AEM {#overview}

Bienvenido a un tutorial de varias partes diseñado para desarrolladores nuevos en la función Editor **de** SPA en Adobe Experience Manager (AEM). Este tutorial explica la implementación de una aplicación React para una marca ficticia de estilo de vida, la WKND. La aplicación React se desarrollará y se diseñará para implementarse con AEM editor de SPA, que asigna los componentes de React a AEM componentes. El SPA completado, implementado en AEM, se puede crear dinámicamente con herramientas de edición en línea tradicionales de AEM.

![Se ha implementado el SPA final](assets/wknd-spa-implementation.png)

*Implementación de WKND SPA*

## Acerca de

El objetivo de este tutorial en varias partes es enseñar a los desarrolladores cómo implementar una aplicación React para trabajar con la función Editor de SPA de AEM. En un escenario real, las actividades de desarrollo se desglosan por persona, a menudo con la participación de un desarrollador **de** Front End y un desarrollador **de** Back End. Creemos que es beneficioso para cualquier desarrollador que esté involucrado en un proyecto AEM Editor de SPA completar este tutorial.

El tutorial está diseñado para funcionar con **AEM como Cloud Service** y es compatible con **AEM 6.5.4+** y **AEM 6.4.8+**. La SPA se implementa mediante:

* [Arquetipo del proyecto Maven AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/overview.html)
* [Editor de SPA AEM](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [React JS](https://reactjs.org/)
* [Crear aplicación de reacción](https://create-react-app.dev/)

*Calcule entre 1 y 2 horas para ver cada parte del tutorial.*

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [última base](https://github.com/adobe/aem-guides-wknd-spa/releases) de código está disponible como Paquetes AEM descargables.

## Requisitos previos

Antes de iniciar este tutorial, necesitará lo siguiente:

* Un conocimiento básico de HTML, CSS y JavaScript
* Familiaridad básica con [React](https://reactjs.org/tutorial/tutorial.html)
* [AEM como Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/)

*Si bien no es necesario, resulta beneficioso tener una comprensión básica del[desarrollo de componentes](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)tradicionales de AEM Sites.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan con el AEM como SDK de Cloud Service que se ejecuta en un entorno de Mac OS con código [de](https://code.visualstudio.com/) Visual Studio como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://docs.adobe.com/content/help/es-ES/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)de desarrollo local.

## Próximos pasos {#next-steps}

¡¿Qué estás esperando?! Inicio el tutorial en el capítulo Proyecto [Editor de](create-project.md) SPA y aprenda a generar un proyecto habilitado para Editor de SPA con el arquetipo de proyecto de AEM.

## Compatibilidad con versiones anteriores {#compatibility}

El código de proyecto de este tutorial se creó para AEM como Cloud Service. Para hacer que el código del proyecto sea compatible con versiones anteriores para **6.5.4+** y **6.4.8+** , se han realizado varias modificaciones en los archivos POM del tutorial.

El [UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** se ha incluido como una dependencia:

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

Se ha agregado un perfil Maven adicional, denominado `classic` para modificar la compilación en entornos de destinatario AEM 6.x:

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

El `classic` perfil está desactivado de forma predeterminada. Si sigue el tutorial con AEM 6.x, añada el `classic` perfil siempre que se le indique que realice una compilación Maven, es decir:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Cuando genere un nuevo proyecto para una implementación AEM, utilice siempre la versión más reciente del [AEM Arquetipo](https://github.com/adobe/aem-project-archetype) de proyecto y actualice el `aemVersion` para destinatario de la versión de AEM deseada.

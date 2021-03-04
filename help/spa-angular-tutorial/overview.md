---
title: Introducción al Editor de SPA y Angular de AEM
description: Cree su primera aplicación de una sola página angular (SPA) que se pueda editar en Adobe Experience Manager, AEM con el SPA WKND. Obtenga información sobre cómo crear una SPA mediante el marco de JS angular con el Editor de SPA de AEM. Este tutorial en varias partes explica la implementación de una aplicación Angular para una marca ficticia de estilo de vida, la WKND. El tutorial cubre la creación de extremo a extremo de la SPA y la integración con AEM.
sub-product: sitios
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: Editor SPA
topic: SPA
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 14%

---


# Cree su primer SPA angular en AEM {#introduction}

Le damos la bienvenida a un tutorial en varias partes diseñado para desarrolladores que utilicen la función **SPA Editor** en Adobe Experience Manager (AEM) de forma nueva. Este tutorial recorre la implementación de una aplicación Angular para una marca ficticia de estilo de vida, la WKND. La aplicación Angular se desarrollará y se diseñará para implementarse con el Editor SPA de AEM, que asigna componentes Angular a componentes AEM. El SPA completado, implementado en AEM, se puede crear dinámicamente con las herramientas de edición en línea tradicionales de AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementación de WKND SPA*

## Acerca de

El objetivo de este tutorial de varias partes es enseñar a un desarrollador cómo implementar una aplicación angular para trabajar con la función Editor de SPA de AEM. En un escenario real, las actividades de desarrollo se desglosan por persona, con frecuencia involucrando a un **desarrollador de Front End** y a un **desarrollador de Back End**. Creemos que es beneficioso para cualquier desarrollador que esté involucrado en un proyecto del Editor de SPA de AEM completar este tutorial.

El tutorial está diseñado para funcionar con **AEM as a Cloud Service** y es compatible con **AEM 6.5.4+** y **AEM 6.4.8+**. La SPA se implementa mediante:

* [Tipo de archivo del proyecto AEM Maven](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/overview.html)
* [Editor de SPA de AEM](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*Calcule entre 1 y 2 horas para pasar por cada parte del tutorial.*

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

El [código base más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes AEM descargables.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Un conocimiento básico de HTML, CSS y JavaScript
* Familiaridad básica con [Angular](https://angular.io/)
* [SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) de AEM as a Cloud Service,  [AEM 6.5.4+ ](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.](https://nodejs.org/en/) jand  [npm](https://www.npmjs.com/)

*Aunque no es necesario, es beneficioso tener una comprensión básica del  [desarrollo de los componentes](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) tradicionales de AEM Sites.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan utilizando el SDK de AEM as a Cloud Service que se ejecuta en un entorno Mac OS con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es novato en AEM 6.5?** Consulte la  [siguiente guía para configurar un entorno](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desarrollo local.

## Pasos siguientes {#next-steps}

¡¿Qué estás esperando?! Inicie el tutorial navegando hasta el capítulo [SPA Editor Project](create-project.md) y aprenda a generar un proyecto habilitado para SPA Editor utilizando el tipo de archivo del proyecto AEM.

## Compatibilidad con versiones anteriores {#compatibility}

El código de proyecto de este tutorial se ha creado para AEM as a Cloud Service. Para que el código del proyecto sea compatible con versiones anteriores para **6.5.4+** y **6.4.8+** se han realizado varias modificaciones.

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

Se ha añadido un perfil Maven adicional, denominado `classic`, para modificar la compilación y dirigirla a los entornos AEM 6.x:

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

El perfil `classic` está deshabilitado de forma predeterminada. Si sigue el tutorial con AEM 6.x, añada el perfil `classic` siempre que se le indique que realice una compilación de Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Al generar un nuevo proyecto para una implementación de AEM, utilice siempre la versión más reciente del [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) y actualice el `aemVersion` para dirigirse a la versión deseada de AEM.

---
title: Introducción al Editor de SPA y Angular de AEM
description: Cree la primera aplicación de una sola página (SPA) de Angular que se pueda editar en Adobe Experience Manager AEM con la SPA de WKND.
version: Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 14%

---

# Creación de su primer Angular SPA AEM de en la {#introduction}

{{edge-delivery-services}}

Bienvenido a un tutorial de varias partes diseñado para desarrolladores nuevos en la **SPA Editor de** función en Adobe Experience Manager AEM (). Este tutorial explora la implementación de una aplicación de Angular para una marca ficticia de estilo de vida, la WKND. La aplicación de Angular AEM SPA está desarrollada y diseñada para implementarse con el Editor de, que asigna componentes de Angular AEM a componentes de la. SPA AEM AEM Los segmentos completados, implementados para la creación de informes de forma dinámica, se pueden crear con las herramientas de edición en línea tradicionales de la creación de informes de la versión en tiempo de ejecución de la versión en tiempo de ejecución de la.

![SPA Implementación final de](assets/wknd-spa-implementation.png)

*SPA Implementación de WKND*

## Acerca de

El objetivo de este tutorial de varias partes es enseñar a un desarrollador cómo implementar una aplicación de Angular SPA AEM para trabajar con la función Editor de de. En un escenario real, las actividades de desarrollo se desglosan por persona, lo que a menudo implica un **Desarrollador front-end** y una **Desarrollador back-end**. AEM SPA Creemos que es beneficioso para cualquier desarrollador involucrado en un proyecto de Editor de completar este tutorial.

El tutorial está diseñado para trabajar con **AEM as a Cloud Service** y es compatible con **AEM.5.4+** y **AEM.4.8+**. SPA La aplicación de la se realiza mediante:

* [Arquetipo del proyecto Maven de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es)
* [AEM SPA Editor de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [Angular](https://angular.io/)

*Calcule entre 1 y 2 horas para completar cada parte del tutorial.*

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

El [última base de código](https://github.com/adobe/aem-guides-wknd-spa/releases) AEM está disponible como paquetes de descarga de la aplicación de descarga de.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Conocimientos básicos de HTML, CSS y JavaScript
* Familiaridad básica con [Angular](https://angular.io/)
* [AEM SDK as a Cloud Service de](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [AEM.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/)

*Si bien no es necesario, es beneficioso tener una comprensión básica de [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. AEM Las capturas de pantalla y los vídeos se capturan mediante el SDK as a Cloud Service de que se ejecuta en un entorno de sistema operativo Mac con [Código de Visual Studio](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> **AEM ¿Nuevo en la versión 6.5 de?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Siguientes pasos {#next-steps}

¿Qué estás esperando?! Inicie el tutorial navegando hasta [SPA Proyecto de editor de](create-project.md) SPA AEM y aprenda a generar un proyecto habilitado para el Editor de mediante el Tipo de archivo del proyecto de.

## Compatibilidad con versiones anteriores {#compatibility}

AEM El código de proyecto de este tutorial se ha creado para su uso en el as a Cloud Service de la. Para que el código del proyecto sea compatible con versiones anteriores de **6.5.4+** y **6.4.8+** se han realizado varias modificaciones.

El [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **Versión 6.4.4** se ha incluido como dependencia:

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

Un perfil de Maven adicional, denominado `classic` AEM se ha agregado para modificar la compilación en entornos de target 6.x:

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

El `classic` El perfil de está desactivado de forma predeterminada. AEM Si está siguiendo el tutorial con la versión 6.x, añada la variable `classic` perfil cada vez que se le indique que realice una compilación de Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM Cuando genere un nuevo proyecto para una implementación de, utilice siempre la versión más reciente de [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) y actualice el `aemVersion` AEM para dirigirse a la versión de destino de la que está.

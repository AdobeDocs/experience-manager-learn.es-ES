---
title: Introducción al editor de SPA de AEM y Angular
description: Cree la primera aplicación de una sola página (SPA) de Angular que se pueda editar en Adobe Experience Manager AEM con la SPA de WKND.
version: Experience Manager as a Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 100%

---

# Creación de su primera SPA de Angular en AEM {#introduction}

{{edge-delivery-services}}

Bienvenido a un tutorial en varias partes diseñado para desarrolladores que se inician en la función **Editor SPA** de Adobe Experience Manager (AEM). Este tutorial explica la implementación de la aplicación Angular para la marca ficticia de estilo de vida, WKND. La aplicación de Angular se ha desarrollado y diseñado para implementarse con el editor de SPA de AEM, que asigna componentes de Angular a componentes de AEM. La SPA completada, implementada en AEM, se puede crear dinámicamente con las herramientas tradicionales de edición en línea de AEM.

![Se implementó la SPA final](assets/wknd-spa-implementation.png)

*Implementación de SPA de WKND*

## Acerca de

El objetivo de este tutorial en varias partes es enseñar a los desarrolladores a implementar una aplicación de Angular para trabajar con la función del editor de SPA de AEM. En un escenario real, las actividades de desarrollo se desglosan por persona y a menudo involucran un **desarrollador front-end** y un **desarrollador back-end**. El completar este tutorial resulta beneficioso para cualquier desarrollador que participe en un proyecto con el editor de SPA de AEM.

El tutorial está diseñado para trabajar con **AEM as a Cloud Service**, además es compatible con versiones anteriores de **AEM 6.5.4+** y **AEM 6.4.8+**. La SPA se implementa usando lo siguiente:

* El [Arquetipo del proyecto Maven de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es)
* y el [editor de SPA de AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=es#content-editing-experience-with-spa)
* [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [Angular](https://angular.io/)

*Calcule entre 1 y 2 horas para completar cada parte del tutorial.*

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base de código más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes descargables de AEM.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Comprender los conceptos básicos de HTML, CSS y JavaScript
* Familiaridad básica con [Angular](https://angular.io/)
* [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=es#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html?lang=es#65) o [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html?lang=es#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/es/) y [npm](https://www.npmjs.com/)

*Aunque no es necesario, es útil tener un conocimiento básico del [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y los vídeos se capturan con el SDK de AEM as a Cloud Service, que se ejecuta en el entorno del SO de Mac con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> **¿Es nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Siguientes pasos {#next-steps}

Inicie ya el tutorial en el capítulo [Proyecto de editor de SPA](create-project.md) para generar un proyecto habilitado con el editor de SPA mediante el arquetipo de proyecto de AEM.

## Compatibilidad con versiones anteriores {#compatibility}

El código de proyecto de este tutorial se ha creado para AEM as a Cloud Service. Se han realizado varias modificaciones para que el código del proyecto sea compatible con versiones anteriores para **6.5.4+** y **6.4.8+**.

[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=es#what-is-the-uberjar), **versión 6.4.4** se ha incluido como dependencia:

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

Se ha añadido un perfil de Maven adicional, denominado `classic`, para modificar la versión y dirigirla a los entornos de AEM 6.x de destino:

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

El perfil `classic` está desactivado de forma predeterminada. Si sigue el tutorial con AEM 6.x, añada el perfil `classic` cada vez que se le indique que realice una compilación de Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Al generar un nuevo proyecto para una implementación de AEM, use siempre la última versión del [arquetipo del proyecto AEM](https://github.com/adobe/aem-project-archetype) y actualice `aemVersion` para que se adapte a la versión de AEM deseada.

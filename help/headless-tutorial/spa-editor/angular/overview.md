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

SPA Bienvenido a un tutorial de varias partes diseñado para desarrolladores nuevos en la función **Editor** de de Adobe Experience Manager AEM. Este tutorial explora la implementación de una aplicación de Angular para una marca ficticia de estilo de vida, la WKND. La aplicación de Angular AEM SPA está desarrollada y diseñada para implementarse con el Editor de de trabajo de, que asigna componentes de Angular AEM a componentes de la. SPA AEM AEM Los segmentos completados, implementados para la creación de informes de forma dinámica, se pueden crear con las herramientas de edición en línea tradicionales de la creación de informes de la versión en tiempo de ejecución de la versión en tiempo de ejecución de la.

SPA ![Se Implementó La Versión Final De La Implementación](assets/wknd-spa-implementation.png)

SPA *Implementación de WKND*

## Acerca de

El objetivo de este tutorial de varias partes es enseñar a un desarrollador cómo implementar una aplicación de Angular SPA AEM para trabajar con la función Editor de de. En un escenario real, las actividades de desarrollo se desglosan por persona, a menudo involucrando a un **desarrollador front-end** y a un **desarrollador back-end**. AEM SPA Creemos que es beneficioso para cualquier desarrollador involucrado en un proyecto de Editor de completar este tutorial.

El tutorial está diseñado para funcionar con **AEM as a Cloud Service AEM AEM** y es compatible con **6.5.4+** y **6.4.8+**. SPA La aplicación de la se realiza mediante:

* [Arquetipo del proyecto Maven de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es)
* AEM SPA [Editor de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [Angular](https://angular.io/)

*Calcular de 1 a 2 horas para completar cada parte del tutorial.*

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

AEM La [base de código más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes de código descargables de la aplicación de descarga de la aplicación de código de la aplicación de código de.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Conocimientos básicos de HTML, CSS y JavaScript
* Familiaridad básica con [Angular](https://angular.io/)
* [SDK de AEM as a Cloud Service AEM AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/)

*Aunque no es necesario, es beneficioso tener una comprensión básica del [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y los vídeos se capturan mediante el SDK de AEM as a Cloud Service que se ejecuta en un entorno de sistema operativo Mac con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> AEM **Nuevo a la versión 6.5 de la?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Siguientes pasos {#next-steps}

¿Qué estás esperando?! SPA SPA AEM Inicie el tutorial navegando hasta el capítulo [Editor de proyectos](create-project.md) y aprenda a generar un proyecto habilitado para el editor de contenido mediante el tipo de archivo del proyecto.

## Compatibilidad con versiones anteriores {#compatibility}

El código de proyecto de este tutorial se ha creado para AEM as a Cloud Service. Para que el código del proyecto sea compatible con versiones anteriores para **6.5.4+** y **6.4.8+**, se han realizado varias modificaciones.

[UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** se ha incluido como dependencia:

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

AEM Se ha agregado un perfil de Maven adicional, denominado `classic`, para modificar la compilación a los entornos de destino de la versión 6.x:

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

El perfil `classic` está deshabilitado de manera predeterminada. AEM Si está siguiendo el tutorial con la versión 6.x, agregue el perfil `classic` siempre que se le indique que realice una compilación de Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM AEM AEM Al generar un nuevo proyecto para una implementación de, utilice siempre la última versión del [Arquetipo de proyecto](https://github.com/adobe/aem-project-archetype) y actualice `aemVersion` para que se dirija a la versión de la que desea que sea el tipo de archivo de la versión de la aplicación {}.

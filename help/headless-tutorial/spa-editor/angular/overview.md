---
title: Introducción al Editor de SPA y Angular de AEM
description: Cree la primera aplicación de una sola página (SPA) de Angular que se pueda editar en Adobe Experience Manager AEM con la SPA de WKND.
sub-product: sites
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: 825124bc6c3be10e6822fb5fb8bd9645d242da76
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 18%

---

# Cree su primer SPA de Angular en AEM {#introduction}

Le damos la bienvenida a un tutorial de varias partes diseñado para desarrolladores que no tienen experiencia con el **Editor SPA** en Adobe Experience Manager (AEM). Este tutorial explica la implementación de una aplicación de Angular para una marca ficticia de estilo de vida, la WKND. La aplicación de Angular se desarrollará y se diseñará para implementarse con AEM Editor de SPA, que asigna componentes de Angular a componentes de AEM. El SPA completado, implementado en AEM, se puede crear dinámicamente con herramientas de edición en línea tradicionales de AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementación de WKND SPA*

## Acerca de

El objetivo de este tutorial de varias partes es enseñar a un desarrollador cómo implementar una aplicación de Angular para trabajar con la función SPA Editor de AEM. En un escenario real, las actividades de desarrollo se desglosan por persona, a menudo implicando a un **Desarrollador de front-end** y **Desarrollador de back-end**. Creemos que es beneficioso para cualquier desarrollador que esté involucrado en un proyecto AEM Editor SPA completar este tutorial.

El tutorial está diseñado para funcionar con **AEM as a Cloud Service** y es compatible con **AEM 6.5.4+** y **AEM 6.4.8+**. El SPA se implementa mediante:

* [Tipo de archivo del proyecto Maven AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es)
* [AEM SPA Editor](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [Angular](https://angular.io/)

*Calcule entre 1 y 2 horas para pasar por cada parte del tutorial.*

## Último código

Todo el código del tutorial se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La variable [base de código más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes AEM descargables.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Un conocimiento básico de HTML, CSS y JavaScript
* Familiaridad básica con [Angular](https://angular.io/)
* [SDK as a Cloud Service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/)

*Aunque no es necesario, es beneficioso tener una comprensión básica de [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan mediante el SDK as a Cloud Service de AEM que se ejecuta en un entorno del sistema operativo Mac con [Código de Visual Studio](https://code.visualstudio.com/) como el IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Siguientes pasos {#next-steps}

¡¿Qué estás esperando?! Inicie el tutorial navegando hasta el [Proyecto del Editor SPA](create-project.md) y aprenda a generar un proyecto habilitado para Editor de SPA con el tipo de archivo del proyecto de AEM.

## Compatibilidad con versiones anteriores {#compatibility}

El código de proyecto de este tutorial se creó para AEM as a Cloud Service. Para que el código del proyecto sea compatible con versiones anteriores de **6.5.4+** y **6.4.8+** se han realizado varias modificaciones.

La variable [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **Versión 6.4.4** se ha incluido como dependencia:

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

Un perfil Maven adicional, denominado `classic` se ha añadido para modificar la compilación para dirigirse a AEM entornos 6.x:

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

La variable `classic` perfil está desactivado de forma predeterminada. Si sigue el tutorial con AEM 6.x, añada la variable `classic` siempre que se le indique que realice una compilación de Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Cuando genere un nuevo proyecto para una implementación AEM, utilice siempre la versión más reciente de [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) y actualice la variable `aemVersion` para dirigirse a la versión de AEM prevista.

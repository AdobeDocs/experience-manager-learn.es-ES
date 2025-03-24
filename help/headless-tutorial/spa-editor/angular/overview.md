---
title: Introducción al Editor de SPA y Angular de AEM
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
ht-degree: 14%

---

# Creación de su primera SPA de Angular en AEM {#introduction}

{{edge-delivery-services}}

Bienvenido a un tutorial de varias partes diseñado para desarrolladores que utilicen la función **Editor de SPA** en Adobe Experience Manager (AEM). Este tutorial explora la implementación de una aplicación de Angular para una marca ficticia de estilo de vida, WKND. La aplicación de Angular se ha desarrollado y diseñado para implementarse con el Editor de SPA de AEM, que asigna componentes de Angular a componentes de AEM. La SPA completada, implementada en AEM, se puede crear dinámicamente con las herramientas tradicionales de edición en línea de AEM.

![Se implementó la SPA final](assets/wknd-spa-implementation.png)

*Implementación de SPA de WKND*

## Acerca de

El objetivo de este tutorial de varias partes es enseñar a un desarrollador cómo implementar una aplicación de Angular para trabajar con la función Editor de SPA de AEM. En un escenario real, las actividades de desarrollo se desglosan por persona, a menudo involucrando a un **desarrollador front-end** y a un **desarrollador back-end**. Creemos que es beneficioso para cualquier desarrollador involucrado en un proyecto de Editor de SPA de AEM completar este tutorial.

El tutorial está diseñado para funcionar con **AEM as a Cloud Service** y es compatible con **AEM 6.5.4+** y **AEM 6.4.8+**. La SPA se implementa mediante:

* [Arquetipo del proyecto Maven de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es)
* [Editor de SPA de AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [Angular](https://angular.io/)

*Calcular de 1 a 2 horas para completar cada parte del tutorial.*

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base de código más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes descargables de AEM.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Conocimientos básicos de HTML, CSS y JavaScript
* Familiaridad básica con [Angular](https://angular.io/)
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) o [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/)

*Aunque no es necesario, es beneficioso tener una comprensión básica del [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y los vídeos se capturan con AEM as a Cloud Service SDK, que se ejecuta en un entorno de sistema operativo Mac con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> **Nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Siguientes pasos {#next-steps}

¿Qué estás esperando?! Inicie el tutorial navegando hasta el capítulo [Proyecto de editor de SPA](create-project.md) y aprenda a generar un proyecto habilitado para el editor de SPA mediante el arquetipo de proyecto de AEM.

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

Se ha agregado un perfil Maven adicional, denominado `classic`, para modificar la compilación y dirigirla a los entornos de AEM 6.x de destino:

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

El perfil `classic` está deshabilitado de manera predeterminada. Si sigue el tutorial con AEM 6.x, agregue el perfil `classic` cada vez que se le indique que realice una compilación de Maven:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Al generar un nuevo proyecto para una implementación de AEM, use siempre la última versión del [tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) y actualice `aemVersion` para que se adapte a la versión de AEM deseada.

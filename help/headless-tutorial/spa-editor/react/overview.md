---
title: Introducción al editor de SPA para AEM y React
description: Cree la primera aplicación de una sola página (SPA) en React que se pueda editar en Adobe Experience Manager AEM con la SPA de WKND. Aprenda a crear una SPA con el marco de trabajo de React JS con AEM SPA Editor. Este tutorial en varias partes explica la implementación de una aplicación de React para una marca ficticia de estilo de vida, WKND. El tutorial cubre la creación de extremo a extremo del SPA y la integración con AEM.
version: Experience Manager as a Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 71
hide: true
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 86%

---

# Creación de su primer SPA de React en AEM {#overview}

{{spa-editor-deprecation}}

Bienvenido a un tutorial en varias partes diseñado para desarrolladores que se inician en la función **Editor SPA** de Adobe Experience Manager (AEM). Este tutorial explica la implementación de una aplicación de React para una marca ficticia de estilo de vida, WKND. La aplicación React se ha desarrollado y diseñado para implementarse con el Editor de SPA de AEM, que asigna los componentes React a los componentes de AEM. La SPA completada, implementada en AEM, se puede crear dinámicamente con las herramientas tradicionales de edición en línea de AEM.

![Se implementó la SPA final](assets/wknd-spa-implementation.png)

*Implementación de SPA de WKND*

## Acerca de

El tutorial está concebido para funcionar con **AEM as a Cloud Service**, compatible con **AEM 6.5.4+** y **AEM 6.4.8+**.

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base de código más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes descargables de AEM.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Comprender los conceptos básicos de HTML, CSS y JavaScript
* Familiaridad básica con [React](https://reactjs.org/tutorial/tutorial.html?lang=es)

*Aunque no es necesario, es útil tener un conocimiento básico del [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y los vídeos se capturan con el SDK de AEM as a Cloud Service, que se ejecuta en el entorno del SO de Mac con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

* [SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=es), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=es#aem-65) o [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=es#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/es/) y [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Nuevo en AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).
>
> **Nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Próximos pasos {#next-steps}

¿Qué estás esperando?! Inicie el tutorial navegando hasta el capítulo [Crear proyecto](create-project.md) y aprenda a generar un proyecto con Editor SPA habilitado mediante el arquetipo de proyecto de AEM.

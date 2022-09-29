---
title: Introducción al Editor de SPA de AEM y React
description: Cree la primera aplicación de una sola página (SPA) en React que se pueda editar en Adobe Experience Manager AEM con la SPA de WKND. Aprenda a crear una SPA con el marco de trabajo de React JS con AEM SPA Editor. Este tutorial en varias partes explica la implementación de una aplicación de React para una marca ficticia de estilo de vida, WKND. El tutorial cubre la creación de extremo a extremo del SPA y la integración con AEM.
sub-product: sites
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 32%

---

# Cree su primer SPA React en AEM {#overview}

Le damos la bienvenida a un tutorial de varias partes diseñado para desarrolladores que no tienen experiencia con el **Editor SPA** en Adobe Experience Manager (AEM). Este tutorial explica la implementación de una aplicación React para una marca ficticia de estilo de vida, la WKND. La aplicación React está desarrollada y diseñada para implementarse con AEM Editor SPA, que asigna componentes de React a componentes AEM. El SPA completado, implementado en AEM, se puede crear dinámicamente con herramientas de edición en línea tradicionales de AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementación de WKND SPA*

## Acerca de

El tutorial está diseñado para funcionar con **AEM as a Cloud Service** y es compatible con **AEM 6.5.4+** y **AEM 6.4.8+**.

## Último código

Todo el código del tutorial se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La variable [base de código más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes AEM descargables.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Un conocimiento básico de HTML, CSS y JavaScript
* Familiaridad básica con [React](https://reactjs.org/tutorial/tutorial.html)

*Aunque no es necesario, es beneficioso tener una comprensión básica de [desarrollo de componentes tradicionales de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es).*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan mediante el SDK as a Cloud Service de AEM que se ejecuta en un entorno del sistema operativo Mac con [Código de Visual Studio](https://code.visualstudio.com/) como el IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

* [SDK as a Cloud Service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) o [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.js](https://nodejs.org/en/) y [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Siguientes pasos {#next-steps}

¡¿Qué estás esperando?! Inicie el tutorial navegando hasta el [Crear proyecto](create-project.md) y aprenda a generar un proyecto habilitado para Editor de SPA con el tipo de archivo del proyecto de AEM.

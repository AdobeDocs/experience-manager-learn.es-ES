---
title: Introducción al Editor de SPA de AEM y React
description: Cree la primera aplicación de una sola página de React (SPA) que se pueda editar en Adobe Experience Manager AEM con la SPA WKND. Aprenda a crear una SPA utilizando el marco de React JS con AEM Editor SPA. Este tutorial en varias partes explica la implementación de una aplicación React para una marca ficticia de estilo de vida, la WKND. El tutorial cubre la creación de extremo a extremo del SPA y la integración con AEM.
sub-product: sitios
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: Editor SPA
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 12%

---


# Cree su primer SPA React en AEM {#overview}

Le damos la bienvenida a un tutorial en varias partes diseñado para desarrolladores que utilicen la función **SPA Editor** de Adobe Experience Manager (AEM) de forma nueva. Este tutorial explica la implementación de una aplicación React para una marca ficticia de estilo de vida, la WKND. La aplicación React se desarrollará y se diseñará para implementarse con AEM Editor SPA, que asigna componentes de React a componentes AEM. El SPA completado, implementado en AEM, se puede crear dinámicamente con herramientas de edición en línea tradicionales de AEM.

![SPA final implementado](assets/wknd-spa-implementation.png)

*Implementación de WKND SPA*

## Acerca de

El tutorial está diseñado para funcionar con **AEM como Cloud Service** y es compatible con **AEM 6.5.4+** y **AEM 6.4.8+**.

## Último código

Todo el código del tutorial se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

El [código base más reciente](https://github.com/adobe/aem-guides-wknd-spa/releases) está disponible como paquetes AEM descargables.

## Requisitos previos

Antes de iniciar este tutorial, necesita lo siguiente:

* Un conocimiento básico de HTML, CSS y JavaScript
* Familiaridad básica con [React](https://reactjs.org/tutorial/tutorial.html)

*Aunque no es necesario, resulta beneficioso tener una comprensión básica del  [desarrollo de componentes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) tradicionales de AEM Sites.*

## Entorno de desarrollo local {#local-dev-environment}

Se necesita un entorno de desarrollo local para completar este tutorial. Las capturas de pantalla y el vídeo se capturan usando el SDK de AEM as a Cloud Service que se ejecuta en un entorno de Mac OS con [Visual Studio Code](https://code.visualstudio.com/) como IDE. Los comandos y el código deben ser independientes del sistema operativo local, a menos que se indique lo contrario.

### Software necesario

* [AEM como SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html) de Cloud Service,  [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) o  [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 o posterior)
* [Node.](https://nodejs.org/en/) jand  [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **¿Es nuevo en AEM 6.5?** Consulte la  [siguiente guía para configurar un entorno](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) de desarrollo local.

## Pasos siguientes {#next-steps}

¡¿Qué estás esperando?! Inicie el tutorial navegando hasta el capítulo [Crear proyecto](create-project.md) y aprenda a generar un proyecto habilitado para SPA Editor mediante el tipo de archivo AEM proyecto.

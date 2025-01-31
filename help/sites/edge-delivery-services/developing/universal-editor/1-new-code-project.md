---
title: Creación de un proyecto de código
description: Cree un proyecto de código para Edge Delivery Services, editable con el editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 176be4209409763b69ceff5e893d0e857efa6d30
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Creación de un proyecto de código de Edge Delivery Services

AEM Para crear sitios web de la plantilla para Edge Delivery Services y el editor universal, usa la plantilla de proyecto [XWalk de plantillas de Adobe AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Esta plantilla crea un nuevo proyecto de código que contiene el CSS y el JavaScript utilizados para crear la experiencia del sitio web. Esta plantilla crea un nuevo repositorio de GitHub y lo carga con el código de plantillas y la configuración de Adobe AEM, lo que proporciona una base sólida para su proyecto de sitio web de la comunidad de.

AEM Recuerde, [los sitios web de la comunidad de usuarios que envían los Edge Delivery Services](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) solo tienen código del lado del cliente (explorador). AEM El código del sitio web no se ejecuta en los servicios de Autor de o Publish.

![Nuevo proyecto para Edge Delivery Services](./assets/1-new-project/new-project.png)

Siga los [pasos detallados descritos en la documentación](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) para crear un proyecto de código de Edge Delivery Services cuyo contenido se pueda editar en el editor universal.  A continuación se muestra una lista resumida de los pasos, incluidos los valores utilizados en este tutorial.

1. **Configurar una cuenta de GitHub.** Si está creando un proyecto para su organización, asegúrese de que la organización tenga una cuenta de GitHub y de que usted sea miembro.
2. AEM **Cree un nuevo proyecto de código** con la [plantilla de proyecto XWalk de plantillas de código ](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. AEM **Instale la aplicación GitHub de sincronización de código de** y conceda acceso al repositorio. Puedes encontrar la [aplicación aquí](https://github.com/apps/aem-code-sync).
4. AEM **Configure el`fstab.yaml`** de su nuevo proyecto para que apunte al servicio de autor de correcto.

   * Para experimentar, puede utilizar entornos de AEM as a Cloud Service AEM más bajos (Fase o Desarrollo), aunque las implementaciones de sitios web reales deben configurarse para utilizar un servicio de producción de.

5. AEM **Edite el`paths.json`** de su nuevo proyecto para asignar la ruta de acceso del servicio de autor de la a la raíz de su sitio web.

Este repositorio Git se clona en el capítulo [entorno de desarrollo local](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) y donde se desarrolla el código.

---
title: Creación de un proyecto de código
description: Cree un proyecto de código para Edge Delivery Services, editable con el editor universal.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Creación de un proyecto de código Edge Delivery Services

Para crear sitios web de AEM para Edge Delivery Services y Universal Editor, usa la [plantilla de proyecto XWalk de plantillas de AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk) de Adobe. Esta plantilla crea un nuevo proyecto de código que contiene el CSS y el JavaScript utilizados para crear la experiencia del sitio web. Esta plantilla crea un nuevo repositorio de GitHub y lo carga con el código de plantillas y la configuración de Adobe, lo que proporciona una base sólida para su proyecto de sitio web de AEM.

Recuerde que los [sitios web de AEM que entrega Edge Delivery Services](https://experienceleague.adobe.com/es/docs/experience-manager-learn/sites/edge-delivery-services/overview) solo tienen código del lado del cliente (explorador). El código del sitio web no se ejecuta en los servicios de AEM Author o Publish.

![Nuevo proyecto de Edge Delivery Services](./assets/1-new-project/new-project.png)

Siga los [pasos detallados descritos en la documentación](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) para crear un proyecto de código Edge Delivery Services cuyo contenido se pueda editar en el editor universal.  A continuación se muestra una lista resumida de los pasos, incluidos los valores utilizados en este tutorial.

1. **Configurar una cuenta de GitHub.** Si está creando un proyecto para su organización, asegúrese de que la organización tenga una cuenta de GitHub y de que usted sea miembro.
2. **Cree un nuevo proyecto de código** con la [plantilla de proyecto XWalk de plantillas de AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Instale la aplicación GitHub de sincronización de código de AEM** y conceda acceso al repositorio. Puedes encontrar la [aplicación aquí](https://github.com/apps/aem-code-sync).
4. **Configure el`fstab.yaml`** de su nuevo proyecto para que apunte al servicio de AEM Author correcto.

   * Para experimentar, puede utilizar entornos de AEM as a Cloud Service más bajos (Fase o Desarrollo); sin embargo, las implementaciones de sitios web reales deben configurarse para utilizar un servicio de AEM de producción.

5. **Edite el`paths.json`** de su nuevo proyecto para asignar la ruta del servicio de AEM Author a la raíz de su sitio web.

Este repositorio Git se clona en el capítulo [entorno de desarrollo local](https://experienceleague.adobe.com/es/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) y donde se desarrolla el código.

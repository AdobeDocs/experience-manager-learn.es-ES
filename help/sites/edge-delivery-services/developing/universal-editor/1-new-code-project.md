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
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 1%

---


# Creación de un proyecto de código de Edge Delivery Services

AEM Para crear sitios web de la plantilla para Edge Delivery Services y el editor universal, usa la plantilla de proyecto [XWalk de plantillas de Adobe AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Esta plantilla crea un nuevo proyecto de código que contiene el CSS y el JavaScript utilizados para crear la experiencia del sitio web. Esta plantilla crea un nuevo repositorio de GitHub y lo carga con el código de plantillas y la configuración de Adobe AEM, lo que proporciona una base sólida para su proyecto de sitio web de la comunidad de.

AEM Recuerde, [los sitios web de la comunidad de usuarios que envían los Edge Delivery Services](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) solo tienen código del lado del cliente (explorador). AEM El código del sitio web no se ejecuta en los servicios de Autor de o Publish.

![Nuevo proyecto para Edge Delivery Services](./assets/1-new-project/new-project.png)

Siga los pasos a continuación para crear un proyecto de código para Edge Delivery Services cuyo contenido se pueda editar en el editor universal:

1. **Configurar una cuenta de GitHub.** Si está creando un proyecto para su organización, asegúrese de que la organización tenga una cuenta de GitHub y de que usted sea miembro.
2. AEM **Cree un nuevo proyecto de código** con la [plantilla de proyecto XWalk de plantillas de código ](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. AEM **Instale la aplicación GitHub de sincronización de código de** y conceda acceso al repositorio. Puedes encontrar la [aplicación aquí](https://github.com/apps/aem-code-sync).
4. AEM **Configure el`fstab.yaml`** de su nuevo proyecto para que apunte al servicio de autor de correcto.

   * Para experimentar, puede utilizar entornos de AEM as a Cloud Service AEM más bajos (Fase, Desarrollo o RDE); sin embargo, las implementaciones de sitios web reales deben configurarse para utilizar un servicio de creación de segmentos de producción (producción).

5. AEM **Edite el`paths.json`** de su nuevo proyecto para asignar la ruta de acceso del servicio de autor de la a la raíz de su sitio web.

Para obtener instrucciones más detalladas, consulte la sección [crear su proyecto de GitHub](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) en la guía de introducción.

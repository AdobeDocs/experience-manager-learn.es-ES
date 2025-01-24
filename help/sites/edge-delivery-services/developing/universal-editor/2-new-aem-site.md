---
title: AEM Creación de un sitio de
description: Cree un sitio en AEM Sites para Edge Delivery Services, editable con el editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# AEM Creación de un sitio de

AEM El sitio de la es desde donde se edita, administra y publica el contenido del sitio web. AEM Para crear un sitio de enviado a través de Edge Delivery Services y creado con el editor universal, use los [Edge Delivery Services AEM AEM con plantilla de sitio de creación de la plantilla de sitio de la creación de la plantilla](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) para crear un nuevo sitio en el Autor de la.

AEM El sitio de la es donde se almacena y crea el contenido del sitio web. AEM La experiencia final es una combinación del contenido del sitio de la con el [código del sitio web](./1-new-code-project.md)

AEM ![Nuevo sitio para Edge Delivery Services y editor universal](./assets/2-new-aem-site/new-site.png) de la lista de distribución de contenido

AEM Siga los pasos a continuación para crear un nuevo sitio de:

1. AEM **Crear un nuevo sitio** en el autor de la. Este tutorial utiliza el siguiente nombre de sitio:
   * Título del sitio: `WKND (Universal Editor)`
   * Nombre del sitio: `aem-wknd-eds-ue`
2. **Importe la plantilla más reciente** de los [Edge Delivery Services AEM con la plantilla del sitio de creación de la plantilla de creación de la aplicación](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Asigne un nombre al sitio** que coincida con el nombre del repositorio de GitHub y establezca la dirección URL de GitHub como dirección URL del repositorio.

AEM Para obtener instrucciones detalladas, consulte [crear y editar una nueva sección de sitio de la lista de distribución de contenido](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) en la guía de introducción a la creación de sitios de la lista de distribución de contenido.

## Publish el nuevo sitio para previsualizar

AEM Después de crear el sitio en el autor de la, publíquelo en la vista previa de los Edge Delivery Services para que el contenido esté disponible para el [entorno de desarrollo local](./3-local-development-environment.md).

1. AEM Inicie sesión en **Autor de la** y navegue hasta **Sitios**.
2. Seleccione el **nuevo sitio** (`WKND (Universal Editor)`) y haga clic en **Administrar publicaciones**.
3. Elija **Vista previa** en **Destinos** y haga clic en **Siguiente**.
4. En **Incluir configuración secundaria**, seleccione **Incluir elementos secundarios**, deseleccione otras opciones y haga clic en **Aceptar**.
5. Haga clic en **Publish** para publicar el contenido del sitio y previsualizarlo.

---
title: Creación de un sitio de AEM
description: Cree un sitio en AEM Sites para Edge Delivery Services editable con el editor universal.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: ae3ade0f31846776aa9bdd3a615d6514b626f48d
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# Creación de un sitio de AEM

En el sitio de AEM es donde se edita, administra y publica el contenido del sitio web. Para crear un sitio de AEM enviado a través de Edge Delivery Services y creado con el editor universal, usa [Edge Delivery Services con la plantilla del sitio de creación de AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) para crear un nuevo sitio en AEM Author.

El sitio de AEM es donde se almacena y crea el contenido del sitio web. La experiencia final es una combinación del contenido del sitio AEM con el código del sitio web [1}.](./1-new-code-project.md)

![Nuevo sitio de AEM para Edge Delivery Services y el editor universal](./assets/2-new-aem-site/new-site.png)

Siga los [pasos detallados descritos en la documentación](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) para crear un nuevo sitio de AEM.  A continuación se muestra una lista resumida de los pasos, incluidos los valores utilizados en este tutorial.
1. **Crear un nuevo sitio** en AEM Author. Este tutorial utiliza el siguiente nombre de sitio:
   * Título del sitio: `WKND (Universal Editor)`
   * Nombre del sitio: `aem-wknd-eds-ue`

      * El valor del nombre de sitio debe coincidir con el nombre de ruta de sitio [agregado a `paths.json`](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping).

2. **Importe la plantilla más reciente** desde [Edge Delivery Services con la plantilla del sitio de creación de AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Asigne un nombre al sitio** que coincida con el nombre del repositorio de GitHub y establezca la dirección URL de GitHub como dirección URL del repositorio.

## Publicar el nuevo sitio para previsualizarlo

Después de crear el sitio en AEM Author, publíquelo en la vista previa de Edge Delivery Services para que el contenido esté disponible para el [entorno de desarrollo local](./3-local-development-environment.md).

1. Inicie sesión en **AEM Author** y vaya a **Sites**.
2. Seleccione el **nuevo sitio** (`WKND (Universal Editor)`) y haga clic en **Administrar publicaciones**.
3. Elija **Vista previa** en **Destinos** y haga clic en **Siguiente**.
4. En **Incluir configuración secundaria**, seleccione **Incluir elementos secundarios**, deseleccione otras opciones y haga clic en **Aceptar**.
5. Haga clic en **Publicar** para publicar el contenido del sitio y previsualizarlo.
6. Una vez publicadas para previsualizarlas, las páginas estarán disponibles en el entorno de vista previa de Edge Delivery Services (las páginas no aparecerán en el servicio de vista previa de AEM).

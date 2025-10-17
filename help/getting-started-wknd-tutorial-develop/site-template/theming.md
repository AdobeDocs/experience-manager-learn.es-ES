---
title: Flujo de trabajo de temáticas | Creación rápida de sitios de AEM
description: Obtenga información sobre cómo actualizar las fuentes de temas de un sitio de Adobe Experience Manager para aplicar estilos específicos de marca. Aprenda a utilizar un servidor proxy para ver una vista previa en directo de las actualizaciones de CSS y Javascript. En este tutorial también se explica cómo implementar actualizaciones de temas en un sitio de AEM mediante la canalización front-end de Adobe Cloud Manager.
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '458'
ht-degree: 100%

---

# Flujo de trabajo de temáticas {#theming}

En este capítulo vamos a actualizar las fuentes temáticas de un sitio Adobe Experience Manager para aplicar estilos específicos de marca. Aprenderemos a utilizar un servidor proxy para ver una vista previa de las actualizaciones de CSS y Javascript a medida que codifiquemos en el sitio activo. En este tutorial también se explica cómo implementar actualizaciones de temas en un sitio de AEM mediante la canalización front-end de Adobe Cloud Manager.

Al final, nuestro sitio se actualiza para incluir estilos que coincidan con la marca WKND.

## Requisitos previos {#prerequisites}

Este es un tutorial que consta de varias partes y se da por hecho que los pasos descritos en el capítulo [Plantillas de página](./page-templates.md) se han completado.

## Objetivos

1. Descubrir cómo se pueden descargar y modificar las fuentes de temas de un sitio.
1. Aprender a codificar el sitio en directo para una vista previa en tiempo real.
1. Comprender el flujo de trabajo completo de envío de actualizaciones compiladas de CSS y JavaScript como parte de un tema mediante la canalización front-end de Adobe Cloud Manager.

## Actualización de un tema {#theme-update}

A continuación, realice cambios en las fuentes de temáticas para que el sitio coincida con el aspecto de la marca WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3453623?captions=spa&quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. Cree un usuario local en AEM para utilizarlo con un servidor de desarrollo proxy.
1. Descargue las fuentes de temas de AEM y ábralas con un IDE local, como VSCode.
1. Modifique las fuentes de temáticas y utilice un servidor de desarrollo proxy para obtener una vista previa de los cambios de CSS y JavaScript en tiempo real.
1. Actualice las fuentes del tema para que el artículo de la revista coincida con los estilos y maquetas de la marca WKND.

### Archivos de solución

Descargue los estilos finalizados para el [Tema de muestra WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Implementación de un tema mediante una canalización front-end {#deploy-theme}

Implemente actualizaciones en un tema en un entorno de AEM mediante la canalización front-end de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. Cree un nuevo repositorio de Git [en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html?lang=es)
1. Añada el proyecto de fuentes temáticas al repositorio de Git de Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configure una [canalización front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=es) en Cloud Manager para implementar el código front-end.
1. Ejecute la canalización front-end para implementar actualizaciones en el entorno de AEM de destino.

### Repositorios de ejemplo

Hay un par de repositorios de GitHub de ejemplo que pueden utilizarse como referencia:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e): se utiliza como ejemplo para proyectos ‘’reales‘’.

## Enhorabuena. {#congratulations}

Enhorabuena. Acaba de actualizar e implementar un tema en AEM.

### Próximos pasos {#next-steps}

Obtenga información más detallada sobre el desarrollo de AEM y comprenda más sobre la tecnología subyacente creando un sitio con el [Arquetipo de proyecto de AEM](../project-archetype/overview.md).

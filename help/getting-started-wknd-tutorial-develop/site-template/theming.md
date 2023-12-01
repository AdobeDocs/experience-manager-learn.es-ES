---
title: Flujo de trabajo de temas AEM | Creación rápida de sitios de
description: Obtenga información sobre cómo actualizar las fuentes de temas de un sitio de Adobe Experience Manager para aplicar estilos específicos de marca. Aprenda a utilizar un servidor proxy para ver una vista previa activa de las actualizaciones de CSS y Javascript. AEM Este tutorial también explicará cómo implementar actualizaciones de temas en un sitio de mediante la canalización front-end de Adobe Cloud Manager.
version: Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 1%

---

# Flujo de trabajo de temas {#theming}

En este capítulo actualizamos las fuentes temáticas de un sitio Adobe Experience Manager para aplicar estilos específicos de marca. Aprendemos a utilizar un servidor proxy para ver una previsualización de las actualizaciones de CSS y Javascript a medida que codificamos en el sitio activo. AEM Este tutorial también explicará cómo implementar actualizaciones de temas en un sitio de mediante la canalización front-end de Adobe Cloud Manager.

Al final, nuestro sitio se actualiza para incluir estilos que coincidan con la marca WKND.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en la sección [Plantillas de página](./page-templates.md) se han completado los capítulos.

## Objetivos

1. Descubra cómo se pueden descargar y modificar las fuentes de temas de un sitio.
1. Aprenda a codificar el sitio en directo para una previsualización en tiempo real.
1. Comprenda el flujo de trabajo completo de entrega de actualizaciones compiladas de CSS y JavaScript como parte de un tema mediante la canalización front-end de Adobe Cloud Manager.

## Actualizar una temática {#theme-update}

A continuación, realice cambios en las fuentes de temas para que el sitio coincida con el aspecto de la marca WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. AEM Cree un usuario local en el entorno de trabajo para utilizarlo con un servidor de desarrollo de proxy.
1. AEM Descargue las fuentes de temas desde el entorno de trabajo y ábralas con un IDE local, como VSCode.
1. Modifique las fuentes de temas y utilice un servidor de desarrollo proxy para previsualizar los cambios de CSS y JavaScript en tiempo real.
1. Actualice las fuentes del tema para que el artículo de la revista coincida con los estilos y maquetas de la marca WKND.

### Archivos de solución

Descargue los estilos finalizados para la [Tema de muestra de WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Implementar un tema mediante una canalización front-end {#deploy-theme}

AEM Implemente actualizaciones de un tema en un entorno de mediante la canalización front-end de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. Crear un nuevo Git [repositorio en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Añada el proyecto de fuentes temáticas al repositorio de Git de Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configurar un [Canalización front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) en Cloud Manager para implementar el código front-end.
1. AEM Ejecute la canalización front-end para implementar actualizaciones en el entorno de destino de la aplicación de destino

### Cesiones temporales de ejemplo

Hay un par de repositorios de GitHub de ejemplo que pueden utilizarse como referencia:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Se utiliza como ejemplo para proyectos del mundo real.

## Enhorabuena. {#congratulations}

AEM ¡Enhorabuena, acaba de actualizar e implementar un tema para la creación de informes

### Pasos siguientes {#next-steps}

AEM Obtenga información más detallada sobre el desarrollo de la y comprenda mejor la tecnología subyacente creando un sitio con la variable [AEM Tipo de archivo del proyecto](../project-archetype/overview.md).

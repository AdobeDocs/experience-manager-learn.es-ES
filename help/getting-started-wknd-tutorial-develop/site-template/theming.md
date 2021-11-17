---
title: Flujo de trabajo temático | Creación rápida de sitios AEM
description: Obtenga información sobre cómo actualizar las fuentes temáticas de un sitio de Adobe Experience Manager para aplicar estilos específicos de marca. Aprenda a utilizar un servidor proxy para ver una previsualización activa de las actualizaciones de CSS y Javascript. Este tutorial también tratará sobre cómo implementar actualizaciones de temas en un sitio AEM mediante la canalización front-end de Adobe Cloud Manager.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---

# Flujo de trabajo temático {#theming}

>[!CAUTION]
>
> La herramienta Creación rápida de sitios es actualmente una vista previa técnica. Está disponible con fines de ensayo y evaluación y no está pensado para su uso en producción a menos que se acuerde con la asistencia al Adobe.

En este capítulo actualizaremos las fuentes temáticas de un sitio de Adobe Experience Manager para aplicar estilos específicos de marca. Aprenderemos a utilizar un servidor proxy para ver una vista previa de las actualizaciones de CSS y Javascript a medida que se codifica en el sitio activo. Este tutorial también tratará sobre cómo implementar actualizaciones de temas en un sitio AEM mediante la canalización front-end de Adobe Cloud Manager.

Al final, nuestro sitio se actualizará para incluir estilos que coincidan con la marca WKND.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en la sección [Plantillas de página](./page-templates.md) se ha completado el capítulo.

## Objetivos

1. Descubra cómo se pueden descargar y modificar las fuentes temáticas de un sitio.
1. Aprenda cómo codificar el sitio activo para obtener una vista previa en tiempo real.
1. Comprenda el flujo de trabajo completo de entrega de actualizaciones de JavaScript y CSS compiladas como parte de un tema mediante la canalización front-end de Adobe Cloud Manager.

## Actualizar un tema {#theme-update}

A continuación, realice cambios en los orígenes de los temas para que el sitio coincida con el aspecto de la marca WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. Cree un usuario local en AEM para utilizarlo con un servidor de desarrollo de proxy.
1. Descargue los orígenes de los temas de AEM y ábralo con un IDE local, como VSCode.
1. Modifique los orígenes de los temas y utilice un servidor de desarrollo proxy para previsualizar los cambios de CSS y JavaScript en tiempo real.
1. Actualice las fuentes temáticas para que el artículo de la revista coincida con los estilos y maquetas de marca WKND.

### Archivos de solución

Descargue los estilos terminados para el [Tema de muestra de WKND](assets/theming/WKND-THEME-src-1.1.zip)

## Implementar un tema mediante una canalización de front-end {#deploy-theme}

Implemente actualizaciones en un tema en un entorno AEM mediante la canalización front-end de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722/?quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. Crear una nueva Git [repositorio en Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Añada su proyecto de fuentes de temas al repositorio de Git de Cloud Manager:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configure un [Canalización front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) en Cloud Manager para implementar el código de front-end.
1. Ejecute la canalización de front-end para implementar actualizaciones en el entorno de AEM de destino.

### Ejemplos de repos

Hay un par de repositorios de GitHub de ejemplo que se pueden usar como referencia:

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Se utiliza como ejemplo para proyectos &quot;reales&quot;.

## Felicitaciones! {#congratulations}

¡Felicidades, acaba de crear y actualizar un tema para AEM!

### Siguientes pasos {#next-steps}

Participe más profundamente en AEM desarrollo y comprenda más de la tecnología subyacente creando un sitio con el [Tipo de archivo del proyecto AEM](../project-archetype/overview.md).

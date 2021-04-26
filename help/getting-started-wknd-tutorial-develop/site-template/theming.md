---
title: Flujo de trabajo temático
seo-title: 'Introducción a AEM Sites: flujo de trabajo temático'
description: Obtenga información sobre cómo actualizar las fuentes temáticas de un sitio de Adobe Experience Manager para aplicar estilos específicos de marca. Aprenda a utilizar un servidor proxy para ver una previsualización activa de las actualizaciones de CSS y Javascript. Este tutorial también tratará sobre cómo implementar actualizaciones de temas en un sitio AEM mediante acciones de GitHub.
sub-product: sitios
version: Cloud Service
type: Tutorial
feature: Componentes principales
topic: Gestión de contenido, desarrollo
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---


# Flujo de trabajo temático {#theming}

>[!CAUTION]
>
> Las funciones de creación rápida de sitios que se muestran aquí se lanzarán en el segundo semestre de 2021. La documentación relacionada está disponible con fines de vista previa.

En este capítulo actualizaremos las fuentes temáticas de un sitio de Adobe Experience Manager para aplicar estilos específicos de marca. Aprenderemos a utilizar un servidor proxy para ver una vista previa de las actualizaciones de CSS y Javascript a medida que se codifica en el sitio activo. Este tutorial también tratará sobre cómo implementar actualizaciones de temas en un sitio AEM mediante acciones de GitHub.

Al final, nuestro sitio se actualizará para incluir estilos que coincidan con la marca WKND.

## Requisitos previos {#prerequisites}

Este es un tutorial en varias partes y se da por hecho que los pasos descritos en el capítulo [Plantillas de página](./page-templates.md) se han completado.

## Objetivos

1. Descubra cómo se pueden descargar y modificar las fuentes temáticas de un sitio.
1. Aprenda cómo codificar el sitio activo para obtener una vista previa en tiempo real.
1. Comprenda el flujo de trabajo completo de entrega de actualizaciones de JavaScript y CSS compiladas como parte de un tema usando las acciones de GitHub.

## Actualizar un tema {#theme-update}

A continuación, realice cambios en los orígenes de los temas para que el sitio coincida con el aspecto de la marca WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. Cree un usuario local en AEM para utilizarlo con un servidor de desarrollo de proxy.
1. Descargue los orígenes de los temas de AEM y ábralo con un IDE local, como VSCode.
1. Modifique los orígenes de los temas y utilice un servidor de desarrollo proxy para previsualizar los cambios de CSS y JavaScript en tiempo real.
1. Actualice las fuentes temáticas para que el artículo de la revista coincida con los estilos y maquetas de marca WKND.

### Archivos de solución

Descargue los estilos terminados para el [Tema de WKND](assets/theming/WKND-THEME-src.zip)

## Implementar un tema {#deploy-theme}

Implemente actualizaciones en un tema en un entorno AEM mediante las acciones de GitHub.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Pasos de alto nivel para el vídeo:

1. Añada el proyecto de fuentes de temas a [GitHub como un nuevo repositorio](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line).
1. Cree [un token de acceso personal en GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) y guárdelo en una ubicación segura.
1. Configure la configuración de GitHub en su entorno de AEM para que apunte a su repositorio de GitHub e incluya su token de acceso personal.
1. Cree los siguientes [secretos cifrados](https://docs.github.com/en/actions/reference/encrypted-secrets) en su repositorio de GitHub:
   * **AEM_SITE** : raíz del sitio AEM (es decir,  `wknd`)
   * **AEM_URL** : url de su entorno de AEM Author
   * **GIT_TOKEN** : token de acceso personal del paso 2.
1. Ejecute la acción de GitHub: **Genere e implemente artefactos de Github**. Esto tendrá el efecto descendente de ejecutar la acción: **Actualice la configuración del tema en AEM con id de artefacto**, que implementará los orígenes del tema en el entorno de AEM especificado por `AEM_URL` y `AEM_SITE`.

### Ejemplos de repos

Hay un par de repositorios de GitHub de ejemplo que se pueden usar como referencia:

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) : se utiliza como ejemplo para proyectos &quot;reales&quot;.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme) : Se utiliza como ejemplo para los usuarios que siguen el tutorial.

## Felicitaciones! {#congratulations}

¡Felicidades, acaba de crear y actualizar un tema para AEM!

### Pasos siguientes {#next-steps}

Dé un paseo más profundo por AEM desarrollo y comprenda más de la tecnología subyacente creando un sitio con el [AEM tipo de archivo del proyecto](../project-archetype/overview.md).

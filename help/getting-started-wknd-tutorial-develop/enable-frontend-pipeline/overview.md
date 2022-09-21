---
title: Habilitar canalización front-end para el tipo de archivo del proyecto AEM estándar
description: Obtenga información sobre cómo habilitar una canalización front-end para un proyecto de AEM estándar para una implementación más rápida de recursos estáticos como CSS, JavaScript, fuentes e iconos. También separación del desarrollo front-end del desarrollo back-end completo del stack en AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# Habilitar canalización front-end para el tipo de archivo del proyecto AEM estándar{#enable-front-end-pipeline-standard-aem-project}

Obtenga información sobre cómo habilitar la variable [AEM proyecto de WKND Sites](https://github.com/adobe/aem-guides-wknd) (también conocido como Standard AEM Project) creado mediante [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) para implementar recursos front-end como CSS, JavaScript, fuentes e iconos mediante una canalización front-end con el fin de acelerar el ciclo de desarrollo e implementación. La separación del desarrollo front-end del desarrollo back-end completo del stack en AEM. También aprenderá cómo son estos recursos front-end __not__ se sirve desde el repositorio de AEM pero desde la CDN, un cambio en el paradigma de envío.


Se crea una nueva canalización front-end en Adobe Cloud Manager que solo genera e implementa `ui.frontend` artefactos a la CDN integrada e informa a AEM sobre su ubicación. En AEM durante la generación de HTML de la página web, la variable `<link>` y `<script>` , consulte esta ubicación de artefactos en la `href` valor de atributo.

Sin embargo, después de la conversión del proyecto WKND Sites AEM , los desarrolladores del front-end pueden trabajar de forma independiente y paralela a cualquier desarrollo del back-end completo en AEM, que tiene sus propias canalizaciones de implementación.

>[!IMPORTANT]
>
>En términos generales, la canalización front-end se utiliza normalmente con la variable [Creación rápida AEM sitio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), hay un tutorial relacionado [Introducción a AEM Sites: Creación rápida de sitios](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) para obtener más información al respecto. Así que en este tutorial y los vídeos asociados encontrará referencias a él, esto es para asegurarse de que se señalan las diferencias sutiles y hay alguna comparación directa o indirecta para explicar conceptos cruciales.


A relacionado [tutorial de varios pasos](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) recorre la implementación de un sitio AEM para una marca ficticia de estilo de vida en WKND utilizando la función de creación rápida del sitio. Revisión de la variable [Flujo de trabajo temático](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) también es útil comprender el funcionamiento de la canalización front-end.

## Información general, ventajas y consideraciones para la canalización front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>Esto solo se aplica a AEM as a Cloud Service y no a las implementaciones de Adobe Cloud Manager basadas en AMS.

## Requisitos previos

El paso de implementación de este tutorial se realiza en un administrador de Adobe Cloud Manager, asegúrese de que dispone de un __Administrador de implementación__ función, consulte Cloud Manage [Definiciones de funciones](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Asegúrese de utilizar la variable [Programa Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) y [Entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) al completar este tutorial.

## Pasos siguientes {#next-steps}

Un tutorial paso a paso recorre el [AEM proyecto de WKND Sites](https://github.com/adobe/aem-guides-wknd) conversión para habilitarla para la canalización front-end.

¿Qué estás esperando? Inicie el tutorial navegando hasta el [Revisar proyecto de pila completa](review-uifrontend-module.md) capítulo y resumen el ciclo de vida de desarrollo del front-end en el contexto del proyecto estándar de AEM Sites.


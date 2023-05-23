---
title: AEM Habilitación de la canalización front-end para el tipo de archivo del proyecto de estándar
description: AEM Obtenga información sobre cómo habilitar una canalización front-end para un proyecto de estándar para una implementación más rápida de recursos estáticos como CSS, JavaScript, fuentes e iconos. AEM También separa el desarrollo front-end del desarrollo back-end completo en el desarrollo de la parte posterior de la pila de la parte superior de la.
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
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---

# AEM Habilitación de la canalización front-end para el tipo de archivo del proyecto de estándar{#enable-front-end-pipeline-standard-aem-project}

Obtenga información sobre cómo habilitar el [AEM Proyecto de sitios de WKND](https://github.com/adobe/aem-guides-wknd) AEM (también conocido como Proyecto de estándar) creado con [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) para implementar recursos front-end como CSS, JavaScript, fuentes e iconos mediante una canalización front-end para un ciclo más rápido de desarrollo a implementación. AEM La separación del desarrollo de front-end del desarrollo de back-end de pila completa en la creación de segmentos de la base de datos de la base de datos de la base de datos de. También aprenderá cómo son estos recursos front-end __no__ AEM se suministra desde el repositorio de, pero desde la red de distribución de contenido (CDN), un cambio en el paradigma de entrega.


Se crea una nueva canalización front-end en Adobe Cloud Manager que solo genera e implementa `ui.frontend` AEM envía artefactos a la CDN integrada e informa a los usuarios sobre su ubicación. AEM En la generación de HTML de la página web, durante la fase de creación de la página web, la variable `<link>` y `<script>` , consulte la ubicación de este artefacto en el `href` valor de atributo.

AEM AEM Sin embargo, después de la conversión del proyecto de WKND Sites, los desarrolladores de front-end pueden trabajar de forma independiente y paralela a cualquier desarrollo de back-end completo en el, que tiene sus propias canalizaciones de implementación.

>[!IMPORTANT]
>
>En términos generales, la canalización front-end se utiliza generalmente con la variable [AEM Creación rápida de sitios de](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), hay un tutorial relacionado [Introducción a AEM Sites: Creación rápida de sitios](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) para obtener más información. En este tutorial y en los vídeos asociados nos encontramos con referencias a él, para asegurarnos de que se destacan las diferencias sutiles y de que haya alguna comparación directa o indirecta para explicar conceptos cruciales.


A relacionado [tutorial de varios pasos](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) AEM recorre la implementación de un sitio web de para una marca ficticia de estilo de vida: WKND mediante la función Creación rápida de sitios. Revisión de la [Flujo de trabajo de temas](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) para comprender el funcionamiento de la canalización front-end también resulta útil.

## Información general, ventajas y consideraciones para la canalización front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>AEM Esto solo se aplica a las implementaciones de Cloud Manager de Adobe basadas en AMS y no a las as a Cloud Service de la.

## Requisitos previos

El paso de implementación de este tutorial tiene lugar en un Adobe de Cloud Manager. Asegúrese de que dispone de un __Administrador de implementación__ función, consulte Cloud Manager [Definiciones de funciones](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Asegúrese de utilizar el [Programa de zona protegida](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) y [Entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) al completar este tutorial.

## Pasos siguientes {#next-steps}

Un tutorial paso a paso recorre el [AEM Proyecto de sitios de WKND](https://github.com/adobe/aem-guides-wknd) conversión para habilitarla para la canalización front-end.

¿Qué estás esperando? Inicie el tutorial navegando hasta [Revisar proyecto de pila completa](review-uifrontend-module.md) y recapitulen el ciclo de vida del desarrollo front-end en el contexto del proyecto estándar de AEM Sites.

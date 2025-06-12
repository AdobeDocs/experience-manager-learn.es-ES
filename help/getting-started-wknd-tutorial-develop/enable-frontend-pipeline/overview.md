---
title: Habilitación de la canalización front-end para el tipo de archivo del proyecto de AEM estándar
description: Obtenga información sobre cómo habilitar una canalización front-end para proyectos de AEM estándar para una implementación más rápida de recursos estáticos como CSS, JavaScript, Fonts, Icons. También la separación del desarrollo front-end del desarrollo back-end completo en AEM.
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
duration: 206
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 1%

---

# Habilitación de la canalización front-end para el tipo de archivo del proyecto de AEM estándar{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

Aprenda a habilitar el [Proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd) (también conocido como Proyecto AEM estándar) creado con el [Arquetipo de proyecto de AEM](https://github.com/adobe/aem-project-archetype) para implementar recursos front-end como CSS, JavaScript, fuentes e iconos mediante una canalización front-end para un ciclo más rápido de desarrollo a implementación. La separación del desarrollo front-end del desarrollo back-end completo en AEM. También aprenderá cómo estos recursos front-end __no__ se proporcionan desde el repositorio de AEM, sino desde la red de distribución de contenido (CDN), un cambio en el paradigma de entrega.


Se crea una nueva canalización front-end en Adobe Cloud Manager que solo genera e implementa `ui.frontend` artefactos en la CDN integrada e informa a AEM sobre su ubicación. En AEM durante la generación de HTML de la página web, las etiquetas `<link>` y `<script>` hacen referencia a esta ubicación de artefacto en el valor de atributo `href`.

Sin embargo, después de la conversión del proyecto AEM de WKND Sites, los desarrolladores de front-end pueden trabajar de forma independiente y paralela a cualquier desarrollo back-end completo en AEM, que tiene sus propias canalizaciones de implementación.

>[!IMPORTANT]
>
>En términos generales, la canalización front-end generalmente se utiliza con [Creación rápida de sitios de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=es), hay un tutorial relacionado [Introducción a AEM Sites - Creación rápida de sitios](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=es) para obtener más información al respecto. En este tutorial y en los vídeos asociados nos encontramos con referencias a él, para asegurarnos de que se destacan las diferencias sutiles y de que haya alguna comparación directa o indirecta para explicar conceptos cruciales.


Un [tutorial de varios pasos](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=es) relacionado explica cómo implementar un sitio de AEM para una marca ficticia de estilo de vida WKND mediante la característica Creación rápida de sitios. También resulta útil revisar el [flujo de trabajo de la temática](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=es) para comprender el funcionamiento de la canalización front-end.

## Información general, ventajas y consideraciones para la canalización front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Esto solo se aplica a AEM as a Cloud Service y no a las implementaciones de Adobe Cloud Manager basadas en AMS.

## Requisitos previos

El paso de implementación de este tutorial tiene lugar en un Cloud Manager de Adobe. Asegúrese de que tiene la función __Administrador de implementación__; consulte Cloud Manager [Definiciones de funciones](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=es#role-definitions).

Asegúrese de usar el [programa de espacio aislado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=es) y el [entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=es) al completar este tutorial.

## Siguientes pasos {#next-steps}

Un tutorial paso a paso muestra la conversión del [proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd) para habilitarlo para la canalización front-end.

¿Qué estás esperando? Inicie el tutorial navegando hasta el capítulo [Revisar proyecto de pila completa](review-uifrontend-module.md) y vuelva a recapitular el ciclo de vida de desarrollo front-end en el contexto del proyecto estándar de AEM Sites.

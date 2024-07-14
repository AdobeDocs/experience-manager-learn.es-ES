---
title: AEM Habilitación de la canalización front-end para el tipo de archivo del proyecto de estándar
description: AEM Obtenga información sobre cómo habilitar una canalización front-end para un proyecto de estándar para una implementación más rápida de recursos estáticos como CSS, JavaScript, fuentes e iconos. AEM También separa el desarrollo front-end del desarrollo back-end completo en el desarrollo de la parte posterior de la pila de la parte superior de la.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 1%

---

# AEM Habilitación de la canalización front-end para el tipo de archivo del proyecto de estándar{#enable-front-end-pipeline-standard-aem-project}

AEM AEM AEM Aprenda a habilitar el [Proyecto de WKND Sites](https://github.com/adobe/aem-guides-wknd) (también conocido como Proyecto de estándar) creado con [Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) para implementar recursos front-end como CSS, JavaScript, Fuentes e Iconos mediante una canalización front-end para un ciclo más rápido de desarrollo a implementación. AEM La separación del desarrollo de front-end del desarrollo de back-end de pila completa en la creación de segmentos de la base de datos de la base de datos de la base de datos de. AEM También aprenderá cómo estos recursos front-end __no__ se proporcionan desde el repositorio de, sino desde la red de distribución de contenido (CDN), un cambio en el paradigma de entrega.


Se crea una nueva canalización front-end en Adobe Cloud Manager AEM que solo genera e implementa `ui.frontend` artefactos en la CDN integrada y que informa a los usuarios de la ubicación de la red de distribución de contenido (CDN) sobre su ubicación. AEM En las etiquetas `<link>` y `<script>` durante la generación del HTML de la página web, consulte esta ubicación del artefacto en el valor de atributo `href`.

AEM AEM Sin embargo, después de la conversión del proyecto de WKND Sites, los desarrolladores de front-end pueden trabajar de forma independiente y paralela a cualquier desarrollo de back-end completo en el, que tiene sus propias canalizaciones de implementación.

>[!IMPORTANT]
>
>AEM En términos generales, la canalización front-end se utiliza generalmente con [Creación rápida de sitios](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), hay un tutorial relacionado [Introducción a AEM Sites: creación rápida de sitios](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) para obtener más información al respecto. En este tutorial y en los vídeos asociados nos encontramos con referencias a él, para asegurarnos de que se destacan las diferencias sutiles y de que haya alguna comparación directa o indirecta para explicar conceptos cruciales.


AEM Un [tutorial de varios pasos](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) relacionado recorre la implementación de un sitio de la aplicación para una marca ficticia de estilo de vida WKND mediante la característica Creación rápida de sitios. También resulta útil revisar el [flujo de trabajo de la temática](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) para comprender el funcionamiento de la canalización front-end.

## Información general, ventajas y consideraciones para la canalización front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Esto solo se aplica a AEM as a Cloud Service y no a las implementaciones de Cloud Manager de Adobe basadas en AMS.

## Requisitos previos

El paso de implementación de este tutorial tiene lugar en un Cloud Manager de Adobe. Asegúrese de que tiene la función __Administrador de implementación__; consulte Cloud Manager [Definiciones de funciones](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Asegúrese de usar el [programa de espacio aislado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) y el [entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) al completar este tutorial.

## Pasos siguientes {#next-steps}

AEM Un tutorial paso a paso recorre la conversión del [Proyecto de sitios WKND de WKND de WKND](https://github.com/adobe/aem-guides-wknd) para habilitarlo para la canalización front-end.

¿Qué estás esperando? Inicie el tutorial navegando hasta el capítulo [Revisar proyecto de pila completa](review-uifrontend-module.md) y vuelva a recapitular el ciclo de vida de desarrollo front-end en el contexto del proyecto estándar de AEM Sites.

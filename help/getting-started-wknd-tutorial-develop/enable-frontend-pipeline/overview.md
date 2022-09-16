---
title: Habilitar la canalización front-end para el tipo de archivo del proyecto de AEM estándar
description: Convierta un proyecto de AEM estándar para una implementación más rápida de recursos estáticos como CSS, JavaScript, fuentes e iconos mediante la canalización front-end. Y la separación del desarrollo front-end del desarrollo back-end completo en AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 2%

---


# Habilitar la canalización front-end para el tipo de archivo del proyecto de AEM estándar{#enable-front-end-pipeline-standard-aem-project}

Obtenga información sobre cómo habilitar el proyecto de AEM estándar creado mediante [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) para implementar recursos estáticos como CSS, JavaScript, fuentes e iconos mediante canalización front-end para acelerar el ciclo de desarrollo e implementación. Y la separación del desarrollo front-end del desarrollo back-end completo en AEM. También aprenderá cómo son estos recursos estáticos __not__ se sirve desde AEM repositorio pero desde la CDN, un cambio en el paradigma de envío.

Se crea una nueva canalización front-end en Adobe Cloud Manager que solo genera e implementa `ui.frontend` artefactos a CDN e informa a AEM sobre su ubicación. Durante la generación de HTML de la página web, la variable `<link>` y `<script>` las etiquetas hacen referencia a esta ubicación en la `href` valor de atributo.

Después de la conversión AEM proyecto estándar, los desarrolladores del front-end pueden trabajar de forma independiente y paralela a cualquier desarrollo back-end de toda la pila en AEM, que tiene sus propias canalizaciones de implementación.

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>Esto solo es aplicable a AEM as a Cloud Service y no a las implementaciones de Adobe Cloud Manager basadas en AMS.


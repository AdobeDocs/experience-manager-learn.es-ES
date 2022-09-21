---
title: Consideraciones sobre desarrollo
description: Considere el impacto en el proceso de desarrollo front-end y back-end una vez que habilite la canalización front-end.
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
source-wordcount: '201'
ht-degree: 0%

---


# Consideraciones sobre desarrollo

Después de habilitar la canalización front-end para implementar únicamente los recursos front-end en AEM entorno as a Cloud Service, hay algún impacto en el desarrollo de AEM local y debe modificar el modelo de ramificación git.

## Objetivo

* Cómo tener un flujo de desarrollo front-end y back-end sin fricciones
* Revise las dependencias entre la canalización completa y la del front-end


## Consideraciones sobre el desarrollo local

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## Enfoque de desarrollo ajustado

* Para el desarrollo local mediante AEM SDK, el equipo de desarrollo back-end sigue necesitando la generación clientlib mediante `ui.frontend` pero durante la implementación de Cloud Manager en AEM entorno as a Cloud Service, debe omitirlo. Esto plantea un desafío sobre cómo aislar los cambios de configuración del proyecto descritos en la sección [Actualizar proyecto](update-project.md) capítulo.

A __solución__ podría ser para ajustar el modelo de ramificación de git y asegurarse de que los cambios de configuración del proyecto de AEM nunca regresen al flujo de __desarrollo local__ bifurque el AEM que utilizan los desarrolladores back-end.


* Como parte de una mejora continua en el proyecto de AEM, si introduce nuevos componentes o actualiza un componente existente que tenga cambios en ambos `ui.app` y `ui.frontend` , debe ejecutar las canalizaciones de pila completa y front-end.




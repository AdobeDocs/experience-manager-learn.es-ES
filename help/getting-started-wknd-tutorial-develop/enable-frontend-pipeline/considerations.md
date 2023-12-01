---
title: Consideraciones de desarrollo
description: Tenga en cuenta el impacto en el proceso de desarrollo del front-end y del back-end una vez que habilite la canalización front-end.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Consideraciones de desarrollo

AEM Después de habilitar la canalización front-end para implementar únicamente los recursos front-end en el entorno as a Cloud Service AEM, se produce cierto impacto en el desarrollo de la local y es necesario modificar el modelo de ramificación de Git.

## Objetivo

* Cómo tener un flujo de desarrollo front-end y back-end sin fricción
* Revise las dependencias entre la canalización de pila completa y la canalización front-end


## Consideraciones sobre el desarrollo local

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Enfoque de desarrollo ajustado

* AEM Para el desarrollo local mediante el SDK de la, el equipo de desarrollo del back-end sigue necesitando la generación de clientlib a través de `ui.frontend` AEM , pero durante la implementación de Cloud Manager en el entorno as a Cloud Service, tiene que omitirlo. Esto presenta un desafío sobre cómo aislar los cambios de configuración del proyecto descritos en la [Actualizar proyecto](update-project.md) capítulo.

A __solución__ AEM podría ser para ajustar el modelo de ramificación de Git y asegurarse de que los cambios de configuración del proyecto de nunca vuelvan a fluir a __desarrollo local__ AEM rama que utilizan los desarrolladores de back-end de la.


* AEM Como parte de una mejora continua de su proyecto de, si introduce nuevos componentes o actualiza un componente existente que tenga cambios en ambos `ui.app` y `ui.frontend` , tiene que ejecutar tanto las canalizaciones full-stack como las front-end.

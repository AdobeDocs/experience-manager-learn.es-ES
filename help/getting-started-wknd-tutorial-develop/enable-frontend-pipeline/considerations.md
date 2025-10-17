---
title: Consideraciones sobre el desarrollo
description: Tenga en cuenta el impacto en el proceso de desarrollo del front-end y del back-end una vez que habilite la canalización front-end.
version: Experience Manager as a Cloud Service
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
duration: 79
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '201'
ht-degree: 100%

---

# Consideraciones sobre el desarrollo

Después de habilitar la canalización front-end para implementar únicamente los recursos front-end en el entorno de AEM as a Cloud Service, se produce cierto impacto en el desarrollo local de AEM y tiene que modificar el modelo de ramificación de Git.

## Objetivo

* Cómo tener un flujo de desarrollo front-end y back-end sin problemas
* Revisar las dependencias entre la canalización de pila completa y la canalización front-end


## Consideraciones sobre el entorno de desarrollo local

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Enfoque de desarrollo ajustado

* Para el desarrollo local mediante el SDK de AEM, el equipo de desarrollo del back-end sigue necesitando la generación de clientlib mediante el módulo `ui.frontend`, pero durante la implementación de Cloud Manager en el entorno de AEM as a Cloud Service debe omitirla. Esto presenta un desafío sobre cómo aislar los cambios de configuración del proyecto descritos en el capítulo [Actualizar proyecto](update-project.md).

Una __solución__ podría ser ajustar su modelo de ramificación de Git y asegurarse de que los cambios de configuración del proyecto de AEM nunca regresen a la rama de __desarrollo local__ que usan los desarrolladores del back-end de AEM.


* Como parte de la mejora continua de su proyecto de AEM, si introduce nuevos componentes o actualiza un componente existente que tiene cambios en los módulos `ui.app` y `ui.frontend`, debe ejecutar canalizaciones full-stack y front-end.

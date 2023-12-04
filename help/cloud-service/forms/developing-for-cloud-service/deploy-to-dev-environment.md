---
title: Implementar en el entorno de desarrollo
description: Implemente el código desde la rama del repositorio de Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
jira: KT-8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
duration: 38
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# Implementar en el entorno de desarrollo

En el paso anterior insertamos nuestra rama maestra desde nuestro repositorio local de Git en la rama MyFirstAF del repositorio de Cloud Manager.

El siguiente paso es implementar el código en el entorno de desarrollo.
Inicie sesión en Cloud Manager y seleccione su programa

Seleccione Implementar en desarrollo como se muestra a continuación


![primer paso](assets/deploy-first-step1.png)


Seleccione Canalización de implementación como se muestra
![primer paso](assets/deploy1.png)

Seleccione el código fuente y la rama Git adecuada
![primer paso](assets/deploy2.png)
Asegúrese de actualizar los cambios

Ejecutar la canalización
![run-pipeline](assets/run-pipeline.png)

Una vez implementado el código, debería ver los cambios en la instancia de Cloud Service de AEM Forms.

## Pasos siguientes

[Actualizando proyecto de arquetipo Maven](./updating-project-archetype.md)

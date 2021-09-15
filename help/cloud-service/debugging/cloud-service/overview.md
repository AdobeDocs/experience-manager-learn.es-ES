---
title: Depuración AEM como Cloud Service
description: en infraestructura de nube con capacidad de autoservicio, escalable y que hace que sea necesario que los desarrolladores de AEM entiendan cómo comprender y depurar varias facetas de AEM como Cloud Service, desde la generación y la implementación hasta la obtención de detalles de la ejecución de aplicaciones de AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---

# Depuración AEM como Cloud Service

AEM como Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM. AEM as a Cloud Service se ejecuta en una infraestructura de nube con capacidad de autoservicio, escalable y de AEM, que requiere que los desarrolladores comprendan cómo comprender y depurar varias facetas de AEM como Cloud Service, desde la compilación y la implementación hasta la obtención de detalles de la ejecución de AEM aplicaciones.

## Registros

Los registros proporcionan detalles sobre cómo funciona su aplicación en AEM como Cloud Service, así como información sobre los problemas con las implementaciones.

[Depuración AEM como Cloud Service mediante registros](./logs.md)

## Compilación e implementación

Las canalizaciones de Adobe Cloud Manager implementan AEM aplicación mediante una serie de pasos para determinar la calidad y viabilidad del código cuando se implementan en AEM como Cloud Service. Cada uno de los pasos puede provocar un error, por lo que es importante comprender cómo depurar las compilaciones para determinar la causa raíz de y cómo resolver cualquier error.

[Depuración AEM como compilación e implementación de Cloud Service](./build-and-deployment.md)

## Developer Console

La consola para desarrolladores proporciona una variedad de información e introspecciones en AEM como entornos de Cloud Service que son útiles para comprender cómo su aplicación es reconocida por y funciona dentro de AEM como Cloud Service.

[Depuración AEM como Cloud Service con Developer Console](./developer-console.md)

## CRXDE Lite

CRXDE Lite es una herramienta clásica pero potente para depurar AEM como entornos de desarrollo de Cloud Service. CRXDE Lite proporciona un conjunto de funciones que ayuda a depurar desde la inspección de todos los recursos y propiedades, la manipulación de las partes mutables del JCR, la investigación de permisos y la evaluación de consultas.

[Depuración AEM como Cloud Service con CRXDE Lite](./crxde-lite.md)

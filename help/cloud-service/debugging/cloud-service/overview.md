---
title: Depuración de AEM as a Cloud Service
description: en infraestructura de nube con capacidad de autoservicio, escalable y que hace que sea necesario que los desarrolladores de AEM comprendan cómo comprender y depurar varias facetas de AEM as a Cloud Service, desde la creación y la implementación hasta la obtención de detalles sobre cómo ejecutar aplicaciones AEM.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 2%

---


# Depuración de AEM as a Cloud Service

AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones AEM. AEM as a Cloud Service se ejecuta en una infraestructura en la nube con capacidad de autoservicio, escalable y de autoservicio, lo que requiere que los desarrolladores de AEM comprendan cómo comprender y depurar varias facetas de AEM as a Cloud Service, desde la creación y la implementación hasta la obtención de detalles sobre cómo ejecutar aplicaciones AEM.

## Registros

Los registros proporcionan detalles sobre cómo funciona su aplicación en AEM as a Cloud Service, así como información sobre los problemas con las implementaciones.

[Depuración de AEM as a Cloud Service mediante registros](./logs.md)

## Compilación e implementación

Las canalizaciones de Adobe Cloud Manager implementan la aplicación de AEM mediante una serie de pasos para determinar la calidad y viabilidad del código cuando se implementa en AEM as a Cloud Service. Cada uno de los pasos puede provocar un error, por lo que es importante comprender cómo depurar las compilaciones para determinar la causa raíz de y cómo resolver cualquier error.

[Depuración de AEM as a Cloud Service: compilación e implementación](./build-and-deployment.md)

## Developer Console

La consola de desarrollador proporciona una variedad de información e introspecciones en los entornos de AEM as a Cloud Service que son útiles para comprender cómo AEM as a Cloud Service reconoce y funciona su aplicación.

[Depuración de AEM as a Cloud Service con Developer Console](./developer-console.md)

## CRXDE Lite

CRXDE Lite es una herramienta clásica pero potente para depurar entornos de desarrollo de AEM as a Cloud Service. CRXDE Lite proporciona un conjunto de funcionalidades que ayudan a depurar desde la inspección de todos los recursos y propiedades, la manipulación de las partes mutables del JCR, la investigación de permisos y la evaluación de consultas.

[Depuración de AEM as a Cloud Service con CRXDE Lite](./crxde-lite.md)

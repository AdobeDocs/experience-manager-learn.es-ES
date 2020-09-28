---
title: Depuración AEM como Cloud Service
description: en la infraestructura de nube de autoservicio, escalable, que requiere que los desarrolladores de AEM entiendan cómo comprender y depurar varias facetas de AEM como Cloud Service, desde la compilación y la implementación hasta obtener detalles de la ejecución de aplicaciones AEM.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
translation-type: tm+mt
source-git-commit: debb65a218ae0b9e5105c3f63018358902330b34
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---


# Depuración AEM como Cloud Service

AEM como Cloud Service es la manera nativa de la nube de aprovechar las aplicaciones AEM. AEM como Cloud Service se ejecuta en una infraestructura de nube autoservicio, escalable, que requiere que los desarrolladores AEM entiendan cómo entender y depurar varias facetas de AEM como Cloud Service, desde la generación y la implementación hasta obtener detalles de cómo ejecutar aplicaciones AEM.

## Registros

Los registros proporcionan detalles sobre el funcionamiento de la aplicación en AEM como Cloud Service, así como información sobre los problemas con las implementaciones.

[Depuración de AEM como Cloud Service mediante registros](./logs.md)

## Creación e implementación

Las canalizaciones de Adobe Cloud Manager implementan AEM aplicación mediante una serie de pasos para determinar la calidad y viabilidad del código cuando se implementa en AEM como Cloud Service. Cada uno de los pasos puede dar como resultado un error, lo que hace importante comprender cómo depurar las compilaciones para determinar la causa raíz de y cómo resolver los errores.

[Depuración AEM como una compilación e implementación Cloud Service](./build-and-deployment.md)

## Developer Console

La consola para desarrolladores proporciona una variedad de información e introspecciones en AEM como entornos de Cloud Service que son útiles para comprender cómo su aplicación es reconocida por y funciona dentro de AEM como Cloud Service.

[Depuración de AEM como Cloud Service con la Consola de programadores](./developer-console.md)

## CRXDE Lite

CRXDE Lite es una herramienta clásica pero poderosa para depurar AEM como entornos de desarrollo Cloud Service. CRXDE Lite proporciona un conjunto de funciones que ayudan a depurar desde la inspección de todos los recursos y propiedades, la manipulación de las partes mutables del JCR, la investigación de permisos y la evaluación de consultas.

[Depuración de AEM como Cloud Service con CRXDE Lite](./crxde-lite.md)

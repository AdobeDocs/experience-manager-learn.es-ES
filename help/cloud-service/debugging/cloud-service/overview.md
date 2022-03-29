---
title: Depuración AEM as a Cloud Service
description: en infraestructura de nube con capacidad de autoservicio, escalable y que hace que sea necesario que los desarrolladores de AEM comprendan cómo comprender y depurar varias facetas de AEM as a Cloud Service, desde la compilación y la implementación hasta la obtención de detalles de la ejecución de aplicaciones de AEM.
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
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Depuración AEM as a Cloud Service

AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM. AEM se ejecuta en una infraestructura en la nube con capacidad de autoservicio, escalable y AEM, que requiere que los desarrolladores comprendan cómo comprender y depurar varias facetas de AEM as a Cloud Service, desde la compilación y la implementación hasta la obtención de detalles de la ejecución de AEM aplicaciones.

## Registros

Los registros proporcionan detalles sobre cómo funciona su aplicación en AEM as a Cloud Service, así como información sobre los problemas con las implementaciones.

[Depuración AEM as a Cloud Service mediante registros](./logs.md)

## Compilación e implementación

Las canalizaciones de Adobe Cloud Manager implementan AEM aplicación mediante una serie de pasos para determinar la calidad y viabilidad del código cuando se implementan en AEM as a Cloud Service. Cada uno de los pasos puede provocar un error, por lo que es importante comprender cómo depurar las compilaciones para determinar la causa raíz de y cómo resolver cualquier error.

[Depuración AEM compilación e implementación as a Cloud Service](./build-and-deployment.md)

## Developer Console

La consola para desarrolladores proporciona una variedad de información e introspecciones en AEM entornos as a Cloud Service que son útiles para comprender cómo su aplicación es reconocida por y funciona dentro de AEM as a Cloud Service.

[Depuración AEM as a Cloud Service con Developer Console](./developer-console.md)

## Explorador del repositorio

El Explorador de repositorios es una potente herramienta que proporciona visibilidad en AEM almacén de datos subyacente, lo que permite depurar fácilmente AEM entorno as a Cloud Service. El explorador de repositorios admite una vista de solo lectura de los recursos y las propiedades de AEM en producción, fase y desarrollo, así como los servicios de autor, publicación y vista previa.

[Depuración AEM as a Cloud Service con el explorador del repositorio](./repository-browser.md)

---
title: Depuración de AEM as a Cloud Service
description: en una infraestructura en la nube escalable y de autoservicio, que requiere que los desarrolladores de AEM entiendan y depuren varias facetas de AEM as a Cloud Service, desde la compilación y la implementación hasta la obtención de detalles sobre la ejecución de aplicaciones de AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '314'
ht-degree: 100%

---

# Depuración de AEM as a Cloud Service

AEM as a Cloud Service es la forma nativa de la nube de aprovechar las aplicaciones de AEM. AEM as a Cloud Service se ejecuta en una infraestructura en la nube escalable de autoservicio, lo que requiere que los desarrolladores de AEM comprendan cómo entender y depurar varias facetas de AEM as a Cloud Service, desde la compilación y la implementación hasta la obtención de detalles sobre la ejecución de aplicaciones de AEM.

## Registros

Los registros proporcionan detalles sobre el funcionamiento de la aplicación en AEM as a Cloud Service, así como información sobre los problemas con las implementaciones.

[Depuración de AEM as a Cloud Service mediante registros](./logs.md)

## Creación e implementación

Las canalizaciones de Adobe Cloud Manager implementan la aplicación de AEM a través de una serie de pasos para determinar la calidad y viabilidad del código cuando se implementa en AEM as a Cloud Service. Cada uno de los pasos puede provocar un error, por lo que es importante comprender cómo depurar las versiones para determinar la causa raíz de los errores y cómo resolverlos.

[Depuración de la versión y la implementación de AEM as a Cloud Service](./build-and-deployment.md)

## Developer Console

Developer Console proporciona una variedad de información e introspecciones en entornos de AEM as a Cloud Service que son útiles para comprender cómo AEM as a Cloud Service reconoce su aplicación y cómo funciona esta dentro del ecosistema.

[Depuración de AEM as a Cloud Service con Developer Console](./developer-console.md)

## Explorador del repositorio

Repository Browser es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente de AEM, lo que permite depurar fácilmente el entorno de AEM as a Cloud Service. Repository Browser admite una vista de solo lectura de los recursos y las propiedades de AEM en Producción, ensayo y desarrollo, así como los servicios de creación, publicación y vista previa.

[Depuración de AEM as a Cloud Service con Repository Browser](./repository-browser.md)

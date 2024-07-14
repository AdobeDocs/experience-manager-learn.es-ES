---
title: Depuración de AEM as a Cloud Service
description: AEM en una infraestructura en la nube escalable y de autoservicio, lo que requiere que los desarrolladores de comprendan cómo comprender y depurar varias facetas de AEM as a Cloud Service AEM, desde la compilación y la implementación hasta la obtención de detalles sobre la ejecución de aplicaciones de la.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# Depuración de AEM as a Cloud Service

AEM as a Cloud Service AEM es la forma nativa de la nube de aprovechar las aplicaciones de la. AEM as a Cloud Service AEM se ejecuta en una infraestructura en la nube escalable y de autoservicio, lo que requiere que los desarrolladores de comprendan cómo comprender y depurar varias facetas de AEM as a Cloud Service AEM, desde la compilación y la implementación hasta la obtención de detalles sobre la ejecución de aplicaciones de la nube.

## Registros

Los registros proporcionan detalles sobre el funcionamiento de la aplicación en AEM as a Cloud Service, así como información sobre los problemas con las implementaciones.

[Depuración de AEM as a Cloud Service mediante registros](./logs.md)

## Compilación e implementación

Adobe Las canalizaciones de Cloud Manager AEM implementan la aplicación de la aplicación a través de una serie de pasos para determinar la calidad y viabilidad del código cuando se implementa en AEM as a Cloud Service. Cada uno de los pasos puede provocar un error, por lo que es importante comprender cómo depurar las compilaciones para determinar la causa raíz de los errores y cómo resolverlos.

[Depuración de la compilación y la implementación de AEM as a Cloud Service](./build-and-deployment.md)

## Consola de desarrollador

Developer Console proporciona una variedad de información e introspecciones en entornos de AEM as a Cloud Service que son útiles para comprender cómo AEM as a Cloud Service reconoce y funciona su aplicación.

[Depuración de AEM as a Cloud Service con Developer Console](./developer-console.md)

## Explorador del repositorio

AEM El Explorador de repositorios es una potente herramienta que proporciona visibilidad sobre el almacén de datos subyacente de los que se dispone en el repositorio de datos, lo que permite depurar fácilmente el entorno de AEM as a Cloud Service. AEM El Explorador de repositorios admite una vista de sólo lectura de los recursos y las propiedades de los recursos en producción, fase y desarrollo, así como de los servicios de autor, Publish y vista previa.

[Depuración de AEM as a Cloud Service con el Explorador de repositorios](./repository-browser.md)

---
title: Capacidad de extensión de los microservicios de Asset Compute para AEM as a Cloud Service
description: Este tutorial recorta la creación de un trabajador simple de Asset Compute que crea una representación de recursos recortando el recurso original en un círculo y aplica el contraste y el brillo configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un trabajador personalizado de Asset Compute para utilizarlo con AEM as a Cloud Service.
feature: Microservicios de Asset Compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integraciones, desarrollo
role: Desarrollador
level: Intermedio, con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 0%

---


# Capacidad de extensión de los microservicios de Asset Compute

Los microservicios Asset Compute de AEM as Cloud Service admiten el desarrollo y la implementación de trabajadores personalizados que se utilizan para leer y manipular datos binarios de recursos almacenados en AEM, normalmente para crear representaciones de recursos personalizadas.

Mientras que en AEM 6.x los procesos personalizados de flujo de trabajo de AEM se utilizaban para leer, transformar y escribir representaciones de recursos, en AEM as a Cloud Service los trabajadores de Asset Compute satisfacen esta necesidad.

## Qué hará

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial recorta la creación de un trabajador simple de Asset Compute que crea una representación de recursos recortando el recurso original en un círculo y aplica el contraste y el brillo configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un trabajador personalizado de Asset Compute para utilizarlo con AEM as a Cloud Service.

### Objetivos {#objective}

1. Aprovisionar y configurar las cuentas y los servicios necesarios para crear e implementar un trabajador de Asset Compute
1. Creación y configuración de un proyecto de Asset Compute
1. Desarrollar un trabajador de Asset Compute que genere una representación personalizada
1. Escriba pruebas para y aprenda a depurar el trabajador personalizado de Asset Compute
1. Implemente el programa de trabajo de Asset Compute e integre el servicio de creación de AEM as a Cloud Service a través de Perfiles de procesamiento

## Configuración

Obtenga información sobre cómo prepararse correctamente para ampliar los trabajadores de Asset Compute y comprender qué servicios y cuentas deben aprovisionarse y configurarse, y qué software debe instalarse localmente para el desarrollo.

### Aprovisionamiento de cuentas y servicios{#accounts-and-services}

Las siguientes cuentas y servicios requieren aprovisionamiento y acceso a para completar el tutorial, el entorno de desarrollo de AEM as a Cloud Service o el programa de espacio aislado, el acceso a Adobe Project Firefly y el almacenamiento de blob de Microsoft Azure.

+ [Suministro de cuentas y servicios](./set-up/accounts-and-services.md)

### Entorno de desarrollo local

El desarrollo local de los proyectos de Asset Compute requiere un conjunto de herramientas de desarrollador específico, diferente del desarrollo tradicional de AEM, que incluye: Microsoft Visual Studio Code, Docker Desktop, Node.js y módulos npm compatibles.

+ [Configuración del entorno de desarrollo local](./set-up/development-environment.md)

### Adobe Project Firefly

Los proyectos de Asset Compute son proyectos de Adobe Project Firefly especialmente definidos y, como tales, requieren acceso a Adobe Project Firefly en Adobe Developer Console para configurarlos e implementarlos.

+ [Configuración del proyecto de Adobe Firefly](./set-up/firefly.md)

## Desarrollar

Obtenga información sobre cómo crear y configurar un proyecto de Asset Compute y, a continuación, desarrollar un trabajador personalizado que genere una representación de recursos personalizada.

### Crear un nuevo proyecto de Asset Compute

Los proyectos de Asset Compute, que contienen uno o más trabajadores de Asset Compute, se generan mediante la CLI de Adobe I/O interactiva. Los proyectos de Asset Compute son proyectos de Adobe Project Firefly especialmente estructurados, que a su vez son proyectos de Node.js.

+ [Crear un nuevo proyecto de Asset Compute](./develop/project.md)

### Configuración de variables de entorno

Las variables de entorno se mantienen en el archivo `.env` para el desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento en la nube necesarias para el desarrollo local.

+ [Configuración de las variables de entorno](./develop/environment-variables.md)

### Configurar manifest.yml

Los proyectos de Asset Compute contienen manifiestos que definen todos los trabajadores de Asset Compute contenidos en el proyecto, así como los recursos disponibles cuando se implementan en Adobe I/O Runtime para su ejecución.

+ [Configurar manifest.yml](./develop/manifest.md)

### Desarrollo de un trabajador

El desarrollo de un trabajador de Asset Compute es el núcleo de la ampliación de los microservicios de Asset Compute, ya que el trabajador contiene el código personalizado que genera u organiza la generación de la representación de recursos resultante.

+ [Desarrollo de un trabajador de Asset Compute](./develop/worker.md)

### Uso de la herramienta de desarrollo de Asset Compute

La herramienta de desarrollo de Asset Compute proporciona un mazo de cables web local para implementar, ejecutar y previsualizar las representaciones generadas por el trabajador, lo que permite un desarrollo rápido e iterativo de los trabajadores de Asset Compute.

+ [Uso de la herramienta de desarrollo de Asset Compute](./develop/development-tool.md)

## Prueba y depuración

Obtenga información sobre cómo probar a los trabajadores personalizados de Asset Compute para que estén seguros de su funcionamiento y depurar a los trabajadores de Asset Compute para que entiendan y solucionen problemas cómo se ejecuta el código personalizado.

### Probar un trabajador

Asset Compute proporciona un marco de pruebas para crear grupos de pruebas para los trabajadores, lo que facilita la definición de pruebas que garanticen un comportamiento adecuado.

+ [Probar un trabajador](./test-debug/test.md)

### Depurar un trabajador

Los trabajadores de Asset Compute proporcionan varios niveles de depuración desde la salida tradicional `console.log(..)` hasta integraciones con __Código VS__ y __wskdebug__, lo que permite a los desarrolladores pasar por el código de trabajo a medida que se ejecuta en tiempo real.

+ [Depurar un trabajador](./test-debug/debug.md)

## Implementar

Obtenga información sobre cómo integrar a los trabajadores personalizados de Asset Compute con AEM as a Cloud Service, implementándolos primero en Adobe I/O Runtime y luego invocando desde AEM as a Cloud Service Author a través de los Perfiles de procesamiento de AEM Assets.

### Implementar en Adobe I/O Runtime

Los trabajadores de Asset Compute deben implementarse en Adobe I/O Runtime para poder utilizarlo con AEM as a Cloud Service.

+ [Uso de perfiles de procesamiento](./deploy/runtime.md)

### Integración de trabajadores mediante Perfiles de procesamiento AEM

Una vez implementados en Adobe I/O Runtime, los trabajadores de Asset Compute se pueden registrar en AEM as a Cloud Service a través de [Perfiles de procesamiento de recursos](../../assets/configuring/processing-profiles.md). Los perfiles de procesamiento se aplican, a su vez, a las carpetas de recursos que se aplican a los recursos que contienen.

+ [Integración con Perfiles de procesamiento AEM](./deploy/processing-profiles.md)

## Avanzado 

Estos tutoriales abreviados abordan casos de uso más avanzados basados en las enseñanzas básicas establecidas en capítulos anteriores.

+ [Desarrolle un ](./advanced/metadata.md) trabajo de metadatos de Asset Compute que pueda volver a escribir metadatos en el

## Código base en Github

El código base del tutorial está disponible en Github en:

+ [rama maestra adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

El código fuente no contiene los archivos `.env` o `config.json` necesarios. Se deben agregar y configurar usando la información de [cuentas y servicios](#accounts-and-services).

## Recursos adicionales

A continuación se indican varios recursos de Adobe que proporcionan más información y API y SDK útiles para desarrollar los trabajadores de Asset Compute.

### Documentación

+ [Documentación de Asset Compute Service](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Léame de la herramienta de desarrollo de Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [Trabajadores de ejemplo de Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### API y SDK

+ [SDK de Asset Compute](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca de ajustes de Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de reintentos de recuperación de nodos de Adobe](https://github.com/adobe/node-fetch-retry)
+ [Trabajadores de ejemplo de Asset Compute](https://github.com/adobe/asset-compute-example-workers)

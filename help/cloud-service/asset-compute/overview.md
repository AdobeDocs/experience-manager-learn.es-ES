---
title: Extensibilidad de los microservicios de asset compute para AEM como Cloud Service
description: Este tutorial recorta la creación de un programa de trabajo de Asset compute sencillo que crea una representación de recursos recortando el recurso original en un círculo y aplica el contraste y el brillo configurables. Aunque el propio programa de trabajo es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un programa de trabajo de Asset compute personalizado para utilizarlo con AEM como Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 0%

---


# Extensibilidad de los microservicios de asset compute

AEM como microservicios de Asset compute de Cloud Service admiten el desarrollo e implementación de trabajadores personalizados que se utilizan para leer y manipular datos binarios de recursos almacenados en AEM, más comúnmente, para crear representaciones de recursos personalizadas.

Mientras que en AEM 6.x los procesos de flujo de trabajo AEM personalizados se utilizaban para leer, transformar y escribir en versiones anteriores de recursos, en AEM como trabajadores de Asset compute Cloud Service satisfacen esta necesidad.

## Qué hará

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial recorta la creación de un programa de trabajo de Asset compute sencillo que crea una representación de recursos recortando el recurso original en un círculo y aplica el contraste y el brillo configurables. Aunque el propio programa de trabajo es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un programa de trabajo de Asset compute personalizado para utilizarlo con AEM como Cloud Service.

### Objetivos {#objective}

1. Aprovisionar y configurar las cuentas y los servicios necesarios para crear e implementar un trabajador de Asset compute
1. Creación y configuración de un proyecto de Asset compute
1. Desarrollar un programa de trabajo de Asset compute que genere una representación personalizada
1. Escriba pruebas para y aprenda a depurar el trabajador de Asset compute personalizado
1. Implementar el programa de trabajo de Asset compute e integrarlo AEM como un servicio Cloud Service Author mediante Perfiles de procesamiento

## Configurar

Aprenda a prepararse adecuadamente para ampliar los trabajadores de Asset compute y comprenda qué servicios y cuentas deben aprovisionarse y configurarse, y qué software debe instalarse localmente para el desarrollo.

### Aprovisionamiento de cuentas y servicios{#accounts-and-services}

Las siguientes cuentas y servicios requieren aprovisionamiento y acceso a para completar el tutorial, AEM como entorno de desarrollo de Cloud Service o programa de Simulador para pruebas, acceso a Adobe Project Firefly y Almacenamiento de blob de Microsoft Azure.

+ [Suministro de cuentas y servicios](./set-up/accounts-and-services.md)

### Entorno de desarrollo local

El desarrollo local de los proyectos de Asset compute requiere un conjunto de herramientas específicas para el desarrollo, diferente del desarrollo de AEM tradicional, que incluye: Microsoft Visual Studio Code, Docker Desktop, Node.js y módulos npm compatibles.

+ [Configurar el entorno de desarrollo local](./set-up/development-environment.md)

### Luciérnagas del proyecto Adobe

Los proyectos de asset compute son proyectos de Adobe definidos especialmente para proyectos de Firefly y, como tales, requieren acceso a Adobe Project Firefly en la consola de desarrolladores de Adobe para configurarlos e implementarlos.

+ [Configurar el proyecto de Adobe Firefly](./set-up/firefly.md)

## Desarrollar

Obtenga información sobre cómo crear y configurar un proyecto de Asset compute y, a continuación, desarrollar un programa de trabajo personalizado que genere una representación de recursos personalizada.

### Crear un nuevo proyecto de Asset compute

Los proyectos de asset compute, que contienen uno o más trabajadores de Asset compute, se generan mediante la Adobe I/O CLI interactiva. Los proyectos de asset compute son proyectos de Adobe especialmente estructurados de Firefly, que a su vez son proyectos de Node.js.

+ [Crear un nuevo proyecto de Asset compute](./develop/project.md)

### Configuración de variables de entorno

Las variables de entorno se mantienen en el archivo `.env` para desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento en la nube necesarias para el desarrollo local.

+ [Configuración de las variables de entorno](./develop/environment-variables.md)

### Configure manifest.yml

Los proyectos de asset compute contienen manifiestos que definen a todos los trabajadores de la Asset compute incluidos en el proyecto, así como los recursos de que disponen cuando se implementan en Adobe I/O Runtime para su ejecución.

+ [Configure manifest.yml](./develop/manifest.md)

### Desarrollar un trabajador

El desarrollo de un trabajador de Asset compute es el núcleo de la ampliación de los microservicios de Asset compute, ya que éste contiene el código personalizado que genera u organiza la generación de la representación de recursos resultante.

+ [Desarrollar un trabajador de Asset compute](./develop/worker.md)

### Uso de la herramienta de desarrollo de Assets computes

La herramienta de desarrollo de Assets computes proporciona un mazo de cables Web local para implementar, ejecutar y previsualizar las representaciones generadas por el trabajador, lo que permite un desarrollo rápido e iterativo de los trabajadores de Assets computes.

+ [Uso de la herramienta de desarrollo de Assets computes](./develop/development-tool.md)

## Prueba y depuración

Obtenga información sobre cómo probar a los trabajadores de Assets computes personalizadas para que tengan confianza en su funcionamiento y depurar a los trabajadores de Assets computes para que entiendan y solucionen problemas cómo se ejecuta el código personalizado.

### Probar un trabajador

La asset compute proporciona un marco de pruebas para crear grupos de pruebas para los trabajadores, lo que facilita la definición de pruebas que garanticen un comportamiento adecuado.

+ [Probar un trabajador](./test-debug/test.md)

### Depurar un trabajador

Los trabajadores de asset compute proporcionan varios niveles de depuración desde la salida tradicional `console.log(..)` hasta integraciones con __VS Code__ y __wskdebug__, lo que permite a los desarrolladores pasar por el código de trabajo a medida que se ejecuta en tiempo real.

+ [Depurar un trabajador](./test-debug/debug.md)

## Implementar

Obtenga información sobre cómo integrar a los trabajadores de Asset compute personalizados con AEM como Cloud Service, implementándolos primero en Adobe I/O Runtime y, a continuación, invocando desde AEM como autor Cloud Service a través de los Perfiles de procesamiento de Recursos AEM.

### Implementar en Adobe I/O Runtime

Los trabajadores de asset compute deben ser desplegados en Adobe I/O Runtime para ser utilizados con AEM como Cloud Service.

+ [Uso de Perfiles de procesamiento](./deploy/runtime.md)

### Integrar trabajadores mediante Perfiles de procesamiento de AEM

Una vez implementados en Adobe I/O Runtime, los trabajadores de Asset compute pueden registrarse en AEM como Cloud Service mediante [Perfiles de procesamiento de recursos](../../assets/configuring/processing-profiles.md). A su vez, los Perfiles de procesamiento se aplican a las carpetas de recursos que se aplican a los recursos que contienen.

+ [Integración con Perfiles de procesamiento de AEM](./deploy/processing-profiles.md)

## Avanzado 

Estos tutoriales abreviados abordan casos de uso más avanzados basados en las enseñanzas básicas establecidas en los capítulos anteriores.

+ [Desarrollar un ](./advanced/metadata.md) trabajo de metadatos de Asset compute que pueda volver a escribir metadatos en el

## El código base en Github

El código base del tutorial está disponible en Github en:

+ [ramificación maestra adobe/aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

El código fuente no contiene los archivos `.env` o `config.json` necesarios. Se deben agregar y configurar usando la información de [cuentas y servicios](#accounts-and-services).

## Recursos adicionales

A continuación se indican varios recursos de Adobe que proporcionan información adicional y API y SDK útiles para desarrollar trabajadores de Asset compute.

### Documentación

+ [Documentación del servicio de asset compute](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Léame de la herramienta de desarrollo de assets computes](https://github.com/adobe/asset-compute-devtool)
+ [Trabajadores de ejemplo de asset compute](https://github.com/adobe/asset-compute-example-workers)

### API y SDK

+ [SDK de asset compute](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca de ajustes de Blobstore de Adobe Cloud](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de reintentos de búsqueda de nodos de Adobe](https://github.com/adobe/node-fetch-retry)
+ [Trabajadores de ejemplo de asset compute](https://github.com/adobe/asset-compute-example-workers)

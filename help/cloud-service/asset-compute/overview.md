---
title: Extensibilidad de los microservicios de Asset Compute para AEM como Cloud Service
description: En este tutorial se explica la creación de un trabajador de cómputo de recursos sencillo que crea una representación de recursos recortando el recurso original en un círculo y aplicando el contraste y el brillo configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un trabajador de cómputo de recursos personalizado para utilizarlo con AEM como Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: ecee5f83dc778b016b6d236c1e3bcc4919ee55a7
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 0%

---


# Extensibilidad de los microservicios de Asset Compute

AEM los microservicios de cómputo de activos de Cloud Service admiten el desarrollo y la implementación de trabajadores personalizados que se utilizan para leer y manipular datos binarios de recursos almacenados en AEM, más comúnmente, para crear representaciones de recursos personalizadas.

Mientras que en AEM 6.x los procesos personalizados de flujo de trabajo de AEM se utilizaban para leer, transformar y escribir representaciones de recursos, en AEM como Cloud Service de recursos de cálculo satisfacen esta necesidad.

## Qué hará

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

En este tutorial se explica la creación de un trabajador de cómputo de recursos sencillo que crea una representación de recursos recortando el recurso original en un círculo y aplicando el contraste y el brillo configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un trabajador de cómputo de recursos personalizado para utilizarlo con AEM como Cloud Service.

### Objetivos {#objective}

1. Aprovisionar y configurar las cuentas y los servicios necesarios para crear e implementar un trabajador de Asset Compute
1. Creación y configuración de un proyecto de cómputo de recursos
1. Desarrollar un trabajador de cómputo de recursos que genere una representación personalizada
1. Escriba pruebas para el trabajador de cálculo de recursos personalizado y aprenda a depurar
1. Implementar el programa de trabajo de cómputo de recursos e integrarlo AEM como un servicio de creación de Cloud Service mediante Perfiles de procesamiento

## Configurar

Aprenda a prepararse adecuadamente para ampliar los trabajadores de Asset Compute y comprenda qué servicios y cuentas deben aprovisionarse y configurarse, y qué software debe instalarse localmente para el desarrollo.

### Aprovisionamiento de cuentas y servicios

Las siguientes cuentas y servicios requieren aprovisionamiento y acceso a para completar el tutorial, AEM como entorno de desarrollo de Cloud Service o programa de Simulador para pruebas, acceso a Adobe Project Firefly y Almacenamiento de blob de Microsoft Azure.

+ [Suministro de cuentas y servicios](./set-up/accounts-and-services.md)

### Entorno de desarrollo local

El desarrollo local de las aplicaciones de Asset Compute requiere un conjunto de herramientas de desarrollador específico, diferente del desarrollo de AEM tradicional, que incluye: Microsoft Visual Studio Code, Docker Desktop, Node.js y módulos npm compatibles.

+ [Configurar el entorno de desarrollo local](./set-up/development-environment.md)

### Luciérnagas del proyecto Adobe

Los proyectos de Asset Compute son aplicaciones de Adobe Project Firefly especialmente definidas y, como tales, requieren acceso a Adobe Project Firefly en la consola de desarrolladores de Adobe para configurarlas e implementarlas.

+ [Configurar el proyecto de Adobe Firefly](./set-up/firefly.md)

## Desarrollar

Obtenga información sobre cómo crear y configurar un proyecto de cómputo de recursos y, a continuación, desarrollar un programa de trabajo personalizado que genere una representación de recursos personalizada.

### Crear un nuevo proyecto de cómputo de recursos

Los proyectos de la aplicación de cómputo de recursos, que contienen uno o más trabajadores de computación de recursos, se generan mediante la CLI de E/S de Adobe interactiva. Las aplicaciones de Asset Compute son aplicaciones de Adobe Project Firefly especialmente estructuradas, que a su vez son aplicaciones de Node.js.

+ [Crear un nuevo proyecto de cómputo de recursos](./develop/project.md)

### Configuración de variables de entorno

Las variables de entorno se mantienen en el `.env` archivo para el desarrollo local y se utilizan para proporcionar las credenciales de E/S de Adobe y las credenciales de almacenamiento en la nube necesarias para el desarrollo local.

+ [Configuración de las variables de entorno](./develop/environment-variables.md)

### Configure manifest.yml

Las aplicaciones de cálculo de recursos contienen manifiestos que definen todos los trabajadores de cómputo de recursos incluidos en el proyecto, así como los recursos que están disponibles cuando se implementan en Adobe I/O Runtime para su ejecución.

+ [Configure manifest.yml](./develop/manifest.md)

### Desarrollar un trabajador

El desarrollo de un trabajador de cómputo de recursos es el núcleo de la ampliación de los microservicios de cómputo de recursos, ya que el trabajador contiene el código personalizado que genera o orquesta la generación de la representación de recursos resultante.

+ [Desarrollar un trabajador de cómputo de recursos](./develop/worker.md)

### Uso de la herramienta de desarrollo de cómputo de recursos

La Herramienta de desarrollo de cómputo de recursos proporciona un mazo Web local para implementar, ejecutar y previsualizar las representaciones generadas por el trabajador, lo que permite un desarrollo rápido e iterativo de los trabajadores de Asset Compute.

+ [Uso de la herramienta de desarrollo de cómputo de recursos](./develop/development-tool.md)

## Prueba y depuración

Obtenga información sobre cómo probar a los trabajadores personalizados de Asset Compute para que tengan confianza en su funcionamiento y depurar a los trabajadores de Asset Compute para que entiendan y solucionen problemas cómo se ejecuta el código personalizado.

### Probar un trabajador

Asset Compute proporciona un marco de pruebas para crear grupos de pruebas para trabajadores, lo que facilita la definición de pruebas que garanticen un comportamiento adecuado.

+ [Probar un trabajador](./test-debug/test.md)

### Depurar un trabajador

Las aplicaciones de Asset Compute proporcionan varios niveles de depuración desde la salida tradicional `console.log(..)` hasta integraciones con __VS Code__ y __wskdebug__, lo que permite a los desarrolladores pasar por el código de trabajo a medida que se ejecuta en tiempo real.

+ [Depurar un trabajador](./test-debug/debug.md)

## Implementar

Obtenga información sobre cómo integrar a los trabajadores personalizados de Asset Compute con AEM como Cloud Service, implementándolos primero en Adobe I/O Runtime e invocando desde AEM como autor Cloud Service a través de Perfiles de procesamiento de AEM Assets.

### Implementar en Adobe I/O Runtime

Los trabajadores de Asset Compute deben implementarse en Adobe I/O Runtime para poder usarlos con AEM como Cloud Service.

+ [Uso de Perfiles de procesamiento](./deploy/runtime.md)

### Integrar trabajadores mediante Perfiles de procesamiento de AEM

Una vez implementados en Adobe I/O Runtime, los trabajadores de Asset Compute pueden registrarse en AEM como Cloud Service mediante Perfiles [de procesamiento de](../../assets/configuring/processing-profiles.md)recursos. A su vez, los Perfiles de procesamiento se aplican a las carpetas de recursos que se aplican a los recursos que contienen.

+ [Integración con Perfiles de procesamiento de AEM](./deploy/processing-profiles.md)

## Recursos adicionales

Los siguientes son varios recursos de Adobe que proporcionan información adicional y API y SDK útiles para desarrollar los trabajadores de Asset Compute.

### Documentación

+ [Documentación de Asset Compute Service](https://docs.adobe.com/content/help/en/asset-compute/using/extend/understand-extensibility.html)
+ [Léame de la herramienta de desarrollo de cómputo de recursos](https://github.com/adobe/asset-compute-devtool)

### Otros ejemplos de código

+ [Trabajadores de ejemplo de cálculo de recursos](https://github.com/adobe/asset-compute-example-workers)

### API y SDK

+ [SDK de cómputo de recursos](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
+ [Biblioteca de ajustes de Blobstore de Adobe Cloud](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de reintentos de búsqueda de nodos de Adobe](https://github.com/adobe/node-fetch-retry)

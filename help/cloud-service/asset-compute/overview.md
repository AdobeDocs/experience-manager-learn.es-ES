---
title: Extensibilidad de los microservicios de asset compute para AEM as a Cloud Service
description: En este tutorial se explica cómo crear un sencillo trabajo de Asset compute que cree una representación de recursos recortando el recurso original en un círculo y aplicando un brillo y un contraste configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un trabajador de Asset compute personalizado para utilizarlo con AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '986'
ht-degree: 0%

---

# Extensibilidad de microservicios de asset compute

los microservicios de Asset compute AEM de as a Cloud Service admiten el desarrollo y la implementación de AEM personalizados que se utilizan para leer y manipular datos binarios de recursos almacenados en, generalmente, para crear representaciones de recursos personalizadas.

AEM AEM Mientras que en la versión 6.x se utilizaban procesos personalizados de flujo de trabajo de la para leer, transformar y escribir de nuevo representaciones de recursos, en la Asset compute de AEM as a Cloud Service los trabajadores satisfacen esta necesidad.

## Qué va a hacer

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

En este tutorial se explica cómo crear un sencillo trabajo de Asset compute que cree una representación de recursos recortando el recurso original en un círculo y aplicando un brillo y un contraste configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un trabajador de Asset compute personalizado para utilizarlo con AEM as a Cloud Service.

### Objetivos {#objective}

1. Aprovisionar y configurar las cuentas y los servicios necesarios para crear e implementar un trabajador de Asset compute
1. Creación y configuración de un proyecto de Asset compute
1. Desarrollar un trabajador de Asset compute que genere una representación personalizada
1. Escriba pruebas para y aprenda a depurar el trabajador de Asset compute personalizado
1. Implemente el trabajador de Asset compute e integre el servicio de AEM as a Cloud Service Author mediante Perfiles de procesamiento

## Configuración de

Aprenda a prepararse correctamente para la ampliación de los trabajadores de Asset compute y comprenda qué servicios y cuentas deben aprovisionarse y configurarse, y qué software debe instalarse localmente para el desarrollo.

### Aprovisionamiento de cuentas y servicios{#accounts-and-services}

Las siguientes cuentas y servicios requieren aprovisionamiento y acceso a para completar el tutorial, el entorno de desarrollo de AEM as a Cloud Service o el programa de zona protegida, el acceso a App Builder y el almacenamiento del blob de Microsoft Azure.

+ [Aprovisionar cuentas y servicios](./set-up/accounts-and-services.md)

### Entorno de desarrollo local

El desarrollo local de proyectos de Asset compute AEM requiere un conjunto de herramientas de desarrollador específico, diferente del desarrollo tradicional de la aplicación, que incluye: Microsoft Visual Studio Code, Docker Desktop, Node.js y módulos npm de compatibilidad.

+ [Configuración del entorno de desarrollo local](./set-up/development-environment.md)

### App Builder

Los proyectos de asset compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en Adobe Developer Console para configurarlos e implementarlos.

+ [Configuración de App Builder](./set-up/app-builder.md)

## Desarrollar

Obtenga información sobre cómo crear y configurar un proyecto de Asset compute y, a continuación, desarrollar un trabajador personalizado que genere una representación de recursos a medida.

### Creación de un nuevo proyecto de Asset compute

Los proyectos de asset compute, que contienen uno o más trabajadores de Asset compute, se generan mediante la CLI de Adobe I/O interactivo. Los proyectos de asset compute son proyectos de App Builder especialmente estructurados, que a su vez son proyectos de Node.js.

+ [Creación de un nuevo proyecto de Asset compute](./develop/project.md)

### Configuración de variables de entorno

Las variables de entorno se mantienen en el archivo `.env` para el desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento en la nube necesarias para el desarrollo local.

+ [Configuración de las variables de entorno](./develop/environment-variables.md)

### Configuración de manifest.yml

Los proyectos de asset compute contienen manifiestos que definen todos los Assets computes de trabajo contenidos en el proyecto, así como los recursos que tienen disponibles cuando se implementan en Adobe I/O Runtime para su ejecución.

+ [Configuración de manifest.yml](./develop/manifest.md)

### Desarrollo de un trabajador

El desarrollo de un trabajador de Asset compute es el núcleo de la ampliación de los microservicios de Asset compute, ya que el trabajador contiene el código personalizado que genera, u organiza, la generación de la representación de recursos resultante.

+ [Desarrollo de un trabajador de Asset compute](./develop/worker.md)

### Uso de la herramienta de desarrollo de Asset compute

La herramienta de desarrollo de Assets computes proporciona un mazo de cables Web local para implementar, ejecutar y previsualizar representaciones generadas por el trabajador, lo que permite un desarrollo de trabajo de Asset compute rápido e iterativo.

+ [Uso de la herramienta de desarrollo de Asset compute](./develop/development-tool.md)

## Prueba y depuración

Aprenda a probar los trabajadores de Asset compute personalizados para confiar en su operación y a depurar los trabajadores de Asset compute para comprender y solucionar problemas de cómo se ejecuta el código personalizado.

### Prueba de un trabajador

Asset Compute proporciona un marco de pruebas para crear grupos de pruebas para los trabajadores, lo que permite definir pruebas que garanticen que el comportamiento correcto sea fácil.

+ [Prueba de un trabajador](./test-debug/test.md)

### Depuración de un trabajador

Los trabajadores de asset compute proporcionan varios niveles de depuración, desde la salida tradicional de `console.log(..)` hasta las integraciones con __código VS__ y __wskdebug__, lo que permite a los desarrolladores recorrer el código de trabajo a medida que se ejecuta en tiempo real.

+ [Depuración de un trabajador](./test-debug/debug.md)

## Implementación de

Obtenga información sobre cómo integrar los trabajadores de Asset compute personalizados con AEM as a Cloud Service, implementándolos primero en Adobe I/O Runtime y luego invocando desde AEM as a Cloud Service Author a través de los Perfiles de procesamiento de AEM Assets.

### Implementación en Adobe I/O Runtime

Los trabajadores de asset compute deben implementarse en Adobe I/O Runtime para poder utilizarse con AEM as a Cloud Service.

+ [Uso de perfiles de procesamiento](./deploy/runtime.md)

### AEM Integración de trabajadores mediante perfiles de procesamiento de

Una vez implementados en Adobe I/O Runtime, los trabajadores de Asset compute se pueden registrar en AEM as a Cloud Service mediante [Perfiles de procesamiento de Assets](../../assets/configuring/processing-profiles.md). A su vez, los perfiles de procesamiento se aplican a las carpetas de recursos que se aplican a los recursos que contienen.

+ [AEM Integración con Perfiles de procesamiento de](./deploy/processing-profiles.md)

## Avanzado 

Estos tutoriales abreviados tratan casos de uso más avanzados basándose en los conocimientos básicos establecidos en los capítulos anteriores.

+ [Desarrolle un trabajador de metadatos de Asset compute](./advanced/metadata.md) que pueda volver a escribir metadatos en

## Base de código en Github

El código base del tutorial está disponible en Github en:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) en la rama maestra

El código fuente no contiene los `.env` o `config.json` archivos necesarios. Deben agregarse y configurarse usando su información de [cuentas y servicios](#accounts-and-services).

## Recursos adicionales

A continuación se muestran varios recursos de Adobe que proporcionan más información y API y SDK útiles para el desarrollo de trabajadores de Asset compute.

### Documentación

+ [Documentación del servicio de Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [léame de la herramienta de desarrollo de Assets computes](https://github.com/adobe/asset-compute-devtool)
+ [Trabajadores de ejemplo de Asset compute](https://github.com/adobe/asset-compute-example-workers)

### API y SDK

+ [SDK de Asset compute](https://github.com/adobe/asset-compute-sdk)
   + [Asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset compute XMP de la](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca del contenedor de blobstore de Adobe Cloud](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de reintento de recuperación de nodo de Adobe](https://github.com/adobe/node-fetch-retry)
+ [Trabajadores de ejemplo de Asset compute](https://github.com/adobe/asset-compute-example-workers)

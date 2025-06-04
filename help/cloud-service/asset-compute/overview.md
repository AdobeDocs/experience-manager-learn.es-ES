---
title: Extensibilidad de los microservicios de Asset Compute para AEM as a Cloud Service
description: Este tutorial muestra la creación de una sencilla herramienta de Asset Compute que crea un renderizado de un activo recortando el activo original en un círculo y aplicando contraste y brillo configurables. Aunque la herramienta en sí es básica, este tutorial la utiliza para explorar la creación, el desarrollo y la implementación de una herramienta de Asset Compute personalizada para su uso con AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
duration: 277
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '986'
ht-degree: 100%

---

# Extensibilidad de microservicios de Asset Compute

Los microservicios de Asset Compute de AEM as Cloud Service admiten el desarrollo y la implementación de herramientas personalizadas que se utilizan para leer y manipular datos binarios de recursos almacenados en AEM, generalmente para crear representaciones de recursos personalizadas.

Mientras que en AEM 6.x se utilizaban procesos de flujo de trabajo de AEM personalizados para leer, transformar y escribir representaciones de recursos, en Asset Compute de AEM as a Cloud Service las herramientas satisfacen esta necesidad.

## Lo que puede hacer

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial muestra la creación de una sencilla herramienta de Asset Compute que crea un renderizado de un activo recortando el activo original en un círculo y aplicando contraste y brillo configurables. Aunque la herramienta en sí es básica, este tutorial la utiliza para explorar la creación, el desarrollo y la implementación de una herramienta de Asset Compute personalizada para su uso con AEM as a Cloud Service.

### Objetivos {#objective}

1. Aprovisionar y configurar las cuentas y los servicios necesarios para crear e implementar una herramienta de Asset Compute
1. Creación y configuración de un proyecto de Asset Compute
1. Desarrollar una herramienta de Asset Compute que genere una representación personalizada
1. Escriba pruebas para y aprenda a depurar la herramienta personalizada de Asset Compute
1. Implemente la herramienta de Asset Compute e integre el servicio de Author de AEM as a Cloud Service mediante Perfiles de procesamiento

## Configurar

Aprenda a prepararse correctamente para la ampliación de los trabajadores de Asset Compute y comprenda qué servicios y cuentas deben aprovisionarse y configurarse, y qué software debe instalarse localmente para el desarrollo.

### Aprovisionamiento de cuentas y servicios{#accounts-and-services}

Las siguientes cuentas y servicios requieren aprovisionamiento y acceso a para completar el tutorial, el entorno de desarrollo de AEM as a Cloud Service o el programa de zona protegida, el acceso a App Builder y el almacenamiento del blob de Microsoft Azure.

+ [Aprovisionar cuentas y servicios](./set-up/accounts-and-services.md)

### Entorno de desarrollo local

El desarrollo local de proyectos de Asset Compute requiere un conjunto de herramientas de desarrollador específico, diferente del desarrollo tradicional de AEM, que incluye: Microsoft Visual Studio Code, Docker Desktop, Node.js y módulos npm de compatibilidad.

+ [Configuración de un entorno de desarrollo de local](./set-up/development-environment.md)

### App Builder

Los proyectos de Asset Compute son proyectos de App Builder especialmente definidos y, como tales, requieren acceso a App Builder en Adobe Developer Console para configurarlos e implementarlos.

+ [Configuración de App Builder](./set-up/app-builder.md)

## Desarrollar

Obtenga información sobre cómo crear y configurar un proyecto de Asset Compute y, a continuación, desarrollar una herramienta personalizada que genere una representación de recursos a medida.

### Crear un nuevo proyecto de Asset Compute

Los proyectos de Asset Compute, que contienen una o más herramientas de Asset Compute, se generan mediante Adobe I/O CLI interactiva. Los proyectos de Asset Compute son proyectos de App Builder especialmente estructurados, que a su vez son proyectos de Node.js.

+ [Crear un nuevo proyecto de Asset Compute](./develop/project.md)

### Configurar variables de entorno

Las variables de entorno se mantienen en el archivo `.env` para el desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento de nube necesarias para el desarrollo local.

+ [Configuración de las variables de entorno](./develop/environment-variables.md)

### Configuración de manifest.yml

Los proyectos de Asset Compute contienen manifiestos que definen todas las herramientas de Asset Compute contenidas en el proyecto, así como los recursos que tienen disponibles cuando se implementan en Adobe I/O Runtime para su ejecución.

+ [Configuración de manifest.yml](./develop/manifest.md)

### Desarrollo de una herramienta

El desarrollo de un trabajador de Asset Compute es el núcleo de la ampliación de los microservicios de Asset Compute, ya que la herramienta contiene el código personalizado que genera, o organiza, la generación de la representación de recursos resultante.

+ [Desarrollo de una herramienta de Asset Compute](./develop/worker.md)

### Uso de la herramienta de desarrollo de Asset Compute

La herramienta de desarrollo de Asset Compute proporciona un arnés web local para implementar, ejecutar y previsualizar representaciones generadas por el trabajador, lo que permite un desarrollo de herramientas de trabajo de Asset Compute rápido e iterativo.

+ [Uso de la herramienta de desarrollo de Asset Compute](./develop/development-tool.md)

## Prueba y depuración

Aprenda a probar las herramientas personalizadas de Asset Compute para confiar en su operación y a depurar las herramientas de Asset Compute para comprender y solucionar problemas de cómo se ejecuta el código personalizado.

### Probar una herramienta

Asset Compute proporciona un marco de pruebas para crear grupos de pruebas para las herramientas, lo que permite definir pruebas que garanticen que el comportamiento adecuado sea sencillo.

+ [Probar una herramienta](./test-debug/test.md)

### Depuración de una herramienta

Las herramientas de Asset Compute proporcionan varios niveles de depuración, desde la salida tradicional de `console.log(..)` hasta las integraciones con __código VS__ y __wskdebug__, lo que permite a los desarrolladores recorrer el código de herramientas de trabajo a medida que se ejecuta en tiempo real.

+ [Depuración de una herramienta](./test-debug/debug.md)

## Implementación

Obtenga información sobre cómo integrar las herramientas personalizadas de Asset Compute con AEM as a Cloud Service, implementándolas primero en Adobe I/O Runtime y luego invocando desde AEM as a Cloud Service Author a través de los Perfiles de procesamiento de AEM Assets.

### Implementación en Adobe I/O Runtime

Las herramientas de Asset Compute deben implementarse en Adobe I/O Runtime para poder utilizarse con AEM as a Cloud Service.

+ [Uso de perfiles de procesamiento](./deploy/runtime.md)

### Integración de trabajadores mediante perfiles de procesamiento de AEM

Una vez implementadas en Adobe I/O Runtime, las herramientas de Asset Compute se pueden registrar en AEM as a Cloud Service mediante [Perfiles de procesamiento de recursos](../../assets/configuring/processing-profiles.md). A su vez, los perfiles de procesamiento se aplican a las carpetas de recursos que se aplican a los recursos que contienen.

+ [Integración con perfiles de procesamiento de AEM](./deploy/processing-profiles.md)

## Avanzado 

Estos tutoriales abreviados tratan casos de uso más avanzados basándose en los conocimientos básicos establecidos en los capítulos anteriores.

+ [Desarrolle una herramienta de metadatos de Asset Compute](./advanced/metadata.md) que pueda volver a escribir metadatos en

## Código base en Github

El código base del tutorial está disponible en Github en:

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @ rama maestra

El código fuente no contiene los archivos `.env` o `config.json` necesarios. Deben añadirse y configurarse usando su información de [cuentas y servicios](#accounts-and-services).

## Recursos adicionales

A continuación se indican varios recursos de Adobe que proporcionan más información y API y SDK útiles para el desarrollo de herramientas de Asset Compute.

### Documentación

+ [Documentación del servicio de Asset Compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html?lang=es)
+ [léame de la herramienta de desarrollo de Asset Compute](https://github.com/adobe/asset-compute-devtool)
+ [Herramientas de ejemplo de Asset Compute](https://github.com/adobe/asset-compute-example-workers)

### APIs y SDK

+ [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk)
   + [Asset Compute Commons](https://github.com/adobe/asset-compute-commons)
   + [Asset Compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca envolvente de Adobe Cloud Blobstore](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de reintento de recuperación de nodos de Adobe](https://github.com/adobe/node-fetch-retry)
+ [Herramientas de ejemplo de Asset Compute](https://github.com/adobe/asset-compute-example-workers)

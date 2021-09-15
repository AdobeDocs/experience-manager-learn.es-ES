---
title: asset compute microservicios extensibilidad para AEM como Cloud Service
description: Este tutorial recorta la creación de un Asset compute de trabajo simple que crea una representación de recursos recortando el recurso original en un círculo y aplica el contraste y el brillo configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un Asset compute de trabajo personalizado para utilizarlo con AEM como Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1026'
ht-degree: 0%

---

# Capacidad de extensión de los microservicios de asset compute

AEM como microservicios de Asset compute de Cloud Service admiten el desarrollo y la implementación de trabajadores personalizados que se utilizan para leer y manipular datos binarios de recursos almacenados en AEM, normalmente para crear representaciones de recursos personalizadas.

Mientras que en AEM 6.x los procesos de flujo de trabajo AEM personalizados se utilizaban para leer, transformar y escribir representaciones de recursos, en AEM los trabajadores de Asset compute de Cloud Service satisfacen esta necesidad.

## Qué hará

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

Este tutorial recorta la creación de un Asset compute de trabajo simple que crea una representación de recursos recortando el recurso original en un círculo y aplica el contraste y el brillo configurables. Aunque el propio trabajador es básico, este tutorial lo utiliza para explorar la creación, el desarrollo y la implementación de un Asset compute de trabajo personalizado para utilizarlo con AEM como Cloud Service.

### Objetivos {#objective}

1. Aprovisionar y configurar las cuentas y los servicios necesarios para crear e implementar un trabajador de Asset compute
1. Creación y configuración de un proyecto de Asset compute
1. Desarrollar un programa de trabajo de Asset compute de am que genere una representación personalizada
1. Escriba pruebas para y aprenda a depurar el Asset compute de trabajo personalizado
1. Implemente el Asset compute de trabajo e inclúyalo AEM como un servicio Cloud Service Author a través de Perfiles de procesamiento

## Configuración

Aprenda a prepararse adecuadamente para la ampliación de los trabajadores del Asset compute, y comprenda qué servicios y cuentas deben aprovisionarse y configurarse, y qué software debe instalarse localmente para el desarrollo.

### Aprovisionamiento de cuentas y servicios{#accounts-and-services}

Las siguientes cuentas y servicios requieren aprovisionamiento y acceso a para completar el tutorial, AEM como entorno de desarrollo de Cloud Service o programa de espacio aislado, acceso a Proyecto de Adobe Firefly y almacenamiento de blob de Microsoft Azure.

+ [Suministro de cuentas y servicios](./set-up/accounts-and-services.md)

### Entorno de desarrollo local

El desarrollo local de los proyectos de Asset compute requiere un conjunto de herramientas para desarrolladores específico, diferente del desarrollo de AEM tradicional, que incluye: Microsoft Visual Studio Code, Docker Desktop, Node.js y módulos npm compatibles.

+ [Configuración del entorno de desarrollo local](./set-up/development-environment.md)

### Luciérnagas del proyecto Adobe

Los proyectos de asset compute son proyectos de Adobe especialmente definidos y como tales, requieren acceso a Proyecto de Adobe Firefly en la consola para desarrolladores de Adobe para configurarlos e implementarlos.

+ [Configuración del proyecto de Adobe Firefly](./set-up/firefly.md)

## Desarrollar

Obtenga información sobre cómo crear y configurar un proyecto de Asset compute y, a continuación, desarrollar un programa de trabajo personalizado que genere una representación de recursos personalizada.

### Crear un nuevo proyecto de Asset compute

Los proyectos de asset compute, que contienen uno o más trabajadores de Asset compute, se generan mediante la CLI de Adobe I/O interactiva. Los proyectos de asset compute son proyectos especialmente estructurados del Proyecto de Adobe Firefly, que a su vez son proyectos de Node.js.

+ [Crear un nuevo proyecto de Asset compute](./develop/project.md)

### Configuración de variables de entorno

Las variables de entorno se mantienen en el archivo `.env` para el desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento en la nube necesarias para el desarrollo local.

+ [Configuración de las variables de entorno](./develop/environment-variables.md)

### Configurar manifest.yml

Los proyectos de asset compute contienen manifiestos que definen todos los trabajadores del Asset compute contenidos en el proyecto, así como los recursos disponibles cuando se implementan en Adobe I/O Runtime para su ejecución.

+ [Configurar manifest.yml](./develop/manifest.md)

### Desarrollo de un trabajador

El desarrollo de un Asset compute de trabajo es la base de la ampliación de los microservicios de Asset compute, ya que el  contiene el código personalizado que genera u organiza la generación de la representación de recursos resultante.

+ [Desarrollo de un trabajador de Asset compute](./develop/worker.md)

### Uso de la herramienta de desarrollo de Asset compute

La herramienta de desarrollo de Assets computes proporciona un mazo de cables web local para implementar, ejecutar y previsualizar las representaciones generadas por el trabajador, lo que permite un desarrollo rápido e iterativo del trabajador de Asset compute.

+ [Uso de la herramienta de desarrollo de Asset compute](./develop/development-tool.md)

## Prueba y depuración

Obtenga información sobre cómo probar a los Assets computes personalizados para que estén seguros de su funcionamiento, y depurar a los trabajadores de Asset compute para que entiendan y solucionen problemas cómo se ejecuta el código personalizado.

### Probar un trabajador

asset compute proporciona un marco de pruebas para crear grupos de pruebas para los trabajadores, lo que facilita la definición de pruebas que garanticen un comportamiento adecuado.

+ [Probar un trabajador](./test-debug/test.md)

### Depurar un trabajador

Los trabajadores de asset compute proporcionan varios niveles de depuración desde la salida tradicional `console.log(..)` hasta integraciones con __Código VS__ y __wskdebug__, lo que permite a los desarrolladores pasar por el código de trabajo a medida que se ejecuta en tiempo real.

+ [Depurar un trabajador](./test-debug/debug.md)

## Implementar

Obtenga información sobre cómo integrar a los trabajadores de Asset compute personalizados con AEM como Cloud Service, implementándolos primero en Adobe I/O Runtime y luego invocando desde AEM como autor Cloud Service a través de los Perfiles de procesamiento de AEM Assets.

### Implementar en Adobe I/O Runtime

Los trabajadores de asset compute deben implementarse en Adobe I/O Runtime para su uso con AEM como Cloud Service.

+ [Uso de perfiles de procesamiento](./deploy/runtime.md)

### Integración de trabajadores mediante Perfiles de procesamiento de AEM

Una vez implementados en Adobe I/O Runtime, los trabajadores de Asset compute se pueden registrar en AEM como un Cloud Service a través de [Assets Processing Profiles](../../assets/configuring/processing-profiles.md). Los perfiles de procesamiento se aplican, a su vez, a las carpetas de recursos que se aplican a los recursos que contienen.

+ [Integración con Perfiles de procesamiento de AEM](./deploy/processing-profiles.md)

## Avanzado 

Estos tutoriales abreviados abordan casos de uso más avanzados basados en las enseñanzas básicas establecidas en capítulos anteriores.

+ [Desarrolle un ](./advanced/metadata.md) trabajo de metadatos de Asset compute que pueda volver a escribir metadatos en el

## Código base en Github

El código base del tutorial está disponible en Github en:

+ [rama maestra adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @

El código fuente no contiene los archivos `.env` o `config.json` necesarios. Se deben agregar y configurar usando la información de [cuentas y servicios](#accounts-and-services).

## Recursos adicionales

A continuación se indican varios recursos de Adobe que proporcionan más información y API y SDK útiles para el desarrollo de trabajadores de Asset compute.

### Documentación

+ [Documentación del servicio de asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [Léame de la herramienta de desarrollo de assets computes](https://github.com/adobe/asset-compute-devtool)
+ [asset compute ejemplo trabajadores](https://github.com/adobe/asset-compute-example-workers)

### API y SDK

+ [SDK de asset compute](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset compute XMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Biblioteca de envoltura de blobstore de Adobe Cloud](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Biblioteca de reintentos de recuperación de nodos de Adobe](https://github.com/adobe/node-fetch-retry)
+ [asset compute ejemplo trabajadores](https://github.com/adobe/asset-compute-example-workers)

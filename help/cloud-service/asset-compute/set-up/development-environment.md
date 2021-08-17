---
title: Configuración de un entorno de desarrollo local para la extensibilidad del Asset compute
description: El desarrollo de los Assets computes, que son aplicaciones JavaScript de Node.js, requiere herramientas de desarrollo específicas que difieren del desarrollo de AEM tradicional, que van desde Node.js y varios módulos npm hasta Docker Desktop y Microsoft Visual Studio Code.
feature: Microservicios de asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integraciones, desarrollo
role: Developer
level: Intermediate, Experienced
source-git-commit: fd72f3c85db8a56ec8abfd1609da53492ee54be2
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 0%

---


# Configuración del entorno de desarrollo local

Los proyectos de Asset compute de Adobe no se pueden integrar con el tiempo de ejecución de AEM local proporcionado por el SDK de AEM y se desarrollan utilizando su propia cadena de herramientas, aparte de la requerida por AEM aplicaciones basadas en el arquetipo de proyecto de AEM Maven.

Para ampliar los microservicios de Asset compute, deben instalarse las siguientes herramientas en el equipo de desarrollo local.

## Instrucciones de configuración abreviadas

A continuación se muestran las instrucciones de configuración abreviadas. Los detalles sobre estas herramientas de desarrollo se describen en secciones discretas a continuación.

1. [Instale el ](https://www.docker.com/products/docker-desktop) escritorio Docker y extraiga las imágenes de Docker necesarias:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Instalar código de Visual Studio](https://code.visualstudio.com/download)
1. [Instalación de Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale los módulos npm requeridos y los complementos CLI de Adobe I/O desde la línea de comandos:

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obtener más información sobre las instrucciones de instalación abreviadas, lea las secciones siguientes.

## Instalar código de Visual Studio{#vscode}

[El código de Microsoft Visual Studio ](https://code.visualstudio.com/download) se utiliza para desarrollar y depurar Assets computes. Mientras que se puede utilizar otro IDE [compatible con JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) para desarrollar el programa de trabajo, solo se puede integrar el código de Visual Studio al programa de trabajo del Asset compute [debug](../test-debug/debug.md).

Este tutorial supone el uso de código de Visual Studio, ya que proporciona la mejor experiencia para desarrolladores para ampliar el Asset compute.

## Instalar Docker Desktop{#docker}

Descargue e instale los proyectos de Asset compute [Docker Desktop](https://www.docker.com/products/docker-desktop) más recientes y estables, ya que esto es necesario para [probar](../test-debug/test.md) y [depurar](../test-debug/debug.md) localmente.

Después de instalar Docker Desktop, inícielo e instale las siguientes imágenes de Docker desde la línea de comandos:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Los desarrolladores de equipos Windows deben asegurarse de que utilizan contenedores Linux para las imágenes anteriores. Los pasos para cambiar a contenedores Linux se describen en la documentación [Docker for Windows](https://docs.docker.com/docker-for-windows/).

## Instalación de Node.js (y npm){#node-js}

Los trabajadores de asset compute están basados en [Node.js](https://nodejs.org/) y, por lo tanto, requieren que Node.js 10+ (y npm) se desarrolle y cree.

+ [Instale Node.js (y npm)](../../local-development-environment/development-tools.md#node-js)  de la misma manera que para el desarrollo de AEM tradicional.

## Instalación de la CLI de Adobe I/O{#aio}

[Instale la CLI](../../local-development-environment/development-tools.md#aio-cli) de Adobe I/O o  ____ aiois un módulo npm de línea de comandos (CLI) que facilite el uso de las tecnologías de Adobe I/O y la interacción con ellas, y se utiliza tanto para generar como para desarrollar localmente trabajadores de Asset compute personalizados.

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_Se requiere la versión 7.1.0 de la CLI de Adobe I/O. Las versiones posteriores de la CLI de Adobe I/O no son compatibles en este momento._


## Instalación del complemento de Asset compute de CLI de Adobe I/O{#aio-asset-compute}

El [complemento de Asset compute de CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalación de wskdebug{#wskdebug}

Descargue e instale el módulo npm [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) para facilitar la depuración local de los trabajadores de Asset compute.

_Visual Studio Code 1.48.x+ es necesario para que  [](#wskdebug) wskdebugto funcione._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar ngrok{#ngrok}

Descargue e instale el módulo npm [ngrok](https://www.npmjs.com/package/ngrok), que proporciona acceso público a su máquina de desarrollo local, para facilitar la depuración local de los trabajadores de Asset compute.

```
$ npm install -g ngrok --unsafe-perm=true
```

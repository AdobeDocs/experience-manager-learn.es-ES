---
title: Configuración de un entorno de desarrollo local para la extensibilidad de Asset Compute
description: El desarrollo de los trabajadores de Asset Compute, que son aplicaciones JavaScript de Node.js, requiere herramientas de desarrollo específicas que difieren del desarrollo tradicional de AEM, que van desde Node.js y varios módulos npm hasta Docker Desktop y Microsoft Visual Studio Code.
feature: Microservicios de Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integraciones, desarrollo
role: Desarrollador
level: Intermedio, con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---


# Configuración del entorno de desarrollo local

Los proyectos de Adobe Asset Compute no se pueden integrar con el tiempo de ejecución local de AEM que proporciona el SDK de AEM y se desarrollan con su propia cadena de herramientas, aparte de la que requieren las aplicaciones AEM basadas en el tipo de archivo del proyecto AEM Maven.

Para ampliar los microservicios de Asset Compute, deben instalarse las siguientes herramientas en el equipo de desarrollo local.

## Instrucciones de configuración abreviadas

A continuación se muestran las instrucciones de configuración abreviadas. Los detalles sobre estas herramientas de desarrollo se describen en secciones discretas a continuación.

1. [Instale el ](https://www.docker.com/products/docker-desktop) escritorio Docker y extraiga las imágenes de Docker necesarias:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Instalar código de Visual Studio](https://code.visualstudio.com/download)
1. [Instalación de Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale los módulos npm requeridos y los complementos CLI de Adobe I/O desde la línea de comandos:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obtener más información sobre las instrucciones de instalación abreviadas, lea las secciones siguientes.

## Instalar código de Visual Studio{#vscode}

[El código de Microsoft Visual Studio ](https://code.visualstudio.com/download) se utiliza para desarrollar y depurar los trabajadores de Asset Compute. Mientras que se puede utilizar otro [IDE compatible con JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) para desarrollar el programa de trabajo, solo se puede integrar el código de Visual Studio al programa de trabajo de Asset Compute [debug](../test-debug/debug.md).

_Visual Studio Code 1.48.x+ es necesario para que  [](#wskdebug) wskdebugto funcione._

Este tutorial supone el uso de código de Visual Studio, ya que proporciona la mejor experiencia para desarrolladores para ampliar Asset Compute.

## Instalar Docker Desktop{#docker}

Descargue e instale los proyectos más recientes y estables de [Docker Desktop](https://www.docker.com/products/docker-desktop), ya que esto es necesario para [probar](../test-debug/test.md) y [depurar](../test-debug/debug.md) Asset Compute localmente.

Después de instalar Docker Desktop, inícielo e instale las siguientes imágenes de Docker desde la línea de comandos:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Los desarrolladores de equipos Windows deben asegurarse de que utilizan contenedores Linux para las imágenes anteriores. Los pasos para cambiar a contenedores Linux se describen en la documentación [Docker for Windows](https://docs.docker.com/docker-for-windows/).

## Instalación de Node.js (y npm){#node-js}

Los trabajadores de Asset Compute están basados en [Node.js](https://nodejs.org/) y, por lo tanto, requieren que Node.js 10+ (y npm) se desarrolle y cree.

+ [Instale Node.js (y npm)](../../local-development-environment/development-tools.md#node-js)  de la misma manera que para el desarrollo tradicional de AEM.

## Instalación de Adobe I/O CLI{#aio}

[Instale la CLI](../../local-development-environment/development-tools.md#aio-cli) de Adobe I/O o  ____ aiois un módulo npm de línea de comandos (CLI) que facilite el uso y la interacción con las tecnologías de Adobe I/O y se utilice tanto para generar como para desarrollar localmente los trabajadores personalizados de Asset Compute.

```
$ npm install -g @adobe/aio-cli
```

## Instalación del complemento de Adobe I/O CLI Asset Compute{#aio-asset-compute}

El [complemento de Asset Compute CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Descargue e instale el módulo npm [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) para facilitar la depuración local de los trabajadores de Asset Compute.

_Visual Studio Code 1.48.x+ es necesario para que  [](#wskdebug) wskdebugto funcione._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar ngrok{#ngrok}

Descargue e instale el módulo npm [ngrok](https://www.npmjs.com/package/ngrok), que proporciona acceso público a su equipo de desarrollo local, para facilitar la depuración local de los trabajadores de Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```

---
title: Configuración de un entorno de desarrollo local para la extensibilidad de Asset Compute
description: El desarrollo de los trabajadores de Asset Compute, que son aplicaciones JavaScript de Node.js, requiere herramientas de desarrollo específicas que difieren del desarrollo tradicional de AEM, que van desde Node.js y varios módulos npm hasta Docker Desktop y Microsoft Visual Studio Code.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
duration: 96
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Configuración del entorno de desarrollo local

Los proyectos de Adobe Asset Compute no se pueden integrar con el tiempo de ejecución local de AEM que proporciona AEM SDK y se desarrollan con su propia cadena de herramientas, aparte de la que requieren las aplicaciones de AEM basadas en el arquetipo de proyecto de AEM Maven.

Para ampliar los microservicios de Asset Compute, se deben instalar las siguientes herramientas en el equipo de desarrollo local.

## Instrucciones de configuración abreviadas

A continuación se muestran unas instrucciones de configuración abreviadas. Los detalles sobre estas herramientas de desarrollo se describen en secciones discretas a continuación.

1. [Instale Docker Desktop](https://www.docker.com/products/docker-desktop) y extraiga las imágenes de Docker necesarias:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Instalar código de Visual Studio](https://code.visualstudio.com/download)
1. [Instalar Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale los módulos npm y los complementos CLI de Adobe I/O necesarios desde la línea de comandos:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obtener más información sobre las instrucciones de instalación abreviadas, lea las secciones siguientes.

## Instalar código de Visual Studio{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) se usa para desarrollar y depurar trabajadores de Asset Compute. Aunque otros [IDE](../../local-development-environment/development-tools.md#set-up-the-development-ide) compatibles con JavaScript se pueden usar para desarrollar el trabajo, solo el código de Visual Studio se puede integrar en [debug](../test-debug/debug.md) Asset Compute worker.

Este tutorial supone el uso de código de Visual Studio, ya que proporciona la mejor experiencia para desarrolladores para ampliar Asset Compute.

## Instalar Docker Desktop{#docker}

Descargue e instale el [Docker Desktop](https://www.docker.com/products/docker-desktop) más reciente y estable, ya que es necesario para [probar](../test-debug/test.md) y [depurar](../test-debug/debug.md) proyectos de Asset Compute localmente.

Después de instalar Docker Desktop, inícielo e instale las siguientes imágenes de Docker desde la línea de comandos:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Los desarrolladores de equipos con Windows deben asegurarse de utilizar contenedores de Linux para las imágenes anteriores. Los pasos para cambiar a contenedores Linux se describen en la [documentación de Docker para Windows](https://docs.docker.com/docker-for-windows/).

## Instalación de Node.js (y npm){#node-js}

Los trabajadores de Asset Compute están basados en [Node.js](https://nodejs.org/) y, por lo tanto, requieren Node.js 10+ (y npm) para desarrollar y generar.

+ [Instale Node.js (y npm)](../../local-development-environment/development-tools.md#node-js) de la misma manera que para el desarrollo tradicional de AEM.

## Instalar CLI de Adobe I/O{#aio}

[Instale la CLI de Adobe I/O](../../local-development-environment/development-tools.md#aio-cli) o __aio__ es un módulo npm de línea de comandos (CLI) que facilita el uso y la interacción con las tecnologías de Adobe I/O y se utiliza tanto para generar como para desarrollar localmente trabajadores personalizados de Asset Compute.

```
$ npm install -g @adobe/aio-cli
```

## Instale el complemento Asset Compute de la CLI de Adobe I/O{#aio-asset-compute}

[Complemento Asset Compute CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Descargue e instale el módulo [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm para facilitar la depuración local de los trabajadores de Asset Compute.

Se requiere _código de Visual Studio 1.48.x+ para que funcione [wskdebug](#wskdebug)._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar ngrok{#ngrok}

Descargue e instale el módulo [ngrok](https://www.npmjs.com/package/ngrok) npm, que proporciona acceso público a su equipo de desarrollo local, para facilitar la depuración local de los trabajadores de Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```

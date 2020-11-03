---
title: Configure un entorno de desarrollo local para la extensibilidad de Asset Compute
description: El desarrollo de los trabajadores de Asset Compute, que son aplicaciones JavaScript de Node.js, requiere herramientas de desarrollo específicas que difieren del desarrollo de AEM tradicional, que van desde Node.js y varios módulos npm hasta Docker Desktop y Microsoft Visual Studio Code.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# Configurar el entorno de desarrollo local

Los proyectos de cálculo de recursos de Adobe no pueden integrarse con el tiempo de ejecución de AEM local proporcionado por el SDK de AEM y se desarrollan utilizando su propia cadena de herramientas, separada de la requerida por las aplicaciones AEM basadas en el arquetipo de proyecto AEM Maven.

Para ampliar los microservicios de Asset Compute, se deben instalar las siguientes herramientas en el equipo de desarrollo local.

## Instrucciones de configuración abreviadas

Las siguientes son instrucciones de configuración abreviadas. Los detalles sobre estas herramientas de desarrollo se describen en secciones discretas a continuación.

1. [Instale Docker Desktop](https://www.docker.com/products/docker-desktop) y extraiga las imágenes de Docker necesarias:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Instalar código de Visual Studio](https://code.visualstudio.com/download)
1. [Instalar Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale los módulos npm y los complementos CLI de E/S de Adobe requeridos desde la línea de comandos:

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obtener más información sobre las instrucciones de instalación abreviadas, lea las secciones siguientes.

## Instalar código de Visual Studio{#vscode}

[El código](https://code.visualstudio.com/download) de Microsoft Visual Studio se utiliza para desarrollar y depurar los trabajadores de Asset Compute. Aunque se pueden usar otros IDE [compatibles con](../../local-development-environment/development-tools.md#set-up-the-development-ide) JavaScript para desarrollar el programa de trabajo, solo se puede integrar el código de Visual Studio para [depurar](../test-debug/debug.md) el programa de trabajo de Asset Compute.

_Se requiere código de Visual Studio 1.48.x+ para que [wskdebug](#wskdebug) funcione._

En este tutorial se asume el uso del código de Visual Studio, ya que proporciona la mejor experiencia de desarrollador para ampliar Asset Compute.

## Instalar el escritorio del acoplador{#docker}

Descargue e instale la versión más reciente y estable de [Docker Desktop](https://www.docker.com/products/docker-desktop), ya que esto es necesario para [probar](../test-debug/test.md) y [depurar](../test-debug/debug.md) localmente los proyectos de Asset Compute.

Después de instalar Docker Desktop, inicio e instale las siguientes imágenes de Docker desde la línea de comandos:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Los desarrolladores de equipos Windows deben asegurarse de que utilizan contenedores Linux para las imágenes anteriores. Los pasos para cambiar a contenedores Linux se describen en la documentación [de](https://docs.docker.com/docker-for-windows/)Docker for Windows.

## Instalar Node.js (y npm){#node-js}

Los trabajadores de Asset Compute están basados en [Node.js](https://nodejs.org/)y, por lo tanto, requieren que Node.js 10+ (y npm) se desarrolle y cree.

+ [Instale Node.js (y npm)](../../local-development-environment/development-tools.md#node-js) de la misma manera que para el desarrollo de AEM tradicional.

## Instalación de CLI de E/S de Adobe{#aio}

[Instale la CLI](../../local-development-environment/development-tools.md#aio-cli)de E/S de Adobe, o __aio__ es un módulo npm de línea de comandos (CLI) que facilita el uso de tecnologías de E/S de Adobe y la interacción con ellas, y se utiliza tanto para generar como para desarrollar localmente los trabajadores personalizados de Asset Compute.

```
$ npm install -g @adobe/aio-cli
```

## Instalación del complemento de cómputo de recursos CLI de E/S de Adobe{#aio-asset-compute}

El complemento de cómputo de recursos CLI de E/S de [Adobe](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Descargue e instale el módulo npm de depuración [](https://www.npmjs.com/package/@openwhisk/wskdebug) Apache OpenWhisk para facilitar la depuración local de los trabajadores de Asset Compute.

_Se requiere código de Visual Studio 1.48.x+ para que [wskdebug](#wskdebug) funcione._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar ngrok{#ngrok}

Descargue e instale el módulo [ngrok](https://www.npmjs.com/package/ngrok) npm, que proporciona acceso público a su equipo de desarrollo local, para facilitar la depuración local de los trabajadores de Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```

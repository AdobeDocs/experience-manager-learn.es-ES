---
title: Configuración de un entorno de desarrollo local para la extensibilidad del Asset compute
description: El desarrollo de los Asset compute AEM de trabajo, que son aplicaciones JavaScript de Node.js, requiere herramientas de desarrollo específicas que difieren del desarrollo tradicional de los programas, que van desde Node.js y varios módulos npm hasta Docker Desktop y Microsoft Visual Studio Code.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 1%

---

# Configuración del entorno de desarrollo local

Los proyectos de Asset compute AEM AEM AEM AEM de Adobe no se pueden integrar con el tiempo de ejecución local de la proporcionado por el SDK y se desarrollan con su propia cadena de herramientas, aparte de la requerida por las aplicaciones de la en función del arquetipo del proyecto Maven de la.

Para ampliar los microservicios de Asset compute, se deben instalar las siguientes herramientas en el equipo de desarrollo local.

## Instrucciones de configuración abreviadas

A continuación se muestran unas instrucciones de configuración abreviadas. Los detalles sobre estas herramientas de desarrollo se describen en secciones discretas a continuación.

1. [Instalar Docker Desktop](https://www.docker.com/products/docker-desktop) y extraiga las imágenes de Docker necesarias:

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Instalar código de Visual Studio](https://code.visualstudio.com/download)
1. [Instalar Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Instale los módulos npm y los complementos CLI de Adobe I/O necesarios desde la línea de comandos:

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Para obtener más información sobre las instrucciones de instalación abreviadas, lea las secciones siguientes.

## Instalar código de Visual Studio{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) se utiliza para desarrollar y depurar Assets computes de trabajo. Mientras que otros [IDE compatible con JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) se puede utilizar para desarrollar el trabajador, sólo el código de Visual Studio se puede integrar en [depurar](../test-debug/debug.md) Trabajador del asset compute.

En este tutorial se da por hecho el uso de código de Visual Studio, ya que proporciona la mejor experiencia para el desarrollador a la hora de ampliar los Assets computes.

## Instalar Docker Desktop{#docker}

Descargue e instale la última versión estable [Docker Desktop](https://www.docker.com/products/docker-desktop), ya que es necesario para [prueba](../test-debug/test.md) y [depurar](../test-debug/debug.md) Asset compute proyectos localmente.

Después de instalar Docker Desktop, inícielo e instale las siguientes imágenes de Docker desde la línea de comandos:

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Los desarrolladores de equipos con Windows deben asegurarse de utilizar contenedores de Linux para las imágenes anteriores. Los pasos para cambiar a contenedores Linux se describen en la sección [Documentación de Docker para Windows](https://docs.docker.com/docker-for-windows/).

## Instalación de Node.js (y npm){#node-js}

Los trabajadores del asset compute son [Node.js](https://nodejs.org/)basado en y, por lo tanto, requiere Node.js 10+ (y npm) para desarrollar y generar.

+ [Instalación de Node.js (y npm)](../../local-development-environment/development-tools.md#node-js) AEM de la misma manera que para el desarrollo tradicional de la.

## Instalar CLI de Adobe I/O{#aio}

[Instale la CLI de Adobe I/O](../../local-development-environment/development-tools.md#aio-cli), o __aio__ es un módulo npm de línea de comandos (CLI) que facilita el uso de las tecnologías de Adobe I/O y la interacción con ellas, y se utiliza tanto para generar como para desarrollar localmente Assets computes personalizados.

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_Se requiere la versión 7.1.0 de CLI de Adobe I/O. En este momento no se admiten versiones posteriores de CLI de Adobe I/O._


## Instalación del complemento de Asset compute CLI de Adobe I/O{#aio-asset-compute}

El [Complemento de Asset compute de CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Instalar wskdebug{#wskdebug}

Descargue e instale [Depuración de Apache OpenWhisk](https://www.npmjs.com/package/@openwhisk/wskdebug) npm para facilitar la depuración local de los trabajadores de Asset compute.

_Visual Studio Code 1.48.x+ es necesario para [wskdebug](#wskdebug) para trabajar._

```
$ npm install -g @openwhisk/wskdebug
```

## Instalar ngrok{#ngrok}

Descargue e instale [ngrok](https://www.npmjs.com/package/ngrok) npm module, que proporciona acceso público a su equipo de desarrollo local, para facilitar la depuración local de los trabajadores de Asset compute.

```
$ npm install -g ngrok --unsafe-perm=true
```

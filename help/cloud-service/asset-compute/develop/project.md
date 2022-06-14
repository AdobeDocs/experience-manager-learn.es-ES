---
title: Creación de un proyecto de Asset compute para la extensibilidad del Asset compute
description: Los proyectos de asset compute son proyectos de Node.js, generados mediante la CLI de Adobe I/O, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM as a Cloud Service.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: 839152aa67ba7ab2929f2c8093bfdc873761a645
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# Creación de un proyecto de Asset compute

Los proyectos de asset compute son proyectos de Node.js, generados usando la CLI de Adobe I/O, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM as a Cloud Service. Un solo proyecto de Asset compute puede contener uno o más trabajadores de Asset compute, cada uno de los cuales tiene un punto final HTTP discreto referenciable desde un perfil de procesamiento as a Cloud Service de AEM.

## Generación de un proyecto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Pulsación para generar un proyecto de Asset compute (sin audio)_

Utilice la variable [Complemento de Asset compute CLI de Adobe I/O](../set-up/development-environment.md#aio-cli) para generar un nuevo proyecto de Asset compute vacío.

1. Desde la línea de comandos, vaya a la carpeta para contener el proyecto.
1. Desde la línea de comandos, ejecute `aio app init` para iniciar la CLI de generación de proyectos interactiva.
   + Este comando puede generar un explorador web que solicite la autenticación en el Adobe I/O. Si es así, proporcione las credenciales de Adobe asociadas con la variable [los servicios y productos requeridos de Adobe](../set-up/accounts-and-services.md). Si no puede iniciar sesión, siga [estas instrucciones sobre cómo generar un proyecto](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleccionar organización__
   + Seleccione la organización de Adobe que tiene AEM as a Cloud Service, App Builder está registrado con
1. __Seleccionar proyecto__
   + Busque y seleccione el proyecto. Esta es la [Título del proyecto](../set-up/app-builder.md) creada a partir de la plantilla de proyecto de App Builder, en este caso `WKND AEM Asset Compute`
1. __Seleccionar Workspace__
   + Seleccione el `Development` workspace
1. __¿Qué funciones de la aplicación de Adobe I/O desea habilitar para este proyecto? Seleccionar componentes para incluir__
   + Seleccione `Actions: Deploy runtime actions`
   + Utilice las teclas de flechas para seleccionar y el espacio para anular la selección o seleccionarlo, e Intro para confirmar la selección
1. __Seleccione el tipo de acciones que desea generar__
   + Seleccione `DX Asset Compute Worker v1`
   + Utilice las teclas de flechas para seleccionar, el espacio para anular la selección o seleccionarlo y la tecla Intro para confirmar la selección
1. __¿Cómo le gustaría nombrar esta acción?__
   + Usar el nombre predeterminado `worker`.
   + Si el proyecto contiene varios trabajadores que realizan diferentes cálculos de recursos, asígneles un nombre semántico

## Generar console.json

La herramienta para desarrolladores requiere un archivo denominado `console.json` que contiene las credenciales necesarias para conectarse a Adobe I/O. Este archivo se descarga desde la consola de Adobe I/O.

1. Abra la página del trabajador del Asset compute [Adobe I/O](https://console.adobe.io) proyecto
1. Seleccione el espacio de trabajo del proyecto para descargar el `console.json` credenciales para, en este caso seleccione `Development`
1. Vaya a la raíz del proyecto de Adobe I/O y pulse __Descargar todo__ en la esquina superior derecha.
1. Un archivo se descarga como `.json` archivo con el prefijo proyecto y espacio de trabajo, por ejemplo: `wkndAemAssetCompute-81368-Development.json`
1. Puede
   + Cambie el nombre del archivo como `console.json` y muévalo a la raíz del proyecto de trabajo de Asset compute. Este es el enfoque de este tutorial.
   + Moverlo a una carpeta arbitraria Y hacer referencia a esa carpeta desde su `.env` archivo con una entrada de configuración `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. La ruta del archivo puede ser absoluta o relativa a la raíz del proyecto. Por ejemplo:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      O bien
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> NOTA
> El archivo contiene credenciales. Si almacena el archivo dentro del proyecto, asegúrese de agregarlo al `.gitignore` para evitar que se compartan. Lo mismo se aplica a la variable `.env` archivo — Estos archivos de credenciales no deben compartirse ni almacenarse en Git.

## Proyecto de asset compute en GitHub

El proyecto de Asset compute final está disponible en GitHub en:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene el estado final del proyecto, totalmente completado con los casos de trabajo y prueba, pero no contiene credenciales, es decir, `.env`, `console.json` o `.aio`._

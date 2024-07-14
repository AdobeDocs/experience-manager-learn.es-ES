---
title: Creación de un proyecto de Asset compute para la extensibilidad de la Asset compute
description: Los proyectos de asset compute son proyectos de Node.js, generados mediante la CLI de Adobe I/O, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM as a Cloud Service.
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# Creación de un proyecto de Asset compute

Los proyectos de asset compute son proyectos de Node.js, generados mediante la CLI de Adobe I/O, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM as a Cloud Service. Un solo proyecto de Asset compute puede contener uno o más Assets computes de trabajo, cada una con un punto final HTTP discreto al que se puede hacer referencia desde un perfil de procesamiento de AEM as a Cloud Service.

## Generar un proyecto

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Pulsación para generar un proyecto de Asset compute (sin audio)_

Use el complemento de Asset compute [Adobe I/O CLI](../set-up/development-environment.md#aio-cli) para generar un nuevo proyecto de Asset compute vacío.

1. Desde la línea de comandos, vaya a la carpeta que contiene el proyecto.
1. Desde la línea de comandos, ejecute `aio app init` para iniciar la CLI de generación de proyectos interactiva.
   + Este comando puede generar un explorador web que solicite autenticación para el Adobe I/O. Si es así, proporcione las credenciales de Adobe asociadas con los [servicios y productos de Adobe necesarios](../set-up/accounts-and-services.md). Si no puede iniciar sesión, siga [estas instrucciones sobre cómo generar un proyecto](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleccionar organización__
   + Seleccione la organización de Adobe que tiene AEM as a Cloud Service y App Builder están registrados con
1. __Seleccionar proyecto__
   + Busque y seleccione el proyecto. Este es el [título de proyecto](../set-up/app-builder.md) creado a partir de la plantilla de proyecto de App Builder, en este caso `WKND AEM Asset Compute`
1. __Seleccionar Workspace__
   + Seleccionar el área de trabajo `Development`
1. __¿Qué características de la aplicación de Adobe I/O desea habilitar para este proyecto? Seleccionar componentes para incluir__
   + Seleccionar `Actions: Deploy runtime actions`
   + Utilice las teclas de flecha para seleccionar y el espacio para anular la selección o seleccionarla, y Entrar para confirmar la selección
1. __Seleccionar tipo de acciones para generar__
   + Seleccionar `DX Asset Compute Worker v1`
   + Utilice las teclas de flecha para seleccionar, el espacio para anular la selección o seleccionarla y la tecla Intro para confirmar la selección
1. __¿Cómo desea asignar un nombre a esta acción?__
   + Utilice el nombre predeterminado `worker`.
   + Si el proyecto contiene varios trabajadores que realizan diferentes cálculos de recursos, asígneles un nombre semántico

## Generar console.json

La herramienta para desarrolladores requiere un archivo denominado `console.json` que contiene las credenciales necesarias para conectarse a la Adobe I/O. Este archivo se descarga desde la consola de Adobe I/O.

1. Abra el proyecto [Adobe I/O](https://console.adobe.io) del trabajador de Asset compute
1. Seleccione el área de trabajo del proyecto para el que desea descargar las credenciales de `console.json`, en este caso seleccione `Development`
1. Vaya a la raíz del proyecto de Adobe I/O y pulse __Descargar todo__ en la esquina superior derecha.
1. Un archivo se descarga como un archivo de `.json` con el prefijo del proyecto y del área de trabajo, por ejemplo: `wkndAemAssetCompute-81368-Development.json`
1. Puede hacer lo siguiente
   + Cambie el nombre del archivo como `console.json` y muévalo a la raíz del proyecto de trabajo de Asset compute. Este es el enfoque de este tutorial.
   + Muévalo a una carpeta arbitraria Y haga referencia a esa carpeta desde su archivo `.env` con una entrada de configuración `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. La ruta de acceso del archivo puede ser absoluta o relativa a la raíz del proyecto. Por ejemplo:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     O bien
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> NOTA
> El archivo contiene credenciales. Si almacena el archivo dentro del proyecto, asegúrese de agregarlo al archivo `.gitignore` para evitar que se comparta. Lo mismo se aplica al archivo `.env`: estos archivos de credenciales no deben compartirse ni almacenarse en Git.

## Proyecto de asset compute en GitHub

El proyecto de Asset compute final está disponible en GitHub en:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene el estado final del proyecto, totalmente relleno con los casos de prueba y trabajo, pero no contiene ninguna credencial, es decir, `.env`, `console.json` o `.aio`._

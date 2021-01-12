---
title: Creación de un proyecto de Asset compute para la extensibilidad de la Asset compute
description: Los proyectos de asset compute son proyectos de Node.js, generados mediante la CLI de Adobe I/O, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM como Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: c551eb984d8fefe19a979ce8c556289caa6805d8
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 0%

---


# Creación de un proyecto de Asset compute

Los proyectos de asset compute son proyectos de Node.js, generados mediante la CLI de Adobe I/O, que se ajustan a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM como Cloud Service. Un solo proyecto de Asset compute puede contener uno o más trabajadores de Asset compute, cada uno de los cuales tiene un punto final HTTP discreto referenciable desde un AEM como Perfil de procesamiento de Cloud Service.

## Generación de un proyecto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Pulsación para generar un proyecto de Asset compute (sin audio)_


Utilice el [complemento de Asset compute CLI de Adobe I/O](../set-up/development-environment.md#aio-cli) para generar un nuevo proyecto de Asset compute vacío.

1. Desde la línea de comandos, desplácese hasta la carpeta en la que desee contener el proyecto.
1. Desde la línea de comandos, ejecute `aio app init` para iniciar la CLI de generación de proyectos interactiva.
   + Esto puede generar un explorador Web que solicite autenticación en Adobe I/O. Si es así, proporcione las credenciales de Adobe asociadas con los [productos y servicios de Adobe requeridos](../set-up/accounts-and-services.md). Si no puede iniciar sesión, siga [estas instrucciones sobre cómo generar un proyecto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleccionar organización__
   + Seleccione la organización de Adobe que tiene AEM como Cloud Service, Project Firefly se registra en
1. __Seleccionar proyecto__
   + Busque y seleccione el proyecto. Este es el [título del proyecto](../set-up/firefly.md) creado a partir de la plantilla de proyecto de Firefly, en este caso `WKND AEM Asset Compute`
1. __Seleccionar espacio de trabajo__
   + Seleccione el espacio de trabajo `Development`
1. __¿Qué funciones de la aplicación de Adobe I/O desea habilitar para este proyecto? Seleccionar componentes para incluir__
   + Seleccione `Actions: Deploy runtime actions`
   + Utilice las teclas de flechas para seleccionar y espaciar para anular la selección o selección, y pulse Intro para confirmar la selección
1. __Seleccionar el tipo de acciones que se generarán__
   + Seleccione `Adobe Asset Compute Worker`
   + Utilice las teclas de flechas para seleccionar, espacio para anular la selección o seleccionar, e Intro para confirmar la selección
1. __¿Cómo desea nombrar esta acción?__
   + Utilice el nombre predeterminado `worker`.
   + Si el proyecto contiene varios trabajadores que realizan diferentes cálculos de recursos, asígneles un nombre semántico

## Generate console.json

Desde la raíz del proyecto de Asset compute recién creado, ejecute el siguiente comando para generar un `console.json`.

```
$ aio app use
```

Compruebe que los detalles del espacio de trabajo actual son correctos, presione `Y` o introduzca para generar un `console.json`. Si `.env` y `.aio` se detectan como ya existentes, toque `x` para omitir su creación.

## Revisar la anatomía del proyecto

El proyecto de Asset compute generado es un proyecto de Node.js para un proyecto de Adobe especializado de Firefly, los siguientes son idiosincráticos para el proyecto de Asset compute:

+ `/actions` contiene subcarpetas y cada subcarpeta define un programa de trabajo de Asset compute.
   + `/actions/<worker-name>/index.js` define el JavaScript ejecutado para realizar el trabajo de este trabajador.
      + El nombre de la carpeta `worker` es un valor predeterminado y puede ser cualquier cosa, siempre y cuando esté registrado en `manifest.yml`.
      + Se puede definir más de una carpeta de trabajo en `/actions` según sea necesario; sin embargo, debe estar registrada en `manifest.yml`.
+ `/test/asset-compute` contiene los grupos de pruebas para cada trabajador. De forma similar a la carpeta `/actions`, `/test/asset-compute` puede contener varias subcarpetas, cada una correspondiente al programa de trabajo que prueba.
   + `/test/asset-compute/worker`, que representa un grupo de pruebas para un trabajador específico, contiene subcarpetas que representan un caso de prueba específico, junto con la entrada de prueba, los parámetros y la salida esperada.
+ `/build` contiene la salida, los registros y los artefactos de las ejecuciones de casos de prueba de Asset compute.
+ `/manifest.yml` define los trabajadores de Asset compute que proporciona el proyecto. Cada implementación de trabajador debe enumerarse en este archivo para que esté disponible para AEM como Cloud Service.
+ `/console.json` define las configuraciones de Adobe I/O
   + Este archivo se puede generar o actualizar mediante el comando `aio app use`.
+ `/.aio` contiene configuraciones utilizadas por la herramienta CLI de aio.
   + Este archivo se puede generar o actualizar mediante el comando `aio app use`.
+ `/.env` define las variables de entorno en una  `key=value` sintaxis y contiene secretos que no se deben compartir. Esto se puede generar o Para proteger estos secretos, este archivo NO se debe registrar en Git y se ignora a través del archivo `.gitignore` predeterminado del proyecto.
   + Este archivo se puede generar o actualizar mediante el comando `aio app use`.
   + Las variables definidas en este archivo se pueden sobrescribir mediante [variables de exportación](../deploy/runtime.md) en la línea de comandos.

Para obtener más detalles sobre la revisión de la estructura del proyecto, consulte la [Anatomía de un proyecto de Adobe Firefly](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

La mayor parte del desarrollo se lleva a cabo en la carpeta `/actions` que desarrolla implementaciones de trabajador y en `/test/asset-compute` pruebas de escritura para los trabajadores de Asset compute personalizados.

## Proyecto de asset compute en Github

El proyecto de Asset compute final está disponible en Github en:

+ [aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene es el estado final del proyecto, totalmente rellenado con los casos de trabajo y prueba, pero no contiene credenciales, por ejemplo. `.env`,  `console.json` o  `.aio`._


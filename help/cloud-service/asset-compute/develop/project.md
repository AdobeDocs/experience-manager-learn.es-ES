---
title: Creación de un proyecto de cómputo de recursos para la extensibilidad de cómputo de recursos
description: Los proyectos de Asset Compute son proyectos de Node.js, generados mediante la CLI de E/S de Adobe, que se adhieren a una estructura determinada que permite implementarlos en Adobe I/O Runtime e integrarlos con AEM como Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 1%

---


# Creación de un proyecto de cómputo de recursos

Los proyectos de Asset Compute son proyectos de Node.js, generados mediante la CLI de E/S de Adobe, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM como Cloud Service. Un solo proyecto de cómputo de recursos puede contener uno o más trabajadores de cómputo de recursos, cada uno de los cuales tiene un punto final HTTP discreto referenciable desde un AEM como Perfil de procesamiento de Cloud Service.

## Generación de un proyecto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)
_Pulsación para generar un proyecto de cómputo de recursos (sin audio)_


Utilice el complemento [](../set-up/development-environment.md#aio-cli) Adobe I/O CLI Asset Compute para generar un nuevo proyecto de cómputo de recursos vacío.

1. Desde la línea de comandos, desplácese hasta la carpeta en la que desee contener el proyecto.
1. Desde la línea de comandos, ejecute `aio app init` para iniciar la CLI de generación de proyectos interactiva.
   + Esto puede generar un explorador Web que solicite autenticación en E/S Adobe. Si es así, proporcione las credenciales de Adobe asociadas con los productos [y servicios de Adobe](../set-up/accounts-and-services.md)necesarios. Si no puede iniciar sesión, siga estas instrucciones para generar un proyecto.
1. __Seleccionar organización__
   + Seleccione la organización de Adobe que tiene AEM como Cloud Service, Project Firefly se registra en
1. __Seleccionar proyecto__
   + Busque y seleccione el proyecto. Este es el título [del](../set-up/firefly.md) proyecto creado a partir de la plantilla de proyecto de Firefly, en este caso `WKND AEM Asset Compute`
1. __Seleccionar espacio de trabajo__
   + Seleccione el espacio de trabajo `Development`
1. __¿Qué funciones de la aplicación de E/S de Adobe desea habilitar para este proyecto? Seleccionar componentes para incluir__
   + Seleccione `Actions: Deploy runtime actions`
   + Utilice las teclas de flechas para seleccionar y espaciar para anular la selección o selección, y pulse Intro para confirmar la selección
1. __Seleccionar el tipo de acciones que se generarán__
   + Seleccione `Adobe Asset Compute Worker`
   + Utilice las teclas de flechas para seleccionar, espacio para anular la selección o seleccionar, e Intro para confirmar la selección
1. __¿Cómo desea nombrar esta acción?__
   + Utilice el nombre predeterminado `worker`.
   + Si el proyecto contiene varios trabajadores que realizan diferentes cálculos de recursos, asígneles un nombre semántico

## Revisar la anatomía del proyecto

El proyecto de cómputo de recursos generado es un proyecto de Node.js para un proyecto de Adobe especializado de proyectos de Firefly; los siguientes proyectos son idiosincrásicos del proyecto de cálculo de recursos:

+ `/actions` contiene subcarpetas y cada subcarpeta define un trabajador de cálculo de recursos.
   + `/actions/<worker-name>/index.js` define el JavaScript ejecutado para realizar el trabajo de este trabajador.
      + El nombre de la carpeta `worker` es un valor predeterminado y puede ser cualquier cosa, siempre y cuando esté registrado en la `manifest.yml`.
      + Se puede definir más de una carpeta de trabajo `/actions` según sea necesario, aunque se deben registrar en la `manifest.yml`.
+ `/test/asset-compute` contiene los grupos de pruebas para cada trabajador. Al igual que la `/actions` carpeta, `/test/asset-compute` puede contener varias subcarpetas, cada una correspondiente al programa de trabajo que prueba.
   + `/test/asset-compute/worker`, que representa un grupo de pruebas para un trabajador específico, contiene subcarpetas que representan un caso de prueba específico, junto con la entrada de prueba, los parámetros y la salida esperada.
+ `/build` contiene la salida, los registros y los artefactos de las ejecuciones de casos de prueba de cálculo de recursos.
+ `/manifest.yml` define los trabajadores de cómputo de recursos que proporciona el proyecto. Cada implementación de trabajador debe enumerarse en este archivo para que esté disponible para AEM como Cloud Service.
+ `/.aio` contiene configuraciones utilizadas por la herramienta CLI de aio. Este archivo se puede configurar mediante el `aio config` comando .
+ `/.env` define las variables de entorno en una `key=value` sintaxis y contiene secretos que no se deben compartir. Para proteger estos secretos, NO se debe registrar este archivo en Git y se ignora mediante el `.gitignore` archivo predeterminado del proyecto.
   + Las variables definidas en este archivo se pueden anular [exportando variables](../deploy/runtime.md) en la línea de comandos.

Para obtener más detalles sobre la revisión de la estructura del proyecto, revise el proyecto [Anatomía de un proyecto Adobe Firefly](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

La mayor parte del desarrollo se realiza en la `/actions` carpeta que desarrolla implementaciones de trabajador y en pruebas de `/test/asset-compute` escritura para los trabajadores de Asset Compute personalizados.

## Proyecto de cómputo de recursos en Github

El proyecto final de Asset Compute está disponible en Github en:

+ [aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene es el estado final del proyecto, totalmente rellenado con los casos de trabajo y prueba, pero no contiene credenciales, por ejemplo.`.env`,`.config.json`o`.aio`._
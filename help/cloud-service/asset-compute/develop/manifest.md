---
title: Configuración del manifest.yml de un proyecto de Asset compute
description: El manifest.yml del proyecto de Asset compute describe todos los trabajadores de este proyecto que se implementarán.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 0%

---

# Configuración de manifest.yml

El `manifest.yml`, ubicado en la raíz del proyecto de Asset compute, describe todos los trabajadores de este proyecto que se van a implementar.

![manifest.yml](./assets/manifest/manifest.png)

## Definición de trabajador predeterminada

Los trabajadores se definen como entradas de acción de Adobe I/O Runtime en `actions`y consta de un conjunto de configuraciones.

Los trabajadores que accedan a otras integraciones de Adobe I/O deben establecer la variable `annotations -> require-adobe-auth` propiedad a `true` como esta [expone las credenciales de Adobe I/O del trabajador](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) a través de `params.auth` objeto. Esto suele ser necesario cuando el trabajador llama a las API de Adobe I/O como las API de Adobe Photoshop, Lightroom o Sensei, y se puede alternar por trabajador.

1. Abrir y revisar el trabajador generado automáticamente `manifest.yml`. Los proyectos que contienen varios Assets computes de trabajo deben definir una entrada para cada trabajador en la sección `actions` matriz.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## Definición de límites

Cada trabajador puede configurar el [límites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) para su contexto de ejecución en Adobe I/O Runtime. Estos valores deben ajustarse para proporcionar un tamaño óptimo para el trabajador, en función del volumen, la tasa y el tipo de recursos que calculará, así como el tipo de trabajo que realiza.

Revisar [Guía de tamaño de Adobe](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) antes de establecer límites. Los trabajadores de asset compute pueden quedarse sin memoria al procesar los recursos, lo que provoca que se elimine la ejecución de Adobe I/O Runtime, por lo que asegúrese de que el tamaño del trabajador sea adecuado para gestionar todos los recursos candidatos.

1. Añadir un `inputs` a la nueva sección `wknd-asset-compute` entrada de acciones. Esto permite ajustar el rendimiento general y la asignación de recursos del trabajador de la Asset compute.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## El manifest.yml terminado

La final `manifest.yml` tiene este aspecto:

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.yml en Github

La final `.manifest.yml` está disponible en Github en:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validación de manifest.yml

Una vez generada la Asset compute `manifest.yml` se ha actualizado, ejecute la herramienta de desarrollo local y asegúrese de que el se inicia correctamente con el actualizado `manifest.yml` configuración.

Para iniciar la herramienta de desarrollo de Assets computes para el proyecto de Asset compute:

1. Abra una línea de comandos en la raíz del proyecto de Asset compute (en VS Code esto se puede abrir directamente en el IDE a través de Terminal > Nuevo terminal) y ejecute el comando:

   ```
   $ aio app run
   ```

1. La herramienta de desarrollo de Assets computes local se abrirá en el explorador web predeterminado en __http://localhost:9000__.

   ![ejecución de aplicación aio](assets/environment-variables/aio-app-run.png)

1. Observe la salida de la línea de comandos y el explorador Web en busca de mensajes de error cuando se inicializa la herramienta de desarrollo.
1. Para detener la herramienta de desarrollo de Assets computes, pulse `Ctrl-C` en la ventana que se ejecutó `aio app run` para finalizar el proceso.

## Solución de problemas

+ [Sangría YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [El límite memorySize se ha establecido en un valor demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)

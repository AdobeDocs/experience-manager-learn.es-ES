---
title: Configuración del manifest.yml de un proyecto de Asset Compute
description: El manifest.yml del proyecto Asset Compute describe todos los trabajadores de este proyecto que se implementarán.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# Configuración de manifest.yml

`manifest.yml`, ubicado en la raíz del proyecto de Asset Compute, describe todos los trabajadores de este proyecto que se van a implementar.

![manifest.yml](./assets/manifest/manifest.png)

## Definición de trabajador predeterminada

Los trabajadores se definen como entradas de acción de Adobe I/O Runtime en `actions` y se componen de un conjunto de configuraciones.

Los trabajadores que tengan acceso a otras integraciones de Adobe I/O deben establecer la propiedad de `annotations -> require-adobe-auth` en `true`, ya que este(a) [expone las credenciales de Adobe I/O del trabajador](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=es#access-adobe-apis) a través del objeto `params.auth`. Esto suele ser necesario cuando el trabajador llama a las API de Adobe I/O, como las API de Adobe Photoshop, Lightroom o Sensei, y se puede alternar por trabajador.

1. Abra y revise el trabajador `manifest.yml` generado automáticamente. Los proyectos que contienen varios trabajadores de Asset Compute deben definir una entrada para cada trabajador en la matriz `actions`.

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

Cada trabajador puede configurar los [límites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) para su contexto de ejecución en Adobe I/O Runtime. Estos valores deben ajustarse para proporcionar un tamaño óptimo para el trabajador, en función del volumen, la tasa y el tipo de recursos que calculará, así como el tipo de trabajo que realiza.

Revise [las instrucciones de tamaño de Adobe](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=es#sizing-workers) antes de establecer límites. Los trabajadores de Asset Compute pueden quedarse sin memoria al procesar los recursos, lo que provoca que se elimine la ejecución de Adobe I/O Runtime, por lo que asegúrese de que el tamaño del trabajador sea adecuado para gestionar todos los recursos candidatos.

1. Agregar una sección `inputs` a la nueva entrada de acciones `wknd-asset-compute`. Esto permite ajustar el rendimiento general y la asignación de recursos del trabajador de Asset Compute.

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

El `manifest.yml` final tiene el siguiente aspecto:

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

El `.manifest.yml` final está disponible en Github en:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validación de manifest.yml

Una vez que se haya actualizado la Asset Compute `manifest.yml` generada, ejecute la herramienta de desarrollo local y asegúrese de que se inicia correctamente con la configuración `manifest.yml` actualizada.

Para iniciar la herramienta de desarrollo de Asset Compute para el proyecto de Asset Compute:

1. Abra una línea de comandos en la raíz del proyecto de Asset Compute (en el código VS, esto se puede abrir directamente en el IDE a través de Terminal > Nuevo terminal) y ejecute el comando:

   ```
   $ aio app run
   ```

1. La herramienta de desarrollo de Asset Compute local se abrirá en el explorador web predeterminado en __http://localhost:9000__.

   ![ejecución de aplicación aio](assets/environment-variables/aio-app-run.png)

1. Observe la salida de la línea de comandos y el explorador Web en busca de mensajes de error cuando se inicializa la herramienta de desarrollo.
1. Para detener la herramienta de desarrollo de Asset Compute, pulse `Ctrl-C` en la ventana que ejecutó `aio app run` para finalizar el proceso.

## Solución de problemas

+ [Sangría YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [El límite memorySize se ha establecido en un valor demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)

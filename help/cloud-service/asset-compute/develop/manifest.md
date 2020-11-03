---
title: Configurar manifest.yml de un proyecto de Asset Compute
description: manifest.yml del proyecto de Asset Compute describe todos los trabajadores de este proyecto que se implementarán.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Configure manifest.yml

El `manifest.yml`, ubicado en la raíz del proyecto Asset Compute, describe todos los trabajadores de este proyecto que se van a implementar.

![manifest.yml](./assets/manifest/manifest.png)

## Definición de trabajador predeterminada

Los trabajadores se definen como entradas de acción de Adobe I/O Runtime en `actions`y están formadas por un conjunto de configuraciones.

Los trabajadores que acceden a otras integraciones de E/S de Adobe deben establecer la `annotations -> require-adobe-auth` propiedad en `true` , ya que esto [expone las credenciales](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) de E/S de Adobe del trabajador a través del `params.auth` objeto. Esto suele ser necesario cuando el trabajador llama a las API de E/S de Adobe, como las API de Adobe Photoshop, Lightroom o Sensei, y se puede alternar por trabajador.

1. Abra y revise el programa de trabajo generado automáticamente `manifest.yml`. Los proyectos que contienen varios trabajadores de cálculo de recursos deben definir una entrada para cada trabajador de la `actions` matriz.

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

## Definir límites

Cada trabajador puede configurar los [límites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) para su contexto de ejecución en Adobe I/O Runtime. Estos valores deben ajustarse para proporcionar un tamaño óptimo al trabajador, en función del volumen, la tasa y el tipo de recursos que computará, así como del tipo de trabajo que realiza.

Revise la guía [de tamaño del](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) Adobe antes de establecer los límites. Los trabajadores de Asset Compute pueden quedarse sin memoria al procesar recursos, lo que resulta en que se anule la ejecución de Adobe I/O Runtime, por lo que debe asegurarse de que el trabajador tenga el tamaño adecuado para gestionar todos los recursos candidatos.

1. Añada una `inputs` sección a la nueva entrada de `wknd-asset-compute` acciones. Esto permite ajustar el rendimiento general y la asignación de recursos del trabajador de cómputo de recursos.

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

El final `manifest.yml` es como:

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


## Validación del manifest.yml

Una vez que se actualice el cálculo de recursos generado, ejecute la herramienta de desarrollo local y asegúrese de que los inicios se realicen correctamente con la `manifest.yml` `manifest.yml` configuración actualizada.

Para la herramienta de desarrollo de cómputo de recursos de inicio para el proyecto de cómputo de recursos:

1. Abra una línea de comandos en la raíz del proyecto de cómputo de recursos (en el código VS se puede abrir directamente en el IDE mediante Terminal > Nuevo terminal) y ejecute el comando:

   ```
   $ aio app run
   ```

1. La herramienta de desarrollo de cómputo de recursos local se abrirá en el explorador Web predeterminado en __http://localhost:9000__.

   ![ejecución de la aplicación de AIO](assets/environment-variables/aio-app-run.png)

1. Observe el resultado de la línea de comandos y el explorador Web para ver los mensajes de error a medida que se inicializa la herramienta de desarrollo.
1. Para detener la herramienta de desarrollo de cómputo de recursos, toque `Ctrl-C` la ventana que se ejecutó `aio app run` para finalizar el proceso.

## Solución de problemas

+ [Sangría de YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [el límite de memorySize está establecido en demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)

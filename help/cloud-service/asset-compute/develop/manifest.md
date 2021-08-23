---
title: Configuración del manifest.yml de un proyecto de Asset compute
description: manifest.yml del proyecto de Asset compute describe todos los trabajadores en este proyecto que se implementarán.
feature: Microservicios de asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integraciones, desarrollo
role: Developer
level: Intermediate, Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# Configurar manifest.yml

El `manifest.yml`, ubicado en la raíz del proyecto de Asset compute, describe todos los trabajadores de este proyecto que se implementarán.

![manifest.yml](./assets/manifest/manifest.png)

## Definición de trabajador predeterminada

Los trabajadores se definen como entradas de acción de Adobe I/O Runtime en `actions` y se componen de un conjunto de configuraciones.

Los trabajadores que accedan a otras integraciones de Adobe I/O deben establecer la propiedad `annotations -> require-adobe-auth` en `true`, ya que [expone las credenciales de Adobe I/O del trabajador](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) mediante el objeto `params.auth`. Esto suele ser necesario cuando el trabajador llama a las API de Adobe I/O, como las API de Adobe Photoshop, Lightroom o Sensei, y se puede alternar por trabajador.

1. Abra y revise el trabajador generado automáticamente `manifest.yml`. Los proyectos que contienen varios Assets computes deben definir una entrada para cada trabajador en la matriz `actions`.

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

Cada trabajador puede configurar los [limits](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) para su contexto de ejecución en Adobe I/O Runtime. Estos valores deben ajustarse para proporcionar un tamaño óptimo para el trabajador, en función del volumen, la tasa y el tipo de recursos que computará, así como del tipo de trabajo que realiza.

Revise [guía de tamaño de Adobe](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) antes de establecer límites. Los trabajadores de asset compute pueden quedarse sin memoria al procesar recursos, lo que provoca que se cancele la ejecución de Adobe I/O Runtime, por lo que debe asegurarse de que el trabajador tenga el tamaño adecuado para gestionar todos los recursos candidatos.

1. Agregue una sección `inputs` a la nueva entrada de acciones `wknd-asset-compute`. Esto permite ajustar el rendimiento general y la asignación de recursos del trabajador de Asset compute.

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

Una vez actualizado el Asset compute generado `manifest.yml`, ejecute la herramienta de desarrollo local y asegúrese de que comienza correctamente con la configuración actualizada de `manifest.yml`.

Para iniciar la herramienta de desarrollo de Asset compute para el proyecto de Asset compute:

1. Abra una línea de comandos en la raíz del proyecto de Asset compute (en el código VS esto se puede abrir directamente en el IDE a través de Terminal > Nuevo terminal) y ejecute el comando:

   ```
   $ aio app run
   ```

1. La herramienta de desarrollo de Assets computes local se abrirá en su explorador web predeterminado en __http://localhost:9000__.

   ![ejecución de aplicación de aio](assets/environment-variables/aio-app-run.png)

1. Vea los mensajes de error en la salida de la línea de comandos y en el explorador web a medida que se inicializa la herramienta de desarrollo.
1. Para detener la herramienta de desarrollo de Asset compute, pulse `Ctrl-C` en la ventana que ejecutó `aio app run` para finalizar el proceso.

## Solución de problemas

+ [Sangría YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize limit está configurado en demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)

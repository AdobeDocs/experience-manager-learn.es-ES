---
title: Herramienta de desarrollo de asset compute
description: La herramienta de desarrollo de Asset compute AEM es un mazo de cables web local que permite a los desarrolladores configurar y ejecutar los creadores de Asset Computer localmente, fuera del contexto del SDK de la con los recursos de Asset compute de Adobe I/O Runtime.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Herramienta de desarrollo de asset compute

La herramienta de desarrollo de Asset compute AEM es un mazo de cables web local que permite a los desarrolladores configurar y ejecutar los creadores de Asset Computer localmente, fuera del contexto del SDK de la con los recursos de Asset compute de Adobe I/O Runtime.

## Ejecutar la herramienta de desarrollo de Assets computes

La herramienta de desarrollo de Assets computes se puede ejecutar desde la raíz del proyecto de Asset compute mediante el comando terminal:

```
$ aio app run
```

Esto iniciará la herramienta de desarrollo en __http://localhost:9000__ y la abrirá automáticamente en una ventana del explorador. Para que se ejecute la herramienta de desarrollo, [se debe proporcionar un devToolToken válido y generado automáticamente mediante un parámetro de consulta](#troubleshooting__devtooltoken).

## Comprensión de la interfaz de Herramientas de desarrollo de Asset compute{#interface}

![Herramienta de desarrollo de Asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Archivo Source:__ La selección del archivo de origen se usa para:
   + Se seleccionó el binario de recursos que actúa como binario `source` que se pasó al trabajador de Asset compute
   + Cargar archivos de origen
1. __definición de perfiles de Asset compute:__ Define el trabajador de Asset compute que se va a ejecutar, incluyendo los parámetros: incluyendo el punto final de la dirección URL del trabajador, el nombre de la representación resultante y cualquier parámetro
1. __Ejecutar:__ El botón Ejecutar ejecuta el perfil de Asset compute tal como se define en el editor de perfiles de configuración de Asset compute
1. __Anular:__ El botón Anular cancela una ejecución iniciada al pulsar el botón Ejecutar
1. __Solicitud/respuesta:__ Proporciona la solicitud y respuesta HTTP al/del trabajador de Asset compute que se ejecuta en Adobe I/O Runtime. Esto puede resultar útil para la depuración
1. __Registros de activación:__ Registros que describen la ejecución del trabajador de Asset compute, junto con los errores. Esta información también está disponible en la salida estándar `aio app run`
1. __Representaciones:__ Muestra todas las representaciones generadas por la ejecución del trabajador de Asset compute
1. __parámetro de consulta devToolToken:__ El token de la herramienta de desarrollo de Assets computes requiere que esté presente un parámetro de consulta `devToolToken` válido. Este token se genera automáticamente cada vez que se genera una nueva herramienta de desarrollo

### Ejecutar un trabajador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Pulsación para ejecutar un trabajo de Asset compute en la herramienta de desarrollo (sin audio)_

1. Asegúrese de que la herramienta de desarrollo de Assets computes se inicie desde la raíz del proyecto mediante el comando `aio app run`.
1. En la herramienta de desarrollo de Assets computes, cargue o seleccione un [archivo de imagen de muestra](../assets/samples/sample-file.jpg)
   + Asegúrese de que el archivo esté seleccionado en la lista desplegable __Archivo Source__
1. Revise el área de texto __definición de perfil de Asset compute__
   + La clave `worker` define la dirección URL del trabajador de Asset compute implementado
   + La clave `name` define el nombre de la representación que se va a generar
   + Se pueden proporcionar otras claves o valores en este objeto JSON y están disponibles en el trabajador en el objeto `rendition.instructions`
      + Opcionalmente, agregue valores para `size`, `contrast` y `brightness`:

        ```json
        {
            "renditions": [
                {
                    "worker": "...",
                    "name": "rendition.png",
                    "size":"800",
                    "contrast": "0.30",
                    "brightness": "-0.15"
                }
            ]
        }
        ```

1. Pulse el botón __Ejecutar__
1. La __sección Representaciones__ se rellenará con un marcador de lugar de representación
1. Una vez que el trabajador finalice, el marcador de posición de representación mostrará la representación generada

Realizar cambios en el código del trabajador mientras se ejecuta la herramienta de desarrollo hará que los cambios se &quot;implementen&quot;. La &quot;implementación en caliente&quot; tarda varios segundos, por lo que permita que la implementación se complete antes de volver a ejecutar el trabajador desde la herramienta de desarrollo.

## Resolución de problemas

+ [Sangría YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [El límite memorySize se ha establecido en un valor demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [No se puede iniciar la herramienta de desarrollo porque falta private.key](../troubleshooting.md#missing-private-key)
+ [Menú desplegable de archivos Source incorrecto](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parámetro de consulta devToolToken faltante o no válido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [No se pueden eliminar los archivos de origen](../troubleshooting.md#unable-to-remove-source-files)
+ [Representación parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

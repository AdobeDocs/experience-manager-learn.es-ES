---
title: Herramienta de desarrollo de assets computes
description: La herramienta de desarrollo de Asset compute es un mazo de cables web local que permite a los desarrolladores configurar y ejecutar los trabajadores de Asset Computer localmente, fuera del contexto del SDK de AEM para los recursos de Asset compute en Adobe I/O Runtime.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# Herramienta de desarrollo de assets computes

La herramienta de desarrollo de Asset compute es un mazo de cables web local que permite a los desarrolladores configurar y ejecutar los trabajadores de Asset Computer localmente, fuera del contexto del SDK de AEM para los recursos de Asset compute en Adobe I/O Runtime.

## Ejecutar la herramienta de desarrollo de Asset compute

La herramienta de desarrollo de Assets computes se puede ejecutar desde la raíz del proyecto de Asset compute mediante el comando terminal:

```
$ aio app run
```

Esto iniciará la herramienta de desarrollo en __http://localhost:9000__ y ábralo automáticamente en una ventana del explorador. Para que se ejecute la herramienta de desarrollo, [se debe proporcionar un devToolToken válido y generado automáticamente mediante un parámetro de consulta](#troubleshooting__devtooltoken).

## Explicación de la interfaz de las herramientas de desarrollo de Asset compute{#interface}

![Herramienta de desarrollo de assets computes](./assets/development-tool/asset-compute-dev-tool.png)

1. __Archivo de origen:__ La selección del archivo de origen se utiliza para:
   + Seleccione el binario de recursos que actúa como el `source` binario transferido al trabajador de Asset compute
   + Cargar archivos de origen
1. __Definición de perfil o perfiles de asset compute:__ Define el Asset compute de trabajo que se va a ejecutar, incluidos los parámetros: incluido el punto final de la URL del trabajador, el nombre de la representación resultante y cualquier parámetro
1. __Ejecutar:__ El botón Ejecutar ejecuta el perfil de Asset compute tal como se define en el editor de perfiles de configuración de Asset compute
1. __Anular:__ El botón Anular cancela una ejecución iniciada al pulsar el botón Ejecutar
1. __Solicitud/respuesta:__ Proporciona la solicitud y respuesta HTTP al/desde el Asset compute de trabajo que se ejecuta en Adobe I/O Runtime. Esto puede resultar útil para depurar
1. __Registros de activación:__ Los registros que describen la ejecución del trabajador de Asset compute, junto con cualquier error. Esta información también está disponible en la `aio app run` salida estándar
1. __Representaciones:__ Muestra todas las representaciones generadas por la ejecución del Asset compute de trabajo
1. __parámetro de consulta devToolToken:__ El token de la herramienta de desarrollo de Asset compute requiere una `devToolToken` parámetro de consulta para estar presente. Este token se genera automáticamente cada vez que se genera una nueva herramienta de desarrollo

### Ejecutar un trabajador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Pulsación para ejecutar un trabajo de Asset compute en la herramienta de desarrollo (sin audio)_

1. Asegúrese de que la herramienta de desarrollo de Asset compute se inicie desde la raíz del proyecto mediante `aio app run` comando.
1. En la herramienta de desarrollo de Asset compute, cargue o seleccione una [archivo de imagen de ejemplo](../assets/samples/sample-file.jpg)
   + Asegúrese de que el archivo esté seleccionado en la variable __Archivo de origen__ lista desplegable
1. Consulte la __Definición de perfil de asset compute__ área de texto
   + La variable `worker` define la dirección URL del trabajador de Asset compute implementado
   + La variable `name` key define el nombre de la representación que se va a generar
   + Se pueden proporcionar otras claves y valores en este objeto JSON y están disponibles en el programa de trabajo en la sección `rendition.instructions` object
      + Si lo desea, puede agregar valores para `size`, `contrast` y `brightness`:

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

1. Toque . __Ejecutar__ botón
1. La variable __Sección Representaciones__ se rellenará con un marcador de posición de representación
1. Una vez que el trabajador haya terminado, el marcador de posición de representación mostrará la representación generada

Si se realizan cambios en el código de trabajo mientras se ejecuta la herramienta de desarrollo, los cambios se &quot;implementarán en caliente&quot;. La &quot;implementación activa&quot; tarda varios segundos, por lo que debe permitir que la implementación se complete antes de volver a ejecutar el programa de trabajo desde la herramienta de desarrollo.

## Solución de problemas

+ [Sangría YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize limit está configurado en demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [La herramienta de desarrollo no se puede iniciar debido a la falta de private.key](../troubleshooting.md#missing-private-key)
+ [Lista desplegable de archivos de origen incorrecta](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Falta el parámetro de consulta devToolToken o no es válido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [No se pueden quitar los archivos de origen](../troubleshooting.md#unable-to-remove-source-files)
+ [Representación devuelta parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

---
title: Herramienta de desarrollo de cómputo de recursos
description: Asset Compute Development Tool es un mazo de cables web local que permite a los desarrolladores configurar y ejecutar los trabajadores de Asset Computer localmente, fuera del contexto del SDK de AEM con los recursos de Asset Compute en Adobe I/O Runtime.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


# Herramienta de desarrollo de cómputo de recursos

Asset Compute Development Tool es un mazo de cables web local que permite a los desarrolladores configurar y ejecutar los trabajadores de Asset Computer localmente, fuera del contexto del SDK de AEM con los recursos de Asset Compute en Adobe I/O Runtime.

## Ejecución de la herramienta de desarrollo de cómputo de recursos

La Herramienta de desarrollo de cómputo de recursos se puede ejecutar desde la raíz del proyecto de cómputo de recursos mediante el comando terminal:

```
$ aio app run
```

Esto inicio la Herramienta de desarrollo en __http://localhost:9000__ y la abre automáticamente en una ventana del explorador. Para que la herramienta de desarrollo se ejecute, debe proporcionarse [un devToolToken válido y autogenerado mediante un parámetro](#troubleshooting__devtooltoken)de consulta.

## Comprender la interfaz de las herramientas de desarrollo de cómputo de recursos{#interface}

![Herramienta de desarrollo de cómputo de recursos](./assets/development-tool/asset-compute-dev-tool.png)

1. __Archivo de origen:__ La selección del archivo de origen se utiliza para:
   + Se seleccionó el binario de recursos que será el `source` binario pasado al trabajador de cálculo de recursos
   + Carga de archivos de origen
1. __Definición de perfiles de cálculo de recursos:__ Define el trabajador de cómputo de recursos que se va a ejecutar, incluidos los parámetros: incluyendo el punto final de la dirección URL del trabajador, el nombre de la representación resultante y cualquier parámetro
1. __Ejecutar:__ El botón Ejecutar ejecuta el perfil de cálculo de recursos tal como se define en el editor de perfiles de configuración de cálculo de recursos
1. __Anular:__ El botón Anular cancela una ejecución iniciada al tocar el botón Ejecutar
1. __Solicitud/Respuesta:__ Proporciona la solicitud HTTP y la respuesta al/desde el programa de trabajo de cálculo de recursos que se ejecuta en Adobe I/O Runtime. Esto puede resultar útil para la depuración
1. __Registros de activación:__ Registros que describen la ejecución del trabajador de cómputo de recursos, junto con cualquier error. Esta información también está disponible en el `aio app run` estándar
1. __Representaciones:__ Muestra todas las representaciones generadas por la ejecución del trabajador de cálculo de recursos
1. __parámetro de consulta devToolToken:__ El token de la herramienta de desarrollo de cómputo de recursos requiere un parámetro de `devToolToken` consulta válido para estar presente. Este token se genera automáticamente cada vez que se genera una nueva herramienta de desarrollo

### Ejecutar un trabajador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Pulsaciones para ejecutar un trabajo de cómputo de recursos en la herramienta de desarrollo (sin audio)_

1. Asegúrese de que la herramienta de desarrollo de cómputo de recursos se inicie desde la raíz del proyecto mediante el `aio app run` comando .
1. En la herramienta de desarrollo de cómputo de recursos, cargue o seleccione un archivo de imagen de [ejemplo](../assets/samples/sample-file.jpg)
   + Asegúrese de que el archivo esté seleccionado en la lista desplegable __Archivo__ de origen
1. Revisar el área de texto de definición __del perfil de cálculo de__ recursos
   + La `worker` clave define la dirección URL del trabajador de cálculo de recursos implementado
   + La `name` clave define el nombre de la representación que se va a generar
   + Se pueden proporcionar otras claves o valores en este objeto JSON y estarán disponibles en el programa de trabajo bajo el `rendition.instructions` objeto
      + De forma opcional, agregue valores para `size`y `contrast` para `brightness`:

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

1. Tap the __Run__ button
1. La sección ____ Representaciones se rellenará con un marcador de posición de representación
1. Una vez finalizado el trabajo, el marcador de posición de representación mostrará la representación generada

Si se realizan cambios en el código de trabajo mientras se ejecuta la herramienta de desarrollo, se &quot;implementarán en caliente&quot; los cambios. La &quot;implementación en caliente&quot; tarda varios segundos, por lo que debe permitir que la implementación se complete antes de volver a ejecutar el programa de trabajo desde la herramienta de desarrollo.

## Solución de problemas

+ [Sangría de YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [el límite de memorySize está establecido en demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [La herramienta de desarrollo no puede inicio debido a la falta de private.key](../troubleshooting.md#missing-private-key)
+ [Lista desplegable de archivos de origen incorrecta](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Falta el parámetro de consulta devToolToken o no es válido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [No se pueden quitar los archivos de origen](../troubleshooting.md#unable-to-remove-source-files)
+ [Representación devuelta parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

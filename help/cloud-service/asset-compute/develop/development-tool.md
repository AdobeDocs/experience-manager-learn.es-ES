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
duration: 201
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
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

La herramienta de desarrollo se iniciará en __http://localhost:9000__ y, automáticamente, lo abre en una ventana del explorador. Para que se ejecute la herramienta de desarrollo, [se debe proporcionar un devToolToken válido y generado automáticamente mediante un parámetro de consulta](#troubleshooting__devtooltoken).

## Comprensión de la interfaz de Herramientas de desarrollo de Asset compute{#interface}

![Herramienta de desarrollo de asset compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Archivo de origen:__ La selección del archivo de origen se utiliza para:
   + Se seleccionó el binario de recursos que actúa como `source` binario pasado al trabajador de Asset compute
   + Cargar archivos de origen
1. __Definición de perfiles de asset compute:__ Define el trabajador de Asset compute que se ejecutará, incluidos los parámetros: incluido el punto final de la URL del trabajador, el nombre de la representación resultante y cualquier parámetro
1. __Ejecutar:__ El botón Ejecutar ejecuta el perfil de Asset compute tal como se define en el editor de perfiles de configuración de Asset compute
1. __Anular:__ El botón Anular cancela una ejecución iniciada al pulsar el botón Ejecutar
1. __Solicitud/respuesta:__ Proporciona la solicitud y respuesta HTTP hacia/desde el trabajador de Asset compute que se ejecuta en Adobe I/O Runtime. Esto puede resultar útil para la depuración
1. __Registros de activación:__ Los registros que describen la ejecución del trabajador de Asset compute, junto con los errores. Esta información también está disponible en la `aio app run` salida estándar
1. __Representaciones:__ Muestra todas las representaciones generadas por la ejecución del trabajador de Asset compute
1. __Parámetro de consulta devToolToken:__ El token de la herramienta de desarrollo de Assets computes requiere un válido `devToolToken` parámetro de consulta que debe estar presente. Este token se genera automáticamente cada vez que se genera una nueva herramienta de desarrollo

### Ejecutar un trabajador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Pulsación de la ejecución de un trabajo de Asset compute en la herramienta de desarrollo (sin audio)_

1. Asegúrese de que la herramienta de desarrollo de Assets computes se inicia desde la raíz del proyecto utilizando `aio app run` comando.
1. En la herramienta de desarrollo de Assets computes, cargue o seleccione un [archivo de imagen de muestra](../assets/samples/sample-file.jpg)
   + Asegúrese de que el archivo está seleccionado en la __Archivo de origen__ desplegable
1. Revise la __definición del perfil de asset compute__ área de texto
   + El `worker` define la dirección URL del trabajador de Asset compute implementado
   + El `name` key define el nombre de la representación que se va a generar
   + Se pueden proporcionar otras claves o valores en este objeto JSON y están disponibles en el trabajador en la variable `rendition.instructions` objeto
      + Si lo desea, agregue valores para `size`, `contrast` y `brightness`:

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

1. Pulse el botón __Ejecutar__ botón
1. El __Sección Representaciones__ se rellenará con un marcador de lugar de representación
1. Una vez que el trabajador finalice, el marcador de posición de representación mostrará la representación generada

Realizar cambios en el código del trabajador mientras se ejecuta la herramienta de desarrollo hará que los cambios se &quot;implementen&quot;. La &quot;implementación en caliente&quot; tarda varios segundos, por lo que permita que la implementación se complete antes de volver a ejecutar el trabajador desde la herramienta de desarrollo.

## Solución de problemas

+ [Sangría YAML incorrecta](../troubleshooting.md#incorrect-yaml-indentation)
+ [El límite memorySize se ha establecido en un valor demasiado bajo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [No se puede iniciar la herramienta de desarrollo porque falta private.key](../troubleshooting.md#missing-private-key)
+ [Lista desplegable de archivos de origen incorrecta](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parámetro de consulta devToolToken faltante o no válido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [No se pueden eliminar los archivos de origen](../troubleshooting.md#unable-to-remove-source-files)
+ [Representación parcialmente dibujada/dañada](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)

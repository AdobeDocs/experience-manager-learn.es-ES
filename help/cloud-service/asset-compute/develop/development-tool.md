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
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '703'
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
1. __Definición de perfil de cálculo de recursos:__ Define el trabajador de cómputo de recursos que se va a ejecutar, incluidos los parámetros: incluyendo el punto final de la dirección URL del trabajador, el nombre de la representación resultante y cualquier parámetro
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

### Lista desplegable de archivos de origen incorrecta{#troubleshooting__dev-tool-application-cache}

La herramienta de desarrollo de cómputo de recursos puede introducir un estado en el que extrae datos antiguos y se nota más en la lista desplegable de archivos __de__ origen que muestra elementos incorrectos.

+ __Error:__ La lista desplegable de archivos de origen muestra elementos incorrectos.
+ __Causa:__ El estado del explorador en caché antiguo hace que
+ __Resolución:__ En el navegador, borre por completo el &quot;estado de la aplicación&quot; de la ficha del navegador, la caché del navegador, el almacenamiento local y el programa de trabajo del servicio.

### Falta el parámetro de consulta devToolToken o no es válido{#troubleshooting__devtooltoken}

+ __Error:__ Notificación &quot;no autorizada&quot; en Asset Compute Development Tool
+ __Causa:__ `devToolToken` falta o no es válido
+ __Resolución:__ Cierre la ventana del navegador de la herramienta de desarrollo de cómputo de recursos, finalice los procesos de la herramienta de desarrollo que se ejecuten iniciados mediante el `aio app run` comando y vuelva a utilizar la herramienta de desarrollo de inicios (mediante `aio app run`).

### No se pueden quitar los archivos de origen{#troubleshooting__remove-source-files}

+ __Error:__ No hay forma de eliminar los archivos de origen agregados de la interfaz de usuario de las herramientas de desarrollo
+ __Causa:__ Esta funcionalidad no se ha implementado
+ __Resolución:__ Inicie sesión en el proveedor de almacenamiento de nube con las credenciales definidas en `.env`. Busque el contenedor que utilizan las herramientas de desarrollo (también especificadas en `.env`), desplácese hasta la carpeta de __origen__ y elimine las imágenes de origen. Es posible que tenga que realizar los pasos descritos en la lista desplegable Archivos [de origen incorrectos](#troubleshooting__dev-tool-application-cache) si los archivos de origen eliminados siguen mostrándose en la lista desplegable, ya que se pueden almacenar en caché localmente en el &quot;estado de la aplicación&quot; de las herramientas de desarrollo.

   ![Almacenamiento de blob de Microsoft Azure](./assets/development-tool/troubleshooting__remove-source-files.png)

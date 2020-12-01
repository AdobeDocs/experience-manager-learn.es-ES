---
title: Configurar las variables de entorno para la extensibilidad de la Asset compute
description: Las variables de entorno se mantienen en el archivo .env para el desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento de nube necesarias para el desarrollo local.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---


# Configuración de las variables de entorno

![archivo dot env](assets/environment-variables/dot-env-file.png)

Antes de comenzar el desarrollo de los trabajadores de Asset compute, asegúrese de que el proyecto está configurado con información de Adobe I/O y de almacenamiento en la nube. Esta información se almacena en el `.env` del proyecto, que se utiliza solamente para el desarrollo local, y no se guarda en Git. El archivo `.env` proporciona una manera conveniente de exponer pares de clave/valores al entorno de desarrollo local de la Asset compute local. Cuando [implementa](../deploy/runtime.md) trabajadores de Asset compute en Adobe I/O Runtime, el archivo `.env` no se utiliza, sino que se pasa un subconjunto de valores a través de variables de entorno. También se pueden almacenar otros parámetros personalizados y secretos en el archivo `.env`, como credenciales de desarrollo para servicios Web de terceros.

## Haga referencia a `private.key`

![clave privada](assets/environment-variables/private-key.png)

Abra el archivo `.env`, quite el comentario de la clave `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` y proporcione la ruta absoluta en el sistema de archivos al `private.key` que se empareja con el certificado público agregado al proyecto Adobe I/O FireFly.

+ Si el par de claves fue generado por Adobe I/O, se descargó automáticamente como parte del `config.zip`.
+ Si proporcionaste la clave pública a Adobe I/O, entonces también debes estar en posesión de la clave privada coincidente.
+ Si no tiene estos pares de claves, puede generar nuevos pares de claves o cargar nuevas claves públicas en la parte inferior de:
   [https://console.adobe.com](https://console.adobe.io) > Proyecto de búsqueda de su Asset compute > Workspaces @ Development > Cuenta de servicio (JWT).

Recuerde que el archivo `private.key` no debe registrarse en Git ya que contiene secretos, sino que debe almacenarse en un lugar seguro fuera del proyecto.

Por ejemplo, en macOS este aspecto podría ser el siguiente:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configuración de las credenciales de Almacenamiento de Cloud

El desarrollo local de los trabajadores de Asset compute requiere acceso a [almacenamiento de nube](../set-up/accounts-and-services.md#cloud-storage). Las credenciales del almacenamiento de nube utilizadas para el desarrollo local se proporcionan en el archivo `.env`.

Este tutorial prefiere el uso de Almacenamiento de blob de Azure, pero Amazon S3 y sus claves correspondientes en el archivo `.env` se pueden usar en su lugar.

### Uso del Almacenamiento de blob de Azure

Quite el comentario y rellene las siguientes claves en el archivo `.env` y llénelas con los valores del almacenamiento de nube aprovisionado que se encuentra en Azure Portal.

![Almacenamiento de blob de Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valor de la clave `AZURE_STORAGE_CONTAINER_NAME`
1. Valor de la clave `AZURE_STORAGE_ACCOUNT`
1. Valor de la clave `AZURE_STORAGE_KEY`

Por ejemplo, esto puede tener el siguiente aspecto (solo valores para ilustración):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

El archivo resultante `.env` tiene el siguiente aspecto:

![Credenciales de Almacenamiento de blob de Azure](assets/environment-variables/cloud-storage-credentials.png)

Si NO utiliza el Almacenamiento Blob de Microsoft Azure, elimine o deje estos comentarios (añadiendo el prefijo `#`).

### Uso de Amazon S3 cloud almacenamiento{#amazon-s3}

Si utiliza Amazon S3 cloud almacenamiento sin comentarios y rellena las siguientes claves en el archivo `.env`.

Por ejemplo, esto puede tener el siguiente aspecto (solo valores para ilustración):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validación de la configuración del proyecto

Una vez configurado el proyecto de Asset compute generado, valide la configuración antes de realizar cambios en el código para asegurarse de que se aprovisionan los servicios de soporte, en los archivos `.env`.

A la herramienta de desarrollo de Assets computes de inicio para el proyecto de Asset compute:

1. Abra una línea de comandos en la raíz del proyecto de Asset compute (en el código VS se puede abrir directamente en el IDE mediante Terminal > Nuevo terminal) y ejecute el comando:

   ```
   $ aio app run
   ```

1. La herramienta de desarrollo de Assets computes local se abrirá en el explorador Web predeterminado en __http://localhost:9000__.

   ![ejecución de la aplicación de AIO](assets/environment-variables/aio-app-run.png)

1. Observe el resultado de la línea de comandos y el explorador Web para ver los mensajes de error a medida que se inicializa la herramienta de desarrollo.
1. Para detener la herramienta de desarrollo de Assets computes, toque `Ctrl-C` en la ventana que ejecutó `aio app run` para finalizar el proceso.

## Solución de problemas

+ [La herramienta de desarrollo no puede inicio debido a la falta de private.key](../troubleshooting.md#missing-private-key)

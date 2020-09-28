---
title: Configuración de las variables de entorno para la extensibilidad del cálculo de recursos
description: Las variables de entorno se mantienen en el archivo .env para el desarrollo local y se utilizan para proporcionar las credenciales de E/S de Adobe y las credenciales de almacenamiento de nube necesarias para el desarrollo local.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 0%

---


# Configuración de las variables de entorno

![archivo dot env](assets/environment-variables/dot-env-file.png)

Antes de comenzar el desarrollo de los trabajadores de Asset Compute, asegúrese de que el proyecto está configurado con información de E/S de Adobe y almacenamiento en la nube. Esta información se almacena en el proyecto `.env` que se utiliza solamente para el desarrollo local, y no se guarda en Git. El `.env` archivo proporciona una manera práctica de exponer pares de clave/valores al entorno de desarrollo local de Asset Compute. Al [implementar](../deploy/runtime.md) los trabajadores de cálculo de recursos en Adobe I/O Runtime, no se utiliza el `.env` archivo, sino que se pasa un subconjunto de valores a través de variables de entorno. También se pueden almacenar otros parámetros personalizados y secretos en el `.env` archivo, como credenciales de desarrollo para servicios Web de terceros.

## Consulte la `private.key`

![clave privada](assets/environment-variables/private-key.png)

Abra el `.env` archivo, quite el comentario de la `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` clave y proporcione la ruta absoluta en el sistema de archivos al `private.key` que coincida con el certificado público agregado al proyecto FireFly de E/S de Adobe.

+ Si el par de claves se generó mediante E/S de Adobe, se descargó automáticamente como parte del `config.zip`.
+ Si ha proporcionado la clave pública para la E/S de Adobe, también debe estar en posesión de la clave privada correspondiente.
+ Si no tiene estos pares de claves, puede generar nuevos pares de claves o cargar nuevas claves públicas en la parte inferior de:
   [https://console.adobe.com](https://console.adobe.io) > Proyecto de la luciérnaga de cómputo de recursos > Espacios de trabajo @ Desarrollo > Cuenta de servicio (JWT).

Recuerde que el `private.key` archivo no debe registrarse en Git ya que contiene secretos, sino que debe almacenarse en un lugar seguro fuera del proyecto.

Por ejemplo, en macOS este aspecto podría ser el siguiente:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configuración de las credenciales de Almacenamiento de Cloud

El desarrollo local de los trabajadores de Asset Compute requiere acceso al almacenamiento [de](../set-up/accounts-and-services.md#cloud-storage)nube. Las credenciales de almacenamiento de nube utilizadas para el desarrollo local se proporcionan en el `.env` archivo.

Este tutorial prefiere el uso de Almacenamiento de blob de Azure, pero en su lugar se puede utilizar Amazon S3 y sus claves correspondientes en el `.env` archivo.

### Uso del Almacenamiento de blob de Azure

Quite el comentario y rellene las siguientes claves del `.env` archivo, y llénelas con los valores del almacenamiento de nube aprovisionado que se encuentra en Azure Portal.

![Almacenamiento de blob de Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valor de la `AZURE_STORAGE_CONTAINER_NAME` clave
1. Valor de la `AZURE_STORAGE_ACCOUNT` clave
1. Valor de la `AZURE_STORAGE_KEY` clave

Por ejemplo, esto puede tener el siguiente aspecto (solo valores para ilustración):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

El `.env` archivo resultante tiene el siguiente aspecto:

![Credenciales de Almacenamiento de blob de Azure](assets/environment-variables/cloud-storage-credentials.png)

Si NO utiliza el Almacenamiento Blob de Microsoft Azure, elimine o deje estos comentarios (añadiendo un prefijo `#`).

### Uso del almacenamiento de nube Amazon S3{#amazon-s3}

Si utiliza Amazon S3 cloud almacenamiento sin comentarios y rellena las siguientes claves en el `.env` archivo.

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

Una vez configurado el proyecto de cálculo de recursos generado, valide la configuración antes de realizar cambios de código para asegurarse de que se aprovisionan los servicios de soporte en los `.env` archivos.

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

### Las herramientas de desarrollo local de cálculo de recursos no pueden inicio debido a la falta de private.key

+ __Error:__ Error del servidor de desarrollo local: Faltan los archivos necesarios en validatePrivateKeyFile.... (a través de la salida estándar del `aio app run` comando)
+ __Causa:__ El `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor del `.env` archivo no apunta a `private.key` o no `private.key` es legible por el usuario actual.
+ __Resolución:__ Revise el `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor del `.env` archivo y asegúrese de que contiene la ruta completa y absoluta al `private.key` archivo del sistema de archivos.

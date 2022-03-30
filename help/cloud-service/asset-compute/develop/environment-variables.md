---
title: Configuración de las variables de entorno para la extensibilidad del Asset compute
description: Las variables de entorno se mantienen en el archivo .env para el desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento en la nube necesarias para el desarrollo local.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Configuración de las variables de entorno

![archivo env. punto](assets/environment-variables/dot-env-file.png)

Antes de comenzar el desarrollo de los trabajadores de Asset compute, asegúrese de que el proyecto esté configurado con información de Adobe I/O y almacenamiento en la nube. Esta información se almacena en el `.env`  que solo se utiliza para el desarrollo local y no se guarda en Git. La variable `.env` proporciona una forma cómoda de exponer pares de claves y valores al entorno de desarrollo local de Asset compute local. When [implementación](../deploy/runtime.md) asset compute de los trabajadores a Adobe I/O Runtime, `.env` no se utiliza, sino que se pasa un subconjunto de valores a través de variables de entorno. Otros parámetros personalizados y secretos se pueden almacenar en la variable `.env` también, como credenciales de desarrollo para servicios web de terceros.

## Consulte la `private.key`

![clave privada](assets/environment-variables/private-key.png)

Abra el `.env` , quite la marca de comentario del archivo `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` y proporcione la ruta absoluta de su sistema de archivos al `private.key` que se asocia con el certificado público agregado al proyecto de Adobe I/O App Builder.

+ Si el Adobe I/O generó el par de claves, se descargó automáticamente como parte del  `config.zip`.
+ Si ha proporcionado la clave pública al Adobe I/O, también debe tener la clave privada correspondiente.
+ Si no tiene estos pares de claves, puede generar nuevos pares de claves o cargar nuevas claves públicas en la parte inferior de:
   [https://console.adobe.com](https://console.adobe.io) > Su proyecto de Asset compute App Builder > Espacios de trabajo en desarrollo > Cuenta de servicio (JWT).

Recuerde `private.key` no debe registrarse en Git porque contiene secretos, sino que debe almacenarse en un lugar seguro fuera del proyecto.

Por ejemplo, en macOS esto puede tener el siguiente aspecto:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configuración de las credenciales de Cloud Storage

El desarrollo local de los Assets computes requiere acceso a [almacenamiento en la nube](../set-up/accounts-and-services.md#cloud-storage). Las credenciales de almacenamiento en la nube utilizadas para el desarrollo local se proporcionan en la variable `.env` archivo.

Este tutorial prefiere el uso de Azure Blob Storage, aunque Amazon S3 y sus claves correspondientes en el `.env` se puede usar en su lugar.

### Uso del almacenamiento del blob de Azure

Descomente y rellene las siguientes claves en la sección `.env` y rellénelas con los valores del almacenamiento en la nube aprovisionado que se encuentra en Azure Portal.

![Almacenamiento de Azure Blob](./assets/environment-variables/azure-portal-credentials.png)

1. Valor para la variable `AZURE_STORAGE_CONTAINER_NAME` key
1. Valor para la variable `AZURE_STORAGE_ACCOUNT` key
1. Valor para la variable `AZURE_STORAGE_KEY` key

Por ejemplo, puede tener el siguiente aspecto (solo valores para ilustración):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

El resultado `.env` tiene el siguiente aspecto:

![Credenciales de almacenamiento de Azure Blob](assets/environment-variables/cloud-storage-credentials.png)

Si NO utiliza el almacenamiento de blob de Microsoft Azure, elimine o deje estos comentarios (prefiriendo con `#`).

### Uso del almacenamiento en la nube Amazon S3{#amazon-s3}

Si utiliza Amazon S3 cloud storage uncomment y rellena las claves siguientes en la `.env` archivo.

Por ejemplo, puede tener el siguiente aspecto (solo valores para ilustración):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validación de la configuración del proyecto

Una vez configurado el proyecto de Asset compute generado, valide la configuración antes de realizar cambios en el código para asegurarse de que se proporcionan los servicios de soporte, en la variable `.env` archivos.

Para iniciar la herramienta de desarrollo de Asset compute para el proyecto de Asset compute:

1. Abra una línea de comandos en la raíz del proyecto de Asset compute (en el código VS esto se puede abrir directamente en el IDE a través de Terminal > Nuevo terminal) y ejecute el comando:

   ```
   $ aio app run
   ```

1. La herramienta de desarrollo de Assets computes local se abrirá en el explorador web predeterminado en __http://localhost:9000__.

   ![ejecución de aplicación de aio](assets/environment-variables/aio-app-run.png)

1. Vea los mensajes de error en la salida de la línea de comandos y en el explorador web a medida que se inicializa la herramienta de desarrollo.
1. Para detener la herramienta de desarrollo de Asset compute, pulse `Ctrl-C` en la ventana que se ha ejecutado `aio app run` para finalizar el proceso.

## Solución de problemas

+ [La herramienta de desarrollo no se puede iniciar debido a la falta de private.key](../troubleshooting.md#missing-private-key)

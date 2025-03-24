---
title: Configuración de las variables de entorno para la extensibilidad de Asset Compute
description: Las variables de entorno se mantienen en el archivo .env para el desarrollo local y se utilizan para proporcionar las credenciales de Adobe I/O y las credenciales de almacenamiento en la nube necesarias para el desarrollo local.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 121
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Configuración de las variables de entorno

![archivo dot env](assets/environment-variables/dot-env-file.png)

Antes de comenzar el desarrollo de los trabajadores de Asset Compute, asegúrese de que el proyecto esté configurado con información de Adobe I/O y almacenamiento en la nube. Esta información se almacena en `.env` del proyecto, que se usa solamente para el desarrollo local y no se guarda en Git. El archivo `.env` proporciona una forma cómoda de exponer pares de clave/valores al entorno de desarrollo local de Asset Compute. Al [implementar](../deploy/runtime.md) trabajadores de Asset Compute en Adobe I/O Runtime, no se usa el archivo `.env`, sino que se pasa un subconjunto de valores a través de variables de entorno. Otros parámetros y secretos personalizados también se pueden almacenar en el archivo `.env`, como las credenciales de desarrollo para servicios web de terceros.

## Hacer referencia a `private.key`

![clave privada](assets/environment-variables/private-key.png)

Abra el archivo `.env`, quite el comentario de la clave `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` y proporcione la ruta de acceso absoluta en su sistema de archivos a `private.key`, que está emparejado con el certificado público agregado a su proyecto de Adobe I/O App Builder.

+ Si el par de claves lo generó Adobe I/O, se descargó automáticamente como parte de `config.zip`.
+ Si ha proporcionado la clave pública a Adobe I/O, también debe tener la clave privada correspondiente.
+ Si no tiene estos pares de claves, puede generar nuevos pares de claves o cargar nuevas claves públicas en la parte inferior de:
  [https://console.adobe.com](https://console.adobe.io) > Su proyecto de Asset Compute App Builder > Espacios de trabajo en Desarrollo > Cuenta de servicio (JWT).

Recuerde que el archivo `private.key` no se debe proteger en Git porque contiene secretos, sino que se debe almacenar en un lugar seguro fuera del proyecto.

Por ejemplo, en macOS esto podría tener el siguiente aspecto:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurar credenciales de almacenamiento en la nube

El desarrollo local de los trabajadores de Asset Compute requiere acceso a [almacenamiento en la nube](../set-up/accounts-and-services.md#cloud-storage). Las credenciales de almacenamiento en la nube utilizadas para el desarrollo local se proporcionan en el archivo `.env`.

Este tutorial prefiere el uso del almacenamiento de Azure Blob, pero se puede utilizar Amazon S3 y sus claves correspondientes en el archivo `.env` en su lugar.

### Usar Azure Blob Storage

Elimine los comentarios y rellene las siguientes claves en el archivo `.env`, y rellénelas con los valores del almacenamiento en la nube aprovisionado que se encuentra en Azure Portal.

![Almacenamiento de blob de Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valor de la clave `AZURE_STORAGE_CONTAINER_NAME`
1. Valor de la clave `AZURE_STORAGE_ACCOUNT`
1. Valor de la clave `AZURE_STORAGE_KEY`

Por ejemplo, esto podría tener el siguiente aspecto (valores solo para ilustración):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

El archivo `.env` resultante tendrá el siguiente aspecto:

![Credenciales de almacenamiento de Azure Blob](assets/environment-variables/cloud-storage-credentials.png)

Si NO usa el almacenamiento del blob de Microsoft Azure, elimine o deje estos elementos comentados (prefiriendo `#`).

### Uso del almacenamiento en la nube de Amazon S3{#amazon-s3}

Si usa el almacenamiento en la nube de Amazon S3, quite el comentario y rellene las siguientes claves en el archivo `.env`.

Por ejemplo, esto podría tener el siguiente aspecto (valores solo para ilustración):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validación de la configuración del proyecto

Una vez configurado el proyecto de Asset Compute generado, valide la configuración antes de realizar cambios en el código para asegurarse de que se proporcionan los servicios de soporte en los archivos `.env`.

Para iniciar la herramienta de desarrollo de Asset Compute para el proyecto de Asset Compute:

1. Abra una línea de comandos en la raíz del proyecto de Asset Compute (en el código VS, esto se puede abrir directamente en el IDE a través de Terminal > Nuevo terminal) y ejecute el comando:

   ```
   $ aio app run
   ```

1. La herramienta de desarrollo de Asset Compute local se abrirá en el explorador web predeterminado en __http://localhost:9000__.

   ![ejecución de aplicación aio](assets/environment-variables/aio-app-run.png)

1. Observe la salida de la línea de comandos y el explorador Web en busca de mensajes de error cuando se inicializa la herramienta de desarrollo.
1. Para detener la herramienta de desarrollo de Asset Compute, pulse `Ctrl-C` en la ventana que ejecutó `aio app run` para finalizar el proceso.

## Solución de problemas

+ [No se puede iniciar la herramienta de desarrollo porque falta private.key](../troubleshooting.md#missing-private-key)

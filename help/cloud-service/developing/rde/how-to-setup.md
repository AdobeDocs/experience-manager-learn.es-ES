---
title: Cómo configurar el entorno de desarrollo rápido
description: AEM Aprenda a configurar el entorno de desarrollo rápido para el as a Cloud Service de la.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 505
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 3%

---

# Cómo configurar el entorno de desarrollo rápido

Aprender **cómo realizar la configuración** AEM Entorno de Desarrollo Rápido (RDE) en as a Cloud Service.

Este vídeo muestra:

- Agregar un RDE al programa mediante Cloud Manager
- AEM Flujo de inicio de sesión RDE con Adobe IMS, de qué manera es similar a cualquier otro entorno as a Cloud Service de la
- Configuración de [CLI extensible de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) también conocido como `aio CLI`
- AEM Instalación y configuración de RDE y Cloud Manager de la `aio CLI` plugin

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Requisitos previos

Lo siguiente debe instalarse de manera local:

- [Node.js](https://nodejs.org/en/) (LTS - Soporte a largo plazo)
- [npm 8+](https://docs.npmjs.com/)

## Configuración local

Para implementar la variable [Proyectos de sitios WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) Para enviar código y contenido al RDE desde su equipo local, complete los siguientes pasos.

### CLI extensible de Adobe I/O Runtime

Instale Adobe I/O Runtime Extensible CLI, también conocida como `aio CLI` ejecutando el siguiente comando desde la línea de comandos.

```shell
$ npm install -g @adobe/aio-cli
```

### AEM complementos de

AEM Instale los complementos de Cloud Manager y RDE de la mediante el `aio cli`de `plugins:install` comando.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

El complemento Cloud Manager permite a los desarrolladores interactuar con Cloud Manager desde la línea de comandos.

AEM El complemento de RDE, permite a los desarrolladores implementar código y contenido desde el equipo local.

Además, para actualizar los complementos, utilice el `aio plugins:update` comando.

## AEM Configuración de complementos de

AEM Los complementos de la deben configurarse para interactuar con su RDE. En primer lugar, con la interfaz de usuario de Cloud Manager, copie los valores de la organización, el programa y el ID de entorno.

1. ID de organización: copie el valor de **Imagen de perfil > Información de cuenta (interna) > Ventana modal > ID de organización actual**

   ![ID de organización](./assets/Org-ID.png)

1. ID de programa: copie el valor de **Información general del programa > Entornos > {ProgramName}-rojo > URI del explorador > números entre `program/` y`/environment`**

1. ID de entorno: copie el valor de **Información general del programa > Entornos > {ProgramName}-red > URI del explorador > números después de`environment/`**

   ![ID de programa y entorno](./assets/Program-Environment-Id.png)

1. A continuación, utilizando `aio cli`de `config:set` para establecer estos valores, ejecute el siguiente comando.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

Puede comprobar los valores de configuración actuales ejecutando el siguiente comando.

```shell
$ aio config:list
```

Además, para cambiar o saber en qué organización ha iniciado sesión actualmente, puede utilizar el siguiente comando.

```shell
$ aio where
```

## Verificar acceso de RDE

AEM Compruebe la instalación y configuración del complemento RDE de la ejecutando el siguiente comando.

```shell
$ aio aem:rde:status
```

La información de estado de RDE se muestra como el estado del entorno, la lista de _AEM su proyecto de_ paquetes y configuraciones en el servicio de creación y publicación.

## Siguiente paso

Aprender [cómo usar](./how-to-use.md) Cree un RDE para implementar código y contenido desde su entorno de desarrollo integrado (IDE) favorito para ciclos de desarrollo más rápidos.


## Recursos adicionales

[Habilitar RDE en la documentación de un programa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Configuración de [CLI extensible de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) también conocido como `aio CLI`

[Uso y comandos de CLI de AIO](https://github.com/adobe/aio-cli#usage)

[Complemento de CLI de Adobe I/O Runtime AEM para interacciones con entornos de desarrollo rápido de](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Complemento CLI de Cloud Manager AIO](https://github.com/adobe/aio-cli-plugin-cloudmanager)

---
title: Configuración del entorno de desarrollo rápido
description: Aprenda a configurar Rapid Development Environment para AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 4%

---


# Configuración del entorno de desarrollo rápido

Más información **configuración** Entorno de desarrollo rápido (RDE) en AEM as a Cloud Service.

Este vídeo muestra:

- Adición de un RDE a su programa mediante Cloud Manager
- Flujo de inicio de sesión de RDE mediante Adobe IMS, cómo se parece a cualquier otro entorno as a Cloud Service AEM
- Configuración de [CLI extensible de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) también se conoce como `aio CLI`
- Configuración y configuración de AEM RDE y Cloud Manager `aio CLI` plugin

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Requisitos previos

Lo siguiente debe instalarse de manera local:

- [Node.js](https://nodejs.org/en/) (LTS: compatibilidad a largo plazo)
- [npm 8+](https://docs.npmjs.com/)

## Configuración local

Para implementar el [El proyecto WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) código y contenido en el RDE desde el equipo local, complete los siguientes pasos.

### CLI extensible de Adobe I/O Runtime

Instale la CLI extensible de Adobe I/O Runtime, también conocida como `aio CLI` ejecutando el siguiente comando desde la línea de comandos.

```shell
$ npm install -g @adobe/aio-cli
```

### AEM complementos

Instalación de Cloud Manager y AEM complementos RDE mediante el uso de `aio cli`&#39;s `plugins:install` comando.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

El complemento de Cloud Manager permite a los desarrolladores interactuar con Cloud Manager desde la línea de comandos.

El complemento AEM RDE permite a los desarrolladores implementar código y contenido desde el equipo local.

Además, para actualizar los complementos, use el `aio plugins:update` comando.

## Configurar AEM complementos

Los complementos AEM deben configurarse para interactuar con su RDE. En primer lugar, con la interfaz de usuario de Cloud Manager, copie los valores del ID de organización, programa y entorno.

1. ID de organización: Copiar el valor de **Imagen de perfil > Información de la cuenta (interna) > Ventana modal > ID de organización actual**

   ![Id. de organización](./assets/Org-ID.png)

1. ID del programa: Copiar el valor de **Descripción general del programa > Entornos > {Nombre del programa}-rde > URI del explorador > números entre `program/` y`/environment`**

1. ID de entorno: Copiar el valor de **Información general del programa > Entornos > {Nombre del programa}-rde > URI del explorador > números después de`environment/`**

   ![ID de programa y entorno](./assets/Program-Environment-Id.png)

1. A continuación, utilizando la variable `aio cli`&#39;s `config:set` configure estos valores ejecutando el siguiente comando.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

Puede verificar los valores de configuración actuales ejecutando el siguiente comando.

```shell
$ aio config:list
```

Además, para cambiar o saber a qué organización ha iniciado sesión, puede utilizar el siguiente comando.

```shell
$ aio where
```

## Verifique el acceso RDE

Compruebe la instalación y configuración AEM del complemento RDE ejecutando el siguiente comando.

```shell
$ aio aem:rde:status
```

La información de estado de RDE se muestra como el estado del entorno, la lista de _su proyecto AEM_ paquetes y configuraciones en el servicio de creación y publicación.

## Siguiente paso

Más información [cómo usar](./how-to-use.md) un RDE para implementar código y contenido de su entorno de desarrollo integrado (IDE) favorito para ciclos de desarrollo más rápidos.


## Recursos adicionales

[Habilitar RDE en una documentación de programa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Configuración de [CLI extensible de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) también se conoce como `aio CLI`

[Uso y comandos de la CLI de AIO](https://github.com/adobe/aio-cli#usage)

[Complemento CLI de Adobe I/O Runtime para interacciones con entornos de desarrollo rápido AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Complemento CLI de AIO de Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)

---
title: Cómo configurar el entorno de desarrollo rápido
description: Aprenda a configurar el entorno de desarrollo rápido para AEM as a Cloud Service.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 2%

---

# Cómo configurar el entorno de desarrollo rápido

Obtenga información sobre **cómo configurar** el entorno de desarrollo rápido (RDE) en AEM as a Cloud Service.

Este vídeo muestra:

- Agregar un RDE al programa mediante Cloud Manager
- Flujo de inicio de sesión RDE con Adobe IMS, por qué es similar a cualquier otro entorno de AEM as a Cloud Service
- Configuración de [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) también conocida como `aio CLI`
- Instalación y configuración del complemento AEM RDE y Cloud Manager `aio CLI` mediante el modo no interactivo. Para ver el modo interactivo, consulte las [instrucciones de configuración](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Requisitos previos

Lo siguiente debe instalarse de manera local:

- [Node.js](https://nodejs.org/en/) (LTS - Soporte a largo plazo)
- [npm 8+](https://docs.npmjs.com/)

## Configuración local

Para implementar el código y el contenido [WKND Sites Project&#39;s](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) en RDE desde el equipo local, complete los siguientes pasos.

### CLI extensible de Adobe I/O Runtime

Instale la CLI extensible de Adobe I/O Runtime, también conocida como `aio CLI` ejecutando el siguiente comando desde la línea de comandos.

```shell
$ npm install -g @adobe/aio-cli
```

### Instalación y configuración de complementos de CLI de aio

La CLI de aio debe tener complementos instalados y configurados con el ID de entorno de organización, programa y RDE para interactuar con su RDE. La configuración se puede realizar a través de la CLI de aio utilizando el modo interactivo más sencillo o el modo no interactivo.

>[!BEGINTABS]

>[!TAB Modo interactivo]

Instale y configure los complementos de AEM RDE mediante el comando `plugins:install` de `aio cli`.

1. Instale el complemento AEM RDE de la CLI de aio mediante el comando `plugins:install` de `aio cli`.

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   El complemento AEM RDE permite a los desarrolladores implementar código y contenido desde el equipo local.

2. Inicie sesión en la CLI extensible de Adobe I/O Runtime ejecutando el siguiente comando para obtener el token de acceso. Asegúrese de iniciar sesión en la misma organización de Adobe que su Cloud Manager.

   ```shell
   $ aio login
   ```

3. Ejecute el siguiente comando para configurar RDE mediante el modo interactivo.

   ```shell
   $ aio aem:rde:setup
   ```

4. La CLI le solicita que introduzca el ID de organización, el ID de programa y el ID de entorno.

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - Elija __No__ si solo está trabajando con un único RDE y desea almacenar la configuración de RDE globalmente en su equipo local.

   - Elija __Sí__ si está trabajando con varios RDE, o si desea almacenar la configuración de RDE localmente, en el archivo `.aio` de la carpeta actual, para cada proyecto.

5. Seleccione el ID de organización, el ID de programa y el ID de entorno de RDE de la lista de opciones disponibles.

6. Compruebe que la organización, el programa y el entorno correctos están configurados ejecutando el siguiente comando.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB Modo no interactivo]

Instale y configure los complementos de Cloud Manager y AEM RDE mediante el comando `plugins:install` de `aio cli`.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

El complemento de Cloud Manager permite a los desarrolladores interactuar con Cloud Manager desde la línea de comandos.

El complemento AEM RDE permite a los desarrolladores implementar código y contenido desde el equipo local.

Los complementos de CLI de aio deben configurarse para interactuar con su RDE.

1. En primer lugar, con Cloud Manager, copie los valores de la organización, el programa y el ID de entorno.

   - ID de organización: copie el valor de **Imagen de perfil > Información de cuenta(interna) > Ventana modal > ID de organización actual**

   ![ID de organización](./assets/Org-ID.png)

   - ID de programa: copie el valor de **Información general del programa > Entornos > {ProgramName}-red > URI del explorador > números entre `program/` y`/environment`**

   ![Id. de programa y entorno](./assets/Program-Environment-Id.png)

   - ID de entorno: copie el valor de **Información general del programa > Entornos > {ProgramName}-red > URI del explorador > números después de`environment/`**

   ![Id. de programa y entorno](./assets/Program-Environment-Id.png)

1. Use el comando `config:set` de `aio cli` para establecer estos valores ejecutando el siguiente comando.

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. Compruebe los valores de configuración actuales ejecutando el siguiente comando.

   ```shell
   $ aio config:list
   ```

1. Cambiar o revisar en qué organización ha iniciado sesión actualmente:

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## Verificar acceso de RDE

Compruebe la instalación y configuración del complemento AEM RDE ejecutando el siguiente comando.

```shell
$ aio aem:rde:status
```

La información de estado de RDE se muestra como el estado del entorno, la lista de _sus paquetes del proyecto AEM_ y las configuraciones en el servicio de creación y publicación.

## Siguiente paso

Aprenda [a usar](./how-to-use.md) un RDE para implementar código y contenido desde su entorno de desarrollo integrado (IDE) favorito para ciclos de desarrollo más rápidos.


## Recursos adicionales

[Habilitando RDE en la documentación de un programa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html?lang=es#enabling-rde-in-a-program)

Configuración de [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) también conocida como `aio CLI`

[uso y comandos de CLI de aio](https://github.com/adobe/aio-cli#usage)

[Complemento CLI de Adobe I/O Runtime para interacciones con entornos de desarrollo rápido de AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Complemento Cloud Manager aio CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)

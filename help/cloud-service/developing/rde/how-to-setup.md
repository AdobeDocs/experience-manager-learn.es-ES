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
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: f714adaa9bb637c0c7b17837c1d4b9f2be737c5c
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 2%

---

# Cómo configurar el entorno de desarrollo rápido

Aprender **cómo realizar la configuración** AEM Entorno de Desarrollo Rápido (RDE) en as a Cloud Service.

Este vídeo muestra:

- Agregar un RDE al programa mediante Cloud Manager
- AEM Flujo de inicio de sesión RDE con Adobe IMS, de qué manera es similar a cualquier otro entorno as a Cloud Service de la
- Configuración de [CLI extensible de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) también conocido como `aio CLI`
- AEM Instalación y configuración de RDE y Cloud Manager de la `aio CLI` mediante el modo no interactivo. Para ver el modo interactivo, consulte la [instrucciones de configuración](#setup-the-aem-rde-plugin)

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

### Instalación y configuración de complementos de CLI de aio

La CLI de aio debe tener complementos instalados y configurados con el ID de entorno de organización, programa y RDE para interactuar con su RDE. La configuración se puede realizar a través de la CLI de aio utilizando el modo interactivo más sencillo o el modo no interactivo.

>[!BEGINTABS]

>[!TAB Modo interactivo]

AEM Instale y configure los complementos de RDE de la mediante el `aio cli`de `plugins:install` comando.

1. AEM Instale el complemento RDE de la CLI de aio usando el complemento RDE de la interfaz de usuario `aio cli`de `plugins:install` comando.

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   AEM El complemento de RDE, permite a los desarrolladores implementar código y contenido desde el equipo local.

2. Inicie sesión en la CLI extensible de Adobe I/O Runtime ejecutando el siguiente comando para obtener el token de acceso. Asegúrese de iniciar sesión en la misma organización de Adobe que Cloud Manager.

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

   - Elegir __No__  si sólo está trabajando con un único RDE y desea almacenar la configuración de RDE globalmente en el equipo local.

   - Elegir __Sí__ si está trabajando con varios RDE, o desea almacenar su configuración de RDE localmente, en la carpeta actual `.aio` archivo, para cada proyecto.

5. Seleccione el ID de organización, el ID de programa y el ID de entorno de RDE de la lista de opciones disponibles.

6. Compruebe que la organización, el programa y el entorno correctos están configurados ejecutando el siguiente comando.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB Modo no interactivo]

AEM Instale y configure los complementos de Cloud Manager y RDE mediante el uso de la variable `aio cli`de `plugins:install` comando.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

El complemento Cloud Manager permite a los desarrolladores interactuar con Cloud Manager desde la línea de comandos.

AEM El complemento de RDE, permite a los desarrolladores implementar código y contenido desde el equipo local.

Los complementos de CLI de aio deben configurarse para interactuar con su RDE.

1. En primer lugar, con Cloud Manager, copie los valores de la organización, el programa y el ID de entorno.

   - ID de organización: copie el valor de **Imagen de perfil > Información de cuenta (interna) > Ventana modal > ID de organización actual**

   ![ID de organización](./assets/Org-ID.png)

   - ID de programa: copie el valor de **Información general del programa > Entornos > {ProgramName}-rojo > URI del explorador > números entre `program/` y`/environment`**

   ![ID de programa y entorno](./assets/Program-Environment-Id.png)

   - ID de entorno: copie el valor de **Información general del programa > Entornos > {ProgramName}-red > URI del explorador > números después de`environment/`**

   ![ID de programa y entorno](./assets/Program-Environment-Id.png)

1. Utilice el `aio cli`de `config:set` para establecer estos valores, ejecute el siguiente comando.

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

[Uso y comandos de CLI de aio](https://github.com/adobe/aio-cli#usage)

[Complemento de CLI de Adobe I/O Runtime AEM para interacciones con entornos de desarrollo rápido de](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Complemento de CLI de aio de Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)

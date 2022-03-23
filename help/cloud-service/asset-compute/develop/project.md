---
title: Creación de un proyecto de Asset compute para la extensibilidad del Asset compute
description: Los proyectos de asset compute son proyectos de Node.js, generados mediante la CLI de Adobe I/O, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM as a Cloud Service.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: 136049776140746c61d42ad1496df15a2d226e3a
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 1%

---

# Creación de un proyecto de Asset compute

Los proyectos de asset compute son proyectos de Node.js, generados usando la CLI de Adobe I/O, que se adhieren a una estructura determinada que les permite implementarse en Adobe I/O Runtime e integrarse con AEM as a Cloud Service. Un solo proyecto de Asset compute puede contener uno o más trabajadores de Asset compute, cada uno de los cuales tiene un punto final HTTP discreto referenciable desde un perfil de procesamiento as a Cloud Service de AEM.

## Generación de un proyecto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Pulsación para generar un proyecto de Asset compute (sin audio)_

Utilice la variable [Complemento de Asset compute CLI de Adobe I/O](../set-up/development-environment.md#aio-cli) para generar un nuevo proyecto de Asset compute vacío.

1. Desde la línea de comandos, vaya a la carpeta para contener el proyecto.
1. Desde la línea de comandos, ejecute `aio app init` para iniciar la CLI de generación de proyectos interactiva.
   + Este comando puede generar un explorador web que solicite la autenticación en el Adobe I/O. Si es así, proporcione las credenciales de Adobe asociadas con la variable [los servicios y productos requeridos de Adobe](../set-up/accounts-and-services.md). Si no puede iniciar sesión, siga [estas instrucciones sobre cómo generar un proyecto](https://www.adobe.io/project-firefly/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Seleccionar organización__
   + Seleccione la organización de Adobe que tiene AEM as a Cloud Service, Project Firefly está registrado con
1. __Seleccionar proyecto__
   + Busque y seleccione el proyecto. Esta es la [Título del proyecto](../set-up/firefly.md) creada a partir de la plantilla de proyecto Firefly, en este caso `WKND AEM Asset Compute`
1. __Seleccionar Workspace__
   + Seleccione el `Development` workspace
1. __¿Qué funciones de la aplicación de Adobe I/O desea habilitar para este proyecto? Seleccionar componentes para incluir__
   + Seleccione `Actions: Deploy runtime actions`
   + Utilice las teclas de flechas para seleccionar y el espacio para anular la selección o seleccionarlo, e Intro para confirmar la selección
1. __Seleccione el tipo de acciones que desea generar__
   + Seleccione `DX Asset Compute Worker v1`
   + Utilice las teclas de flechas para seleccionar, el espacio para anular la selección o seleccionarlo y la tecla Intro para confirmar la selección
1. __¿Cómo le gustaría nombrar esta acción?__
   + Usar el nombre predeterminado `worker`.
   + Si el proyecto contiene varios trabajadores que realizan diferentes cálculos de recursos, asígneles un nombre semántico

## Generar console.json

La herramienta para desarrolladores requiere un archivo denominado `console.json` que contiene las credenciales necesarias para conectarse a Adobe I/O. Este archivo se descarga desde la consola de Adobe I/O.

1. Abra la página del trabajador del Asset compute [Adobe I/O](https://console.adobe.io) proyecto
1. Seleccione el espacio de trabajo del proyecto para descargar el `console.json` credenciales para, en este caso seleccione `Development`
1. Vaya a la raíz del proyecto de Adobe I/O y pulse __Descargar todo__ en la esquina superior derecha.
1. Un archivo se descarga como `.json` archivo con el prefijo proyecto y espacio de trabajo, por ejemplo: `wkndAemAssetCompute-81368-Development.json`
1. Puede
   + Cambie el nombre del archivo como `config.json` y muévalo a la raíz del proyecto de trabajo de Asset compute. Este es el enfoque de este tutorial.
   + Moverlo a una carpeta arbitraria Y hacer referencia a esa carpeta desde su `.env` archivo con una entrada de configuración `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. La ruta del archivo puede ser absoluta o relativa a la raíz del proyecto. Por ejemplo:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      O bien
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> NOTA
> El archivo contiene credenciales. Si almacena el archivo dentro del proyecto, asegúrese de agregarlo al `.gitignore` para evitar que se compartan. Lo mismo se aplica a la variable `.env` archivo — Estos archivos de credenciales no deben compartirse ni almacenarse en Git.

## Revisar la anatomía del proyecto

El proyecto de Asset compute generado es un proyecto de Node.js para su uso como proyecto de Adobe especializado de Firefly. Los siguientes elementos estructurales son idiosincráticos del proyecto de Asset compute:

+ `/actions` contiene subcarpetas y cada subcarpeta define un programa de trabajo de Asset compute.
   + `/actions/<worker-name>/index.js` define el JavaScript utilizado para realizar el trabajo de este trabajador.
      + El nombre de la carpeta `worker` es un valor predeterminado y puede ser cualquier, siempre y cuando esté registrado en la variable `manifest.yml`.
      + Se puede definir más de una carpeta de trabajo en `/actions` según sea necesario, sin embargo, deben registrarse en la variable `manifest.yml`.
+ `/test/asset-compute` contiene los grupos de pruebas para cada trabajador. Similar a la variable `/actions` carpeta, `/test/asset-compute` puede contener varias subcarpetas, cada una correspondiente al programa de trabajo que prueba.
   + `/test/asset-compute/worker`, que representa un grupo de pruebas para un trabajador específico, contiene subcarpetas que representan un caso de prueba específico, junto con la entrada de prueba, los parámetros y la salida esperada.
+ `/build` contiene la salida, los registros y los artefactos de las ejecuciones de casos de prueba de Asset compute.
+ `/manifest.yml` define los Assets computes que proporciona el proyecto. Cada implementación de trabajador debe enumerarse en este archivo para que esté disponible para AEM as a Cloud Service.
+ `/console.json` define las configuraciones de Adobe I/O
   + Este archivo se puede generar o actualizar utilizando la variable `aio app use` comando.
+ `/.aio` contiene configuraciones utilizadas por la herramienta CLI de aio.
   + Este archivo se puede generar o actualizar utilizando la variable `aio app use` comando.
+ `/.env` define variables de entorno en un `key=value` y contiene secretos que no deben compartirse. Para proteger estos secretos, este archivo NO debe registrarse en Git y se ignora a través del proyecto predeterminado `.gitignore` archivo.
   + Este archivo se puede generar o actualizar utilizando la variable `aio app use` comando.
   + Las variables definidas en este archivo pueden ser anuladas por [exportación de variables](../deploy/runtime.md) en la línea de comandos.

Para obtener más información sobre el examen de la estructura del proyecto, consulte la [Anatomía de un proyecto de Adobe Firefly](https://www.adobe.io/project-firefly/docs/guides/).

La mayor parte del desarrollo tiene lugar en el `/actions` desarrollo de carpetas en implementaciones de trabajador y en `/test/asset-compute` escribir pruebas para los Assets computes personalizados.

## Proyecto de asset compute en GitHub

El proyecto de Asset compute final está disponible en GitHub en:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contiene el estado final del proyecto, totalmente completado con los casos de trabajo y prueba, pero no contiene credenciales, es decir, `.env`, `console.json` o `.aio`._

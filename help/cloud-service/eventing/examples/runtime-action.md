---
title: Eventos de acción y AEM de Adobe I/O Runtime
description: Obtenga información sobre cómo recibir eventos de AEM mediante la acción de Adobe I/O Runtime y revise la carga útil, los encabezados y los metadatos.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
duration: 457
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
exl-id: b1c127a8-24e7-4521-b535-60589a1391bf
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 1%

---

# Eventos de acción y AEM de Adobe I/O Runtime

Aprenda a recibir eventos de AEM mediante la acción [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) y a revisar los detalles del evento, como la carga útil, los encabezados y los metadatos.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime es una plataforma sin servidor que permite ejecutar código en respuesta a Adobe I/O Events. De este modo, le ayuda a crear aplicaciones basadas en eventos sin tener que preocuparse por la infraestructura.

En este ejemplo, crea una [Acción](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) de Adobe I/O Runtime que recibe Eventos de AEM y registra los detalles del evento.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

Los pasos de alto nivel son los siguientes:

- Creación de un proyecto en Adobe Developer Console
- Inicializar proyecto para desarrollo local
- Configuración de un proyecto en Adobe Developer Console
- Almacene en déclencheur el evento de AEM y compruebe la ejecución de la acción

## Requisitos previos

Para completar este tutorial, necesita lo siguiente:

- Entorno AEM as a Cloud Service con [evento de AEM habilitado](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Acceso a [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started).

- [CLI de Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalado en el equipo local.

## Creación de un proyecto en Adobe Developer Console

Para crear un proyecto en Adobe Developer Console, siga estos pasos:

- Vaya a [Adobe Developer Console](https://developer.adobe.com/) y haga clic en el botón **Consola**.

- En la sección **Inicio rápido**, haga clic en **Crear proyecto a partir de la plantilla**. A continuación, en el cuadro de diálogo **Examinar plantillas**, seleccione **App Builder**.

- Actualice el título del proyecto, el nombre de la aplicación y Añadir espacio de trabajo si es necesario. A continuación, haga clic en **Guardar**.

  ![Crear proyecto en Adobe Developer Console](../assets/examples/runtime-action/create-project.png)


## Inicializar proyecto para desarrollo local

Para agregar la acción de Adobe I/O Runtime al proyecto, debe inicializar el proyecto para el desarrollo local. En el equipo local, abra el terminal, desplácese hasta donde desee inicializar el proyecto y siga estos pasos:

- Inicialice el proyecto ejecutando

  ```bash
  aio app init
  ```

- Seleccione `Organization`, `Project` que creó en el paso anterior y el área de trabajo. En el paso `What templates do you want to search for?`, seleccione la opción `All Templates`.

  ![Selección-proyecto-organización - Inicializar proyecto](../assets/examples/runtime-action/all-templates.png)

- En la lista de plantillas, seleccione la opción `@adobe/generator-app-excshell`.

  ![Plantilla de extensibilidad - Inicializar proyecto](../assets/examples/runtime-action/extensibility-template.png)

- Abra un proyecto en su IDE favorito, por ejemplo VSCode.

- La _plantilla de extensibilidad_ (`@adobe/generator-app-excshell`) seleccionada proporciona una acción de tiempo de ejecución genérica; el código se encuentra en el archivo `src/dx-excshell-1/actions/generic/index.js`. Actualicémoslo para que sea sencillo, registremos los detalles del evento y devolvamos una respuesta de éxito. Sin embargo, en el siguiente ejemplo, se mejora para procesar los eventos de AEM recibidos.

  ```javascript
  const fetch = require("node-fetch");
  const { Core } = require("@adobe/aio-sdk");
  const {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs,
  } = require("../utils");
  
  // main function that will be executed by Adobe I/O Runtime
  async function main(params) {
  // create a Logger
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });
  
  try {
      // 'info' is the default level if not set
      logger.info("Calling the main action");
  
      // log parameters, only if params.LOG_LEVEL === 'debug'
      logger.debug(stringParameters(params));
  
      const response = {
      statusCode: 200,
      body: {
          message: "Received AEM Event, it will be processed in next example",
      },
      };
  
      // log the response status code
      logger.info(`${response.statusCode}: successful request`);
      return response;
  } catch (error) {
      // log any server errors
      logger.error(error);
      // return with 500
      return errorResponse(500, "server error", logger);
  }
  }
  
  exports.main = main;
  ```

- Finalmente, implemente la acción actualizada en Adobe I/O Runtime ejecutando.

  ```bash
  aio app deploy
  ```

## Configuración de un proyecto en Adobe Developer Console

Para recibir eventos de AEM y ejecutar la acción de Adobe I/O Runtime creada en el paso anterior, configure el proyecto en Adobe Developer Console.

- En Adobe Developer Console, vaya al [proyecto](https://developer.adobe.com/console/projects) creado en el paso anterior y haga clic en para abrirlo. Seleccione el área de trabajo `Stage`, aquí es donde se implementó la acción.

- Haga clic en el botón **Agregar servicio** y seleccione la opción **API**. En el modal **Add an API**, seleccione **Adobe Services** > **API de administración de E/S** y haga clic en **Siguiente**, siga los pasos de configuración adicionales y haga clic en **Guardar la API configurada**.

  ![Agregar servicio - Configurar proyecto](../assets/examples/runtime-action/add-io-management-api.png)

- Del mismo modo, haga clic en el botón **Agregar servicio** y seleccione la opción **Evento**. En el cuadro de diálogo **Agregar eventos**, seleccione **Experience Cloud** > **AEM Sites** y haga clic en **Siguiente**. Siga los pasos de configuración adicionales, seleccione la instancia de AEM CS, los tipos de evento y otros detalles.

- Finalmente, en el paso **Cómo recibir eventos**, expanda la opción **Acción en tiempo de ejecución** y seleccione la acción _genérica_ creada en el paso anterior. Haga clic en **Guardar eventos configurados**.

  ![Acción en tiempo de ejecución: configurar el proyecto ](../assets/examples/runtime-action/select-runtime-action.png)

- Revise los detalles de registro de eventos, también la ficha **Seguimiento de depuración** y compruebe la solicitud y respuesta de **Sondeo de desafío**.

  ![Detalles de registro de evento](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## Déclencheur de eventos de AEM

Para almacenar en déclencheur eventos de AEM desde el entorno de AEM as a Cloud Service que se haya registrado en el proyecto de Adobe Developer Console anterior, siga estos pasos:

- Acceda a su entorno de creación de AEM as a Cloud Service e inicie sesión a través de [Cloud Manager](https://my.cloudmanager.adobe.com/).

- Según sus **Eventos suscritos**, cree, actualice, elimine, publique o cancele la publicación de un fragmento de contenido.

## Revisar detalles del evento

Después de completar los pasos anteriores, debería ver los eventos de AEM que se envían a la acción genérica.

Puede revisar los detalles del evento en la ficha **Seguimiento de depuración** de los detalles de registro de eventos.

![Detalles de evento de AEM](../assets/examples/runtime-action/aem-event-details.png)


## Próximos pasos

En el siguiente ejemplo, vamos a mejorar esta acción para procesar eventos de AEM, volver a llamar al servicio de creación de AEM para obtener detalles de contenido, almacenar detalles en el almacenamiento de Adobe I/O Runtime y mostrarlos mediante la aplicación de una sola página (SPA).

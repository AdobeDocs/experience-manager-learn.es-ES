---
title: Eventos de acción y de de Adobe I/O Runtime AEM
description: AEM Obtenga información sobre cómo recibir eventos de mediante la acción de Adobe I/O Runtime y revise los detalles del evento, como la carga útil, los encabezados y los metadatos.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-01-29T00:00:00Z
jira: KT-14878
thumbnail: KT-14878.jpeg
source-git-commit: 85e1ee33626d27f1b6c07bc631a7c1068930f827
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---


# Eventos de acción y de de Adobe I/O Runtime AEM

AEM Obtenga información sobre cómo recibir eventos de mediante [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Acción y revisar los detalles del evento, como la carga útil, los encabezados y los metadatos.

>[!VIDEO](https://video.tv.adobe.com/v/3427053?quality=12&learn=on)

Adobe I/O Runtime es una plataforma sin servidor que permite ejecutar código en respuesta a eventos de Adobe I/O. De este modo, le ayuda a crear aplicaciones basadas en eventos sin tener que preocuparse por la infraestructura.

En este ejemplo, se crea un Adobe I/O Runtime [Acción](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) AEM que recibe eventos de y registra los detalles del evento.
https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/

Los pasos de alto nivel son los siguientes:

- Crear proyecto en la consola de Adobe Developer
- Inicializar proyecto para desarrollo local
- Configuración de un proyecto en la consola de Adobe Developer
- Déclencheur AEM de eventos de y verificación de la ejecución de acciones

## Requisitos previos

Para completar este tutorial, necesita lo siguiente:

- AEM entorno as a Cloud Service con [AEM Eventing activado de](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- Acceso a [Consola de Adobe Developer](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [CLI de Adobe Developer](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) instalado en el equipo local.

## Crear proyecto en la consola de Adobe Developer

Para crear un proyecto en la consola de Adobe Developer, siga estos pasos:

- Vaya a [Consola de Adobe Developer](https://developer.adobe.com/) y haga clic en **Consola** botón.

- En el **Inicio rápido** , haga clic en **Crear proyecto a partir de plantilla**. A continuación, en la **Examinar plantillas** diálogo, seleccione **Generador de aplicaciones** plantilla.

- Actualice el título del proyecto, el nombre de la aplicación y Añadir espacio de trabajo si es necesario. A continuación, haga clic en **Guardar**.

  ![Crear proyecto en la consola de Adobe Developer](../assets/examples/runtime-action/create-project.png)


## Inicializar proyecto para desarrollo local

Para agregar la acción de Adobe I/O Runtime al proyecto, debe inicializar el proyecto para el desarrollo local. En el equipo local, abra el terminal, desplácese hasta donde desee inicializar el proyecto y siga estos pasos:

- Inicialice el proyecto ejecutando

  ```bash
  aio app init
  ```

- Seleccione el `Organization`, el `Project` ha creado en el paso anterior y el espacio de trabajo. Entrada `What templates do you want to search for?` paso, seleccione `All Templates` opción.

  ![Org-Project-Selection: inicializar el proyecto](../assets/examples/runtime-action/all-templates.png)

- En la lista de plantillas, seleccione `@adobe/generator-app-excshell` opción.

  ![Plantilla de extensibilidad: Inicializar proyecto](../assets/examples/runtime-action/extensibility-template.png)

- Abra un proyecto en su IDE favorito, por ejemplo VSCode.

- El seleccionado _Plantilla de extensibilidad_ (`@adobe/generator-app-excshell`) proporciona una acción de tiempo de ejecución genérica, el código se encuentra en `src/dx-excshell-1/actions/generic/index.js` archivo. Actualicémoslo para que sea sencillo, registremos los detalles del evento y devolvamos una respuesta de éxito. AEM Sin embargo, en el siguiente ejemplo, se mejora para procesar los eventos de recibidos.

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

## Configuración de un proyecto en la consola de Adobe Developer

AEM Para recibir eventos de la y ejecutar la acción de Adobe I/O Runtime creada en el paso anterior, configure el proyecto en la consola de Adobe Developer.

- En la consola de Adobe Developer, vaya a [proyecto](https://developer.adobe.com/console/projects) creado en el paso anterior y haga clic en para abrirlo. Seleccione el `Stage` espacio de trabajo, aquí es donde se implementó la acción.

- Clic **Añadir servicio** y seleccione **API** opción. En el **Añadir una API** modal, seleccione **Servicios de Adobe** > **API de administración de E/S** y haga clic en **Siguiente**, siga los pasos de configuración adicionales y haga clic en **Guardar API configurada**.

  ![Agregar servicio: configurar proyecto](../assets/examples/runtime-action/add-io-management-api.png)

- Igualmente, haga clic en **Añadir servicio** y seleccione **Evento** opción. En el **Añadir eventos** diálogo, seleccione **Experience Cloud** > **AEM Sites** y haga clic en **Siguiente**. Siga los pasos de configuración adicionales, seleccione la instancia de AEM CS, los tipos de evento y otros detalles.

- Finalmente, en la **Cómo recibir eventos** paso, expandir **Acción Runtime** y seleccione la opción _genérico_ acción creada en el paso anterior. Clic **Guardar eventos configurados**.

  ![Acción de tiempo de ejecución: configurar proyecto ](../assets/examples/runtime-action/select-runtime-action.png)

- Revise los detalles de registro del evento, así como la **Seguimiento de depuración** y verifique la **Sondeo de desafío** solicitud y respuesta.

  ![Detalles de registro del evento](../assets/examples/runtime-action/debug-tracing-challenge-probe.png)


## Déclencheur AEM de eventos de

Para obtener un déclencheur AEM AEM de los eventos de la as a Cloud Service que se han registrado en el proyecto de la consola de Adobe Developer anterior, siga estos pasos:

- AEM Acceda a su entorno de creación as a Cloud Service de e inicie sesión mediante [Cloud Manager](https://my.cloudmanager.adobe.com/).

- En función de su **Eventos suscritos**, crear, actualizar, eliminar, publicar o cancelar la publicación de un fragmento de contenido.

## Revisar detalles del evento

AEM Después de completar los pasos anteriores, debería ver los eventos de la que se envían a la acción genérica.

Puede revisar los detalles del evento en la **Seguimiento de depuración** de los detalles de registro del evento.

![AEM Detalles de evento de](../assets/examples/runtime-action/aem-event-details.png)


## Pasos siguientes

AEM AEM En el siguiente ejemplo, vamos a mejorar esta acción para procesar eventos de, volver a llamar al servicio de creación de contenido para obtener detalles de contenido, almacenar detalles en el almacenamiento de Adobe I/O Runtime SPA y mostrarlos mediante la aplicación de una sola página ().


---
title: Acelerar las operaciones de contenido de AEM mediante el servidor MCP de contenido
description: Aprenda a utilizar AEM Content MCP Server desde su IDE con tecnología de IA preferido, como Cursor, para optimizar y acelerar las operaciones de contenido de AEM, reduciendo el esfuerzo manual y aumentando la productividad.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: tutorial
duration: null
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20474
exl-id: 843209cb-2f31-466c-b5b1-a9fb26965bc0
source-git-commit: 6a0eb6e8f5fa9d7152f46d6b8054dc89ff656507
workflow-type: tm+mt
source-wordcount: '850'
ht-degree: 0%

---

# Acelerar las operaciones de contenido de AEM mediante el servidor MCP de contenido

Use **Content MCP Server** de un IDE con tecnología de IA como [Cursor IDE](https://www.cursor.com/) para trabajar con contenido de AEM en lenguaje natural, sin código de API de bajo nivel ni navegación por la interfaz de usuario.

En este tutorial _revisará_ los detalles del fragmento de contenido Adventure, _actualizará_ un fragmento (por ejemplo, el precio de una aventura) y _verificará_ el cambio en la [aplicación WKND Adventures React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) desde su IDE con un _entorno AEM inferior_ (RDE o Desarrollo) sin abandonar el flujo MCP.

>[!VIDEO](https://video.tv.adobe.com/v/3480895/?learn=on&enablevpops)

## Información general

AEM as a Cloud Service proporciona _servidores MCP_ para que su aplicación de chat o IDE pueda trabajar con AEM de forma segura. **Content MCP Server** admite páginas, fragmentos y recursos. Consulte [Servidores MCP en AEM](./overview.md) para obtener más información.

## Cómo pueden usarlo los desarrolladores

Conecte el [IDE de cursor](https://www.cursor.com/) al servidor MCP de contenido y ejecute el escenario siguiente.

### Instalación: servidor MCP de contenido en el cursor

Vamos a configurar el servidor MCP de contenido en cursor con estos pasos.

1. Abra Cursor en el equipo.

1. Vaya a **Configuración** > **Configuración del cursor** desde el menú Cursor para abrir la ventana de configuración.
   ![Configuración del cursor](../assets/content-mcp-server/cursor-settings.png)

1. En la barra lateral izquierda, haga clic en **Herramientas y MCP** para abrir el panel.
   ![Herramientas y MCP](../assets/content-mcp-server/tools-mcp.png)

1. Haga clic en **Agregar MCP personalizado** o en **Nuevo servidor MCP** para abrir `mcp.json` y, a continuación, pegue esta configuración:

   ```json
   {
       "mcpServers": {
           // Use this for create, read, update, and delete operations
           "AEM-RDE-Content": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content"
           },
           //Use this for read-only operations
           "AEM-RDE-Content-Read-Only": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content-readonly"
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > Con fines de tutorial, la configuración anterior agrega **Contenido** y **Contenido (solo lectura)** para este tutorial. En la práctica, **Contenido** ya incluye todo lo que ofrece **Contenido (solo lectura)**, además de las herramientas de creación, actualización y eliminación.
   >
   >
   > Si desea evitar cualquier posibilidad de crear, modificar o eliminar contenido, configure solo **Contenido (solo lectura)** (`/content-readonly`) y omita **Contenido** (`/content`). De esa manera se evitan cambios accidentales.

   ![Agregar servidor MCP de AEM](../assets/content-mcp-server/mcp-json-file.png)

1. En la ventana Configuración del cursor, haga clic en **Conectar** para iniciar el proceso de autenticación. Utiliza el flujo PKCE de OAuth 2.0 para obtener el **token de acceso específico del usuario** para acceder al servidor MCP de AEM.
   ![Conectarse al servidor MCP de AEM](../assets/content-mcp-server/connect-to-aem-mcp-server.png)

1. Inicie sesión con su Adobe ID y, a continuación, vuelva a la ventana Configuración del cursor.
   ![Iniciar sesión con Adobe ID](../assets/content-mcp-server/login-with-adobe-id.png)

1. Confirme que **AEM-RDE-Content-Read-Only** y **AEM-RDE-Content** se muestran como conectados. Puede expandir cada servidor para ver sus herramientas.

   ![Servidores MCP de AEM](../assets/content-mcp-server/connected-aem-mcp-servers.png)

### Configuración - Aplicación React de WKND Adventures

A continuación, configure la [aplicación WKND Adventures React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) en el cursor.

1. Clone estos dos repositorios en su equipo:

   ```bash
   ## WKND GraphQL repo, the `react-app` folder is the WKND Adventures app
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   
   ## WKND Site repo, you deploy this to RDE so the app can use its content fragments data via GraphQL
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Implemente el proyecto [WKND Site](https://github.com/adobe/aem-guides-wknd) en su RDE. Para ver los pasos detallados, consulte [Cómo usar el entorno de desarrollo rápido](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use#deploy-aem-artifacts-using-the-aem-rde-plugin).

1. Abra la carpeta `react-app` en su IDE.

1. Editar `.env.development` y establecer:
   - `REACT_APP_HOST_URI`: su URL de autor de RDE
   - `REACT_APP_AUTH_METHOD`: para ser `basic`
   - `REACT_APP_BASIC_AUTH_USER` y `REACT_APP_AEM_AUTH_PASSWORD`: para ser `aem-headless` (cree este usuario en RDE y agréguelo al grupo `administrators`)

1. Desde el terminal IDE, ejecute:

   ```bash
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. En tu navegador, ve a [http://localhost:3000](http://localhost:3000) para ver la aplicación WKND Adventures.

   ![Aplicación React - WKND Adventures](../assets/content-mcp-server/react-app-wknd-adventures.png)

### Escenario de productividad: revisión y actualización de contenido de AEM

Supongamos que necesita mostrar un banner de _OFERTA ACTUAL_ en las tarjetas de Aventura cuando se cumple una regla simple. El enfoque habitual sería:

- Consulte el código de componente Tarjetas de aventura
- Añada la lógica de cuándo mostrar el titular
- Consulte el modelo de fragmento de contenido Aventura en AEM
- Cambie una o más propiedades de fragmento de aventura para probar la regla

Para que todo sea simple, vamos a mostrar el banner de _OFERTA CALIENTE_ cuando el precio de la aventura sea inferior a $100.

Dado que la aplicación React obtiene sus datos del entorno de RDE, debe conocer el modelo de fragmento de contenido de Aventura y, a continuación, actualizar las propiedades de fragmento correctas. Esto es exactamente lo que AEM Content MCP Server puede ayudar. A continuación se muestra cómo.

1. En Cursor, abra una nueva conversación y escriba:

   ```text
   I want to review my Content Fragment Models from AEM RDE, can you list the Adventure Content Fragment details.
   ```

   ![Revisar modelos de fragmentos de contenido](../assets/content-mcp-server/review-content-fragment-models-prompt-response.png)


   Antes de invocar al servidor MCP de contenido, solicita confirmación para continuar. Por lo tanto, se mantiene el control de las operaciones de contenido.

   La API utiliza el servidor MCP de contenido para recuperar los datos y luego los presenta de una manera clara y estructurada. Incluye detalles del modelo de fragmento de contenido, el número de fragmentos y la información de resumen.

1. Para poner en déclencheur el banner _HOT DEAL_, actualiza el precio de una aventura. En el mismo chat, intente:

   ```text
   Can you update adventure Beervana in Portland's price to 99.99
   ```

   ![Actualizar precio de aventura](../assets/content-mcp-server/update-adventure-price-prompt-response.png)

   Del mismo modo, la API solicita confirmación para continuar antes de actualizar el contenido. También resume la operación de contenido antes y después de la actualización.

1. En la aplicación React, confirma que la tarjeta Beervana ahora muestra el banner _HOT DEAL_.

   ![Verificar titular de oferta actual](../assets/content-mcp-server/verify-hot-deal-banner.png)

### Indicadores adicionales

Pruebe estas peticiones de datos centradas en el contenido en su IDE (con Content MCP Server conectado) para explorar más flujos de trabajo y funciones.

- Descubrir contenido:

  ```text
  List all content fragments in the WKND Adventures folder
  
  List all WKND Site pages from US English site
  
  Can you give me page metadata for Tahoe Skiing English page? 
  
  List assets of Bali Surf camp
  
  What Content Fragment models are available in this environment?
  ```

- Buscar contenido:

  ```text
  Search for content fragments that mention 'cycling'
  
  Do we have a magazine page in US English site with "Camping" in it
  ```

- Actualizar contenido:

  ```text
  In WKND US English create a copy of Downhill Skiing Wyoming as "Test Downhill Skiing Wyoming"
  
  In newly created "Test Downhill Skiing Wyoming" please change title to "Duplicated Page"
  ```

- Publicar o cancelar la publicación:

  ```text
  Can you publish the page at /us/en/adventures/test-downhill-skiing-wyoming and give me publish page URL
  
  Can you unpublish the test-downhill-skiing-wyoming page
  ```

## Resumen

El servidor MCP de contenido de AEM se configura en Cursor y se conecta al entorno RDE (o Desarrollo). A continuación, utilizó la aplicación WKND Adventures React y charló en lenguaje natural para revisar los detalles del fragmento de contenido Adventure. También ha actualizado el precio de un fragmento con la API solicitando su confirmación antes de cada operación de contenido. Ha verificado el cambio en la aplicación en ejecución. Puede utilizar el mismo flujo centrado en las personas desde su IDE para revisar, actualizar y crear contenido de AEM sin cambiar a la interfaz de usuario de AEM ni escribir código de API de bajo nivel.

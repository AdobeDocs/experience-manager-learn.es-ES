---
title: 'AEM AEM Implementación de producción mediante un servicio de publicación en la: Introducción a las soluciones sin encabezado - GraphQL'
description: AEM Obtenga información acerca de los servicios de creación y publicación de y el patrón de implementación recomendado para aplicaciones sin encabezado. En este tutorial, aprenderá a utilizar variables de entorno para cambiar dinámicamente un extremo de GraphQL en función del entorno de destino. AEM Aprenda a configurar correctamente los recursos de intercambio de recursos de origen cruzado (CORS) para su uso compartido.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
duration: 620
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2137'
ht-degree: 5%

---

# AEM Implementación de producción con un servicio de publicación de

En este tutorial, se configura un entorno local para simular el contenido que se distribuye desde una instancia de autor a una instancia de publicación. AEM También generará una compilación de producción de una aplicación de React configurada para consumir contenido del entorno de publicación de mediante las API de GraphQL. AEM A lo largo del camino, aprenderá a utilizar de forma eficaz las variables de entorno y a actualizar las configuraciones de CORS de la.

## Requisitos previos

Este tutorial forma parte de un tutorial de varias partes. Se supone que se han completado los pasos descritos en las partes anteriores.

## Objetivos

Obtenga información sobre cómo:

* AEM Comprender la arquitectura de autor y publicación de la.
* Conozca las prácticas recomendadas para administrar variables de entorno.
* AEM Obtenga información sobre cómo configurar correctamente los para el uso compartido de recursos de origen cruzado (CORS).

## Crear patrón de implementación de publicación {#deployment-pattern}

Un entorno de AEM completo está formado por un Autor, una Publicación y un Dispatcher. El servicio de creación es donde los usuarios internos crean, administran y previsualizan contenido. El servicio de publicación se considera el entorno &quot;activo&quot; y suele ser con el que interactúan los usuarios finales. El contenido, después de editarse y aprobarse en el servicio de creación, se distribuye al de publicación.

El patrón de implementación más común con las aplicaciones sin encabezado de AEM es tener la versión de producción de la aplicación conectada a un servicio de publicación de AEM.

![Patrón de implementación de alto nivel](assets/publish-deployment/high-level-deployment.png)

El diagrama anterior muestra este patrón de implementación común.

1. A **Autor del contenido** AEM utiliza el servicio de creación de para crear, editar y administrar contenido.
2. El **autor de contenido** y otros usuarios internos pueden obtener una previsualización del contenido directamente en el servicio de creación. Se puede configurar una versión de previsualización de la aplicación que se conecte al servicio de creación.
3. Una vez aprobado el contenido, se puede **publicado** AEM al servicio Publicación de la.
4. **Usuarios finales** interactúe con la versión de producción de la aplicación. La aplicación de producción se conecta al servicio de publicación y utiliza las API de GraphQL para solicitar y consumir contenido.

AEM El tutorial simula la implementación anterior añadiendo una instancia de Publicación de la a la configuración actual. En capítulos anteriores, la aplicación React actuaba como una previsualización conectándose directamente a la instancia de autor. Una compilación de producción de la aplicación React se implementa en un servidor estático Node.js que se conecta a la nueva instancia de publicación.

Al final, se están ejecutando tres servidores locales:

* http://localhost:4502: instancia de autor
* http://localhost:4503 - Instancia de publicación
* http://localhost:5000: aplicación de React en modo de producción, conectándose a la instancia de publicación.

## AEM Instalación de SDK de: modo de publicación {#aem-sdk-publish}

Actualmente tenemos una instancia en ejecución del SDK en **Autor** modo. El SDK también se puede iniciar en **Publish** AEM modo para simular un entorno de publicación de la.

Una guía más detallada para configurar un entorno de desarrollo local [se puede encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. En el sistema de archivos local, cree una carpeta dedicada para instalar la instancia de publicación, con el nombre `~/aem-sdk/publish`.
1. Copie el archivo jar de inicio rápido utilizado para la instancia de autor en capítulos anteriores y péguelo en `publish` directorio. Alternativamente, vaya a [Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html) Descargue el SDK más reciente y extraiga el archivo jar de Quickstart.
1. Cambie el nombre del archivo jar a `aem-publish-p4503.jar`.

   El `publish` La cadena especifica que el JAR de inicio rápido se inicia en el modo Publicación. El `p4503` especifica que el servidor de Quickstart se ejecuta en el puerto 4503.

1. Abra una nueva ventana de terminal y vaya a la carpeta que contiene el archivo jar. AEM Instale e inicie la instancia de:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Proporcione una contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar la predeterminada para el desarrollo local a fin de evitar configuraciones adicionales.
1. AEM Una vez que la instancia de haya terminado de instalarse, se abrirá una nueva ventana del explorador en [http://localhost:4503/content.html](http://localhost:4503/content.html)

   Se espera que devuelva una página 404 Not Found. AEM Esta es una instancia totalmente nueva de y no se ha instalado ningún contenido.

## Instalación de contenido de muestra y puntos finales de GraphQL {#wknd-site-content-endpoints}

Al igual que en la instancia de autor, la instancia de publicación debe tener habilitados los extremos de GraphQL y necesita contenido de muestra. A continuación, instale el sitio de referencia de WKND en la instancia de publicación.

1. AEM Descargue el último paquete de datos compilado para el sitio WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Asegúrese de descargar la versión estándar compatible con las versiones as a Cloud Service y AEM de **no** el `classic` versión.

1. Inicie sesión en la instancia de publicación navegando directamente a: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) con el nombre de usuario `admin` y contraseña `admin`.
1. A continuación, vaya al Administrador de paquetes en [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Clic **Cargar paquete** y elija el paquete WKND descargado en el paso anterior. Haga clic en **Instalar** para instalar el paquete.
1. Después de instalar el paquete, el sitio de referencia de WKND ya está disponible en [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Cerrar sesión como `admin` haciendo clic en el botón &quot;Cerrar sesión&quot; en la barra de menús.

   ![Sitio de referencia de cierre de sesión de WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   AEM AEM A diferencia de la instancia de autor de la, las instancias de publicación de la aplicación tienen de forma predeterminada acceso anónimo de solo lectura. Queremos simular la experiencia de un usuario anónimo al ejecutar la aplicación React.

## Actualizar las variables de entorno para que apunten a la instancia de publicación {#react-app-publish}

A continuación, actualice las variables de entorno utilizadas por la aplicación React para que apunten a la instancia de Publish. La aplicación React debe **solamente** conéctese a la instancia de Publish en el modo de producción.

A continuación, añada un nuevo archivo `.env.production.local` para simular la experiencia de producción.

1. Abra la aplicación WKND GraphQL React en su IDE.

1. Debajo `aem-guides-wknd-graphql/react-app`, agregue un archivo con el nombre `.env.production.local`.
1. Rellenar `.env.production.local` con lo siguiente:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Agregar nuevo archivo de variable de entorno](assets/publish-deployment/env-production-local-file.png)

   El uso de variables de entorno facilita la alternancia del extremo de GraphQL entre un entorno de autor o publicación sin agregar lógica adicional dentro del código de la aplicación. Más información sobre [Las variables de entorno personalizadas para React se pueden encontrar aquí](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Tenga en cuenta que no se incluye ninguna información de autenticación, ya que los entornos de publicación proporcionan acceso anónimo al contenido de forma predeterminada.

## Implementación de un servidor de nodos estático {#static-server}

La aplicación React se puede iniciar usando el servidor de Webpack, pero solo es para desarrollo. A continuación, simule una implementación de producción utilizando [servir](https://github.com/vercel/serve) para alojar una compilación de producción de la aplicación React mediante Node.js.

1. Abra una nueva ventana de terminal y vaya al `aem-guides-wknd-graphql/react-app` directorio

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Instalar [servir](https://github.com/vercel/serve) con el siguiente comando:

   ```shell
   $ npm install serve --save-dev
   ```

1. Abra el archivo `package.json` en `react-app/package.json`. Añada un script llamado `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   El `serve` El script realiza dos acciones. En primer lugar, se genera una compilación de producción de la aplicación React. Segundo, el servidor Node.js se inicia y utiliza la compilación de producción.

1. Vuelva al terminal e introduzca el comando para iniciar el servidor estático:

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. Abra un explorador nuevo y vaya a [http://localhost:5000/](http://localhost:5000/). Debería ver el servicio de la aplicación React.

   ![Aplicación React ofrecida](assets/publish-deployment/react-app-served-port5000.png)

   Observe que la consulta de GraphQL funciona en la página principal. Inspect el **XHR** solicitud de con sus herramientas para desarrolladores. Observe que el POST de GraphQL se conecta a la instancia de publicación en `http://localhost:4503/content/graphql/global/endpoint.json`.

   Sin embargo, todas las imágenes están rotas en la página de inicio!

1. Haga clic en una de las páginas de detalles de la aventura.

   ![Error de detalles de aventura](assets/publish-deployment/adventure-detail-error.png)

   Observe que se produce un error de GraphQL para `adventureContributor`. En los próximos ejercicios, las imágenes rotas y la `adventureContributor` los problemas se han corregido.

## Referencias de imagen absolutas {#absolute-image-references}

Las imágenes parecen rotas debido a la `<img src` se establece en una ruta relativa y termina apuntando al servidor estático del nodo en `http://localhost:5000/`. AEM En su lugar, estas imágenes deben apuntar a la instancia Publicación de la. Hay varias soluciones potenciales para esto. Al utilizar el servidor de desarrollo de Webpack, el archivo `react-app/src/setupProxy.js` AEM configure un proxy entre el servidor de Webpack y la instancia de autor de la para cualquier solicitud a `/content`. Se puede utilizar una configuración proxy en un entorno de producción, pero debe configurarse en el nivel de servidor web. Por ejemplo, [Módulo de proxy de Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

Se puede actualizar la aplicación para incluir una dirección URL absoluta utilizando `REACT_APP_HOST_URI` variable de entorno. AEM En su lugar, vamos a utilizar una función de la API de GraphQL para solicitar una dirección URL absoluta a la imagen.

1. Detenga el servidor Node.js.
1. Vuelva al IDE y abra el archivo `Adventures.js` en `react-app/src/components/Adventures.js`.
1. Añada el `_publishUrl` a la propiedad `ImageRef` dentro de `allAdventuresQuery`:

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` y `_authorUrl` son valores integrados en la variable `ImageRef` para facilitar la inclusión de direcciones url absolutas.

1. Repita los pasos anteriores para modificar la consulta utilizada en `filterQuery(activity)` función para incluir el `_publishUrl` propiedad.
1. Modifique la `AdventureItem` componente en `function AdventureItem(props)` para hacer referencia al `_publishUrl` en lugar del `_path` al construir la propiedad `<img src=''>` etiqueta:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Abra el archivo `AdventureDetail.js` en `react-app/src/components/AdventureDetail.js`.
1. Repita los mismos pasos para modificar la consulta de GraphQL y agregue `_publishUrl` propiedad para la aventura

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. Modifique los dos `<img>` Etiquetas para la imagen principal de aventura y la referencia de imagen del colaborador en `AdventureDetail.js`:

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. Vuelva al terminal e inicie el servidor estático:

   ```shell
   $ npm run serve
   ```

1. Vaya a [http://localhost:5000/](http://localhost:5000/) y observar que las imágenes aparecen y que la `<img src''>` puntos de atributo a `http://localhost:4503`.

   ![Imágenes rotas corregidas](assets/publish-deployment/broken-images-fixed.png)

## Simular publicación de contenido {#content-publish}

Recuerde que se produce un error de GraphQL para `adventureContributor` cuando se solicita una página de detalles de la aventura. El **Colaborador** El modelo de fragmento de contenido aún no existe en la instancia de publicación. Actualizaciones realizadas en **Aventura** El modelo de fragmento de contenido tampoco está disponible en la instancia Publicar. Estos cambios se realizaron directamente en la instancia de autor y deben distribuirse en la instancia de publicación.

Esto es algo que hay que tener en cuenta a la hora de implementar nuevas actualizaciones en una aplicación que depende de actualizaciones en un fragmento de contenido o un modelo de fragmento de contenido.

A continuación, permite simular la publicación de contenido entre las instancias locales Author y Publish.

1. Inicie la instancia de autor (si no se ha iniciado ya) y vaya al Administrador de paquetes en [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Descargar el paquete [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) e instálelo mediante el Administrador de paquetes.

   Este paquete instala una configuración que permite a la instancia de autor publicar contenido en la instancia de publicación. Pasos manuales para [esta configuración se puede encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > AEM En un entorno as a Cloud Service de, el nivel de Author se configura automáticamente para distribuir contenido al de Publish.

1. Desde el **AEM Inicio de** , vaya a **Herramientas** > **Assets** > **Modelos de fragmento de contenido**.

1. Haga clic en **Sitio WKND** carpeta.

1. Seleccione los tres modelos y haga clic en **Publish**:

   ![Publicar modelos de fragmentos de contenido](assets/publish-deployment/publish-contentfragment-models.png)

   Aparecerá un cuadro de diálogo de confirmación, haga clic en **Publish**.

1. Vaya al fragmento de contenido del Bali Surf Camp en [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Haga clic en **Publish** en la barra de menús superior.

   ![Haga clic en el botón Publicar en el Editor de fragmentos de contenido](assets/publish-deployment/publish-bali-content-fragment.png)

1. El asistente Publicar muestra todos los recursos dependientes que deben publicarse. En este caso, el fragmento referenciado **stacey-roswells** y también se hace referencia a varias imágenes. Los recursos a los que se hace referencia se publican junto con el fragmento.

   ![Recursos referenciados para publicar](assets/publish-deployment/referenced-assets.png)

   Haga clic en **Publish** para volver a publicar el fragmento de contenido y los recursos dependientes.

1. Vuelva a la aplicación de React que se ejecuta en [http://localhost:5000/](http://localhost:5000/). Ahora puede hacer clic en el Bali Surf Camp para ver los detalles de la aventura.

1. AEM Vuelva a la instancia de autor de la en [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) y actualice el **Título** del fragmento. **Guardar y cerrar** el fragmento. Entonces **publicar** el fragmento.
1. Volver a [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) y observe los cambios publicados.

   ![Bali Surf Camp Publicar actualización](assets/publish-deployment/bali-surf-camp-update.png)

## Actualizar configuración de COR

AEM AEM La seguridad de es de forma predeterminada y no permite que las propiedades web que no son de tipo realicen llamadas del lado del cliente. AEM AEM La configuración de Intercambio de recursos de origen cruzado (CORS) de puede permitir que dominios específicos realicen llamadas a la.

AEM A continuación, experimente con la configuración CORS de la instancia de publicación de la.

1. Vuelva a la ventana del terminal donde se está ejecutando la aplicación React con el comando. `npm run serve`:

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   Tenga en cuenta que se proporcionan dos direcciones URL. Uno con `localhost` y otro que utiliza la dirección IP de la red local.

1. Vaya a la dirección que empieza por [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). La dirección es ligeramente diferente para cada equipo local. Observe que hay un error CORS al recuperar los datos. Esto se debe a que la configuración actual de CORS solo permite solicitudes de `localhost`.

   ![Error CORS](assets/publish-deployment/cors-error-not-fetched.png)

   AEM A continuación, actualice la configuración de CORS Publicar en la para permitir solicitudes desde la dirección IP de red.

1. Vaya a [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) e inicie sesión con el nombre de usuario `admin` y contraseña `admin`.

1. Vaya a [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) y busque la configuración de WKND GraphQL en `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Actualice el **Orígenes permitidos** para incluir la dirección IP de red:

   ![Actualizar configuración de CORS](assets/publish-deployment/cors-update.png)

   También es posible incluir una expresión regular para permitir todas las solicitudes de un subdominio específico. Guarde los cambios.

1. Buscar por **Filtro de referente de Apache Sling** y revise la configuración. El **Permitir vaciado** también es necesaria la configuración de para habilitar las solicitudes de GraphQL desde un dominio externo.

   ![Filtro de remitente del reenvío de Sling](assets/publish-deployment/sling-referrer-filter.png)

   Se han configurado como parte del sitio de referencia de WKND. Puede ver el conjunto completo de configuraciones de OSGi a través de [el repositorio de GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > AEM Las configuraciones de OSGi se administran en un proyecto de que está comprometido con el control de código fuente. AEM AEM Cloud Service Un proyecto de se puede implementar en entornos de como mediante Cloud Manager. El [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) puede ayudar a generar un proyecto para una implementación específica.

1. Vuelva a la aplicación de React a partir de [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) y observe que la aplicación ya no emite un error CORS.

   ![Error de CORS corregido](assets/publish-deployment/cors-error-corrected.png)

## Enhorabuena. {#congratulations}

Felicitaciones. AEM Ahora ha simulado una implementación de producción completa utilizando un entorno de publicación en la. AEM También ha aprendido a utilizar la configuración CORS en la configuración de la aplicación de la configuración de la configuración de la aplicación de la configuración de la aplicación de forma.

## Otros recursos

Para obtener más información sobre los fragmentos de contenido y GraphQL, consulte los siguientes recursos:

* [Entrega de contenido sin encabezado mediante fragmentos de contenido con GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=es)
* [API de GraphQL de AEM para su uso con fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=es)
* [Autenticación basada en tokens](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [AEM Implementación del código para que se ejecute de forma as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)

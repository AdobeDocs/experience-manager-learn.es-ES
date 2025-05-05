---
title: 'Implementación de producción mediante un servicio de publicación de AEM: Introducción a AEM sin encabezado: GraphQL'
description: Obtenga información acerca de los servicios de AEM Author y Publish y el patrón de implementación recomendado para aplicaciones sin encabezado. En este tutorial, aprenderá a utilizar variables de entorno para cambiar dinámicamente un extremo de GraphQL en función del entorno de destino. Aprenda a configurar correctamente AEM para el uso compartido de recursos de origen cruzado (CORS).
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
duration: 486
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2137'
ht-degree: 5%

---

# Implementación de producción con un servicio de publicación de AEM

En este tutorial, se configura un entorno local para simular el contenido que se distribuye desde una instancia de autor a una instancia de publicación. También generará una compilación de producción de una aplicación de React configurada para consumir contenido del entorno de publicación de AEM mediante las API de GraphQL. Con el tiempo aprenderá a utilizar de forma eficaz las variables de entorno y a actualizar las configuraciones CORS de AEM.

## Requisitos previos

Este tutorial forma parte de un tutorial de varias partes. Se supone que se han completado los pasos descritos en las partes anteriores.

## Objetivos

Obtenga información sobre cómo:

* Comprender la arquitectura de AEM Author y Publish.
* Conozca las prácticas recomendadas para administrar variables de entorno.
* Aprenda a configurar correctamente AEM para el uso compartido de recursos de origen cruzado (CORS).

## Crear patrón de implementación de publicación {#deployment-pattern}

Un entorno de AEM completo está formado por un Autor, una Publicación y un Dispatcher. El servicio de creación es donde los usuarios internos crean, administran y previsualizan contenido. El servicio de publicación se considera el entorno &quot;activo&quot; y suele ser con el que interactúan los usuarios finales. El contenido, después de editarse y aprobarse en el servicio de creación, se distribuye al de publicación.

El patrón de implementación más común con las aplicaciones sin encabezado de AEM es tener la versión de producción de la aplicación conectada a un servicio de publicación de AEM.

![Patrón de implementación de alto nivel](assets/publish-deployment/high-level-deployment.png)

El diagrama anterior muestra este patrón de implementación común.

1. Un **autor de contenido** utiliza el servicio de autor de AEM para crear, editar y administrar contenido.
2. El **autor de contenido** y otros usuarios internos pueden obtener una previsualización del contenido directamente en el servicio de creación. Se puede configurar una versión de previsualización de la aplicación que se conecte al servicio de creación.
3. Una vez aprobado el contenido, puede **publicarse** en el servicio de publicación de AEM.
4. **Los usuarios finales** interactúan con la versión de producción de la aplicación. La aplicación de producción se conecta al servicio de publicación y utiliza las API de GraphQL para solicitar y consumir contenido.

El tutorial simula la implementación anterior añadiendo una instancia de publicación de AEM a la configuración actual. En capítulos anteriores, la aplicación React actuaba como una previsualización conectándose directamente a la instancia de autor. Una compilación de producción de la aplicación React se implementa en un servidor estático Node.js que se conecta a la nueva instancia de publicación.

Al final, se están ejecutando tres servidores locales:

* http://localhost:4502: instancia de autor
* http://localhost:4503 - Instancia de publicación
* http://localhost:5000: aplicación de React en modo de producción, conectándose a la instancia de publicación.

## Instalación de AEM SDK: modo Publicación {#aem-sdk-publish}

Actualmente tenemos una instancia en ejecución de SDK en modo **Author**. SDK también se puede iniciar en modo **Publish** para simular un entorno de publicación de AEM.

Puede encontrar una guía más detallada para configurar un entorno de desarrollo local [aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es#local-development-environment-set-up).

1. En el sistema de archivos local, cree una carpeta dedicada para instalar la instancia de publicación, con el nombre `~/aem-sdk/publish`.
1. Copie el archivo jar de inicio rápido utilizado para la instancia de autor en capítulos anteriores y péguelo en el directorio `publish`. También puede navegar hasta [Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html), descargar la última versión de SDK y extraer el archivo jar de Quickstart.
1. Cambie el nombre del archivo jar a `aem-publish-p4503.jar`.

   La cadena `publish` especifica que el JAR de inicio rápido se inicia en el modo Publicación. El `p4503` especifica que el servidor de Quickstart se ejecuta en el puerto 4503.

1. Abra una nueva ventana de terminal y vaya a la carpeta que contiene el archivo jar. Instale e inicie la instancia de AEM:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Proporcione una contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar la predeterminada para el desarrollo local a fin de evitar configuraciones adicionales.
1. Cuando la instancia de AEM haya terminado de instalarse, se abrirá una nueva ventana del explorador en [http://localhost:4503/content.html](http://localhost:4503/content.html)

   Se espera que devuelva una página 404 Not Found. Es una instancia de AEM completamente nueva y no se ha instalado ningún contenido.

## Instalación de contenido de muestra y puntos finales de GraphQL {#wknd-site-content-endpoints}

Al igual que en la instancia de autor, la instancia de publicación debe tener habilitados los extremos de GraphQL y necesita contenido de muestra. A continuación, instale el sitio de referencia de WKND en la instancia de publicación.

1. Descargue el paquete de AEM compilado más reciente para el sitio WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Asegúrese de descargar la versión estándar compatible con AEM as a Cloud Service y **no** la versión de `classic`.

1. Inicie sesión en la instancia de publicación navegando directamente a: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) con el nombre de usuario `admin` y la contraseña `admin`.
1. A continuación, vaya al Administrador de paquetes en [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Haga clic en **Cargar paquete** y elija el paquete WKND descargado en el paso anterior. Haga clic en **Instalar** para instalar el paquete.
1. Después de instalar el paquete, el sitio de referencia de WKND ya está disponible en [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Para cerrar la sesión como usuario de `admin`, haga clic en el botón &quot;Cerrar sesión&quot; en la barra de menús.

   ![Sitio de referencia de cierre de sesión de WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   A diferencia de la instancia de autor de AEM, las instancias de publicación de AEM tienen de forma predeterminada acceso anónimo de solo lectura. Queremos simular la experiencia de un usuario anónimo al ejecutar la aplicación React.

## Actualizar las variables de entorno para que apunten a la instancia de publicación {#react-app-publish}

A continuación, actualice las variables de entorno utilizadas por la aplicación React para que apunten a la instancia de Publish. La aplicación React debería **solamente** conectarse a la instancia Publish en el modo de producción.

A continuación, agregue un nuevo archivo `.env.production.local` para simular la experiencia de producción.

1. Abra la aplicación WKND GraphQL React en su IDE.

1. Debajo de `aem-guides-wknd-graphql/react-app`, agregue un archivo de nombre `.env.production.local`.
1. Rellene `.env.production.local` con lo siguiente:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Agregar nuevo archivo de variable de entorno](assets/publish-deployment/env-production-local-file.png)

   El uso de variables de entorno facilita la alternancia del extremo de GraphQL entre un entorno de autor o publicación sin agregar lógica adicional dentro del código de la aplicación. Encontrará más información sobre [variables de entorno personalizadas para React aquí](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Tenga en cuenta que no se incluye ninguna información de autenticación, ya que los entornos de publicación proporcionan acceso anónimo al contenido de forma predeterminada.

## Implementación de un servidor de nodos estático {#static-server}

La aplicación React se puede iniciar usando el servidor de Webpack, pero solo es para desarrollo. A continuación, simule una implementación de producción usando [serve](https://github.com/vercel/serve) para hospedar una compilación de producción de la aplicación React usando Node.js.

1. Abra una nueva ventana de terminal y vaya al directorio `aem-guides-wknd-graphql/react-app`

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Instale [serve](https://github.com/vercel/serve) con el siguiente comando:

   ```shell
   $ npm install serve --save-dev
   ```

1. Abra el archivo `package.json` en `react-app/package.json`. Agregar un script de nombre `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   El script `serve` realiza dos acciones. En primer lugar, se genera una compilación de producción de la aplicación React. Segundo, el servidor Node.js se inicia y utiliza la compilación de producción.

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

   ![Aplicación React Served](assets/publish-deployment/react-app-served-port5000.png)

   Observe que la consulta de GraphQL funciona en la página principal. Inspeccione la solicitud **XHR** con sus herramientas para desarrolladores. Observe que la PUBLICACIÓN DE GraphQL se envía a la instancia de publicación en `http://localhost:4503/content/graphql/global/endpoint.json`.

   Sin embargo, todas las imágenes están rotas en la página de inicio!

1. Haga clic en una de las páginas de detalles de la aventura.

   ![Error de detalles de aventura](assets/publish-deployment/adventure-detail-error.png)

   Observe que se produce un error de GraphQL para `adventureContributor`. En los próximos ejercicios, se solucionarán las imágenes rotas y los problemas de `adventureContributor`.

## Referencias de imagen absolutas {#absolute-image-references}

Las imágenes parecen rotas porque el atributo `<img src` está establecido en una ruta relativa y termina señalando al servidor estático del nodo en `http://localhost:5000/`. En su lugar, estas imágenes deben señalar a la instancia de publicación de AEM. Hay varias soluciones potenciales para esto. Al usar el servidor de desarrollo de Webpack, el archivo `react-app/src/setupProxy.js` configuró un proxy entre el servidor de Webpack y la instancia de autor de AEM para cualquier solicitud a `/content`. Se puede utilizar una configuración proxy en un entorno de producción, pero debe configurarse en el nivel de servidor web. Por ejemplo, [módulo proxy de Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

Se puede actualizar la aplicación para incluir una dirección URL absoluta mediante la variable de entorno `REACT_APP_HOST_URI`. En su lugar, vamos a utilizar una función de la API de GraphQL de AEM para solicitar una URL absoluta a la imagen.

1. Detenga el servidor Node.js.
1. Vuelva al IDE y abra el archivo `Adventures.js` en `react-app/src/components/Adventures.js`.
1. Agregar la propiedad `_publishUrl` a `ImageRef` dentro de `allAdventuresQuery`:

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

   `_publishUrl` y `_authorUrl` son valores integrados en el objeto `ImageRef` para facilitar la inclusión de direcciones URL absolutas.

1. Repita los pasos anteriores para modificar la consulta utilizada en la función `filterQuery(activity)` a fin de incluir la propiedad `_publishUrl`.
1. Modifique el componente `AdventureItem` en `function AdventureItem(props)` para hacer referencia a `_publishUrl` en lugar de a la propiedad `_path` al construir la etiqueta `<img src=''>`:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Abra el archivo `AdventureDetail.js` en `react-app/src/components/AdventureDetail.js`.
1. Repita los mismos pasos para modificar la consulta de GraphQL y agregue la propiedad `_publishUrl` para la aventura

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

1. Modifique las dos etiquetas `<img>` de la imagen principal de la aventura y la referencia de la imagen del colaborador en `AdventureDetail.js`:

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

1. Vaya a [http://localhost:5000/](http://localhost:5000/) y observe que las imágenes aparecen y que el atributo `<img src''>` señala a `http://localhost:4503`.

   ![Se corrigieron imágenes rotas](assets/publish-deployment/broken-images-fixed.png)

## Simular publicación de contenido {#content-publish}

Recuerde que se produce un error de GraphQL para `adventureContributor` cuando se solicita una página de detalles de aventura. El modelo de fragmento de contenido **Contributor** aún no existe en la instancia de publicación. Las actualizaciones realizadas en el modelo de fragmento de contenido **Adventure** tampoco están disponibles en la instancia de publicación. Estos cambios se realizaron directamente en la instancia de autor y deben distribuirse en la instancia de publicación.

Esto es algo que hay que tener en cuenta a la hora de implementar nuevas actualizaciones en una aplicación que depende de actualizaciones en un fragmento de contenido o un modelo de fragmento de contenido.

A continuación, permite simular la publicación de contenido entre las instancias locales Author y Publish.

1. Inicie la instancia de autor (si no se ha iniciado ya) y vaya al Administrador de paquetes en [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Descargue el paquete [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) e instálelo mediante el Administrador de paquetes.

   Este paquete instala una configuración que permite a la instancia de autor publicar contenido en la instancia de publicación. Pasos manuales para [esta configuración se encuentra aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=es#content-distribution).

   >[!NOTE]
   >
   > En un entorno de AEM as a Cloud Service, el nivel de Author se configura automáticamente para distribuir contenido al de Publish.

1. En el menú **Inicio de AEM**, vaya a **Herramientas** > **Assets** > **Modelos de fragmentos de contenido**.

1. Haga clic en la carpeta **WKND Site**.

1. Seleccione los tres modelos y haga clic en **Publicar**:

   ![Publicar modelos de fragmentos de contenido](assets/publish-deployment/publish-contentfragment-models.png)

   Aparecerá un cuadro de diálogo de confirmación, haga clic en **Publicar**.

1. Vaya al fragmento de contenido Bali Surf Camp en [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Haga clic en el botón **Publicar** en la barra de menú superior.

   ![Haga clic en el botón Publicar en el editor de fragmentos de contenido](assets/publish-deployment/publish-bali-content-fragment.png)

1. El asistente Publicar muestra todos los recursos dependientes que deben publicarse. En este caso, se enumera el fragmento **stacey-roswells** al que se hace referencia y también se hace referencia a varias imágenes. Los recursos a los que se hace referencia se publican junto con el fragmento.

   ![Se hace referencia a Assets para publicar](assets/publish-deployment/referenced-assets.png)

   Vuelva a hacer clic en el botón **Publicar** para publicar el fragmento de contenido y los recursos dependientes.

1. Vuelva a la aplicación React que se ejecuta en [http://localhost:5000/](http://localhost:5000/). Ahora puede hacer clic en el Bali Surf Camp para ver los detalles de la aventura.

1. Vuelva a la instancia de autor de AEM en [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) y actualice el **Título** del fragmento. **Guardar y cerrar** el fragmento. A continuación, **publique** el fragmento.
1. Vuelva a [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) y observe los cambios publicados.

   ![Actualización de publicación del Campamento de Surf de Bali](assets/publish-deployment/bali-surf-camp-update.png)

## Actualizar configuración de COR

AEM es seguro de forma predeterminada y no permite que las propiedades web que no son de AEM realicen llamadas del lado del cliente. La configuración de Intercambio de recursos de origen cruzado (CORS) de AEM puede permitir que dominios específicos realicen llamadas a AEM.

A continuación, experimente con la configuración CORS de la instancia de publicación de AEM.

1. Vuelva a la ventana del terminal donde se está ejecutando la aplicación React con el comando `npm run serve`:

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

   Tenga en cuenta que se proporcionan dos direcciones URL. Uno con `localhost` y otro con la dirección IP de red local.

1. Vaya a la dirección que comienza por [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). La dirección es ligeramente diferente para cada equipo local. Observe que hay un error CORS al recuperar los datos. Esto se debe a que la configuración actual de CORS solo permite solicitudes de `localhost`.

   ![Error CORS](assets/publish-deployment/cors-error-not-fetched.png)

   A continuación, actualice la configuración CORS de publicación de AEM para permitir solicitudes desde la dirección IP de red.

1. Vaya a [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) e inicie sesión con el nombre de usuario `admin` y la contraseña `admin`.

1. Vaya a [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) y busque la configuración de WKND GraphQL en `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Actualice el campo **Orígenes permitidos** para incluir la dirección IP de red:

   ![Actualizar configuración de CORS](assets/publish-deployment/cors-update.png)

   También es posible incluir una expresión regular para permitir todas las solicitudes de un subdominio específico. Guarde los cambios.

1. Busque **Filtro de referente de Apache Sling** y revise la configuración. La configuración **Permitir vacío** también es necesaria para habilitar las solicitudes de GraphQL desde un dominio externo.

   ![Filtro de remitente del reenvío de Sling](assets/publish-deployment/sling-referrer-filter.png)

   Se han configurado como parte del sitio de referencia de WKND. Puede ver el conjunto completo de configuraciones de OSGi a través de [el repositorio de GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > Las configuraciones de OSGi se administran en un proyecto de AEM comprometido con el control de código fuente. Un proyecto de AEM se puede implementar en entornos de AEM as a Cloud Service mediante Cloud Manager. El [tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) puede ayudar a generar un proyecto para una implementación específica.

1. Vuelva a la aplicación React a partir de [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) y observe que la aplicación ya no genera un error CORS.

   ![Error CORS corregido](assets/publish-deployment/cors-error-corrected.png)

## Enhorabuena. {#congratulations}

¡Enhorabuena! Ahora ha simulado una implementación de producción completa utilizando un entorno de publicación de AEM. También ha aprendido a utilizar la configuración CORS en AEM.

## Otros recursos

Para obtener más información sobre los fragmentos de contenido y GraphQL, consulte los siguientes recursos:

* [Entrega de contenido sin encabezado mediante fragmentos de contenido con GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=es)
* [API de GraphQL de AEM para su uso con fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=es)
* [Autenticación basada en token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=es#authentication)
* [Implementando código en AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=es#cloud-manager)

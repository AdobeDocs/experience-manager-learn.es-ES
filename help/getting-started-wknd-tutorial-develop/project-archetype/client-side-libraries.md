---
title: Bibliotecas de cliente y flujo de trabajo front-end
description: Aprenda a utilizar las bibliotecas de cliente para implementar y administrar la implementación de CSS y JavaScript para una implementación de sitios de Adobe Experience Manager (AEM). Descubra cómo el módulo ui.frontend, un proyecto de Webpack, se puede integrar en el proceso de compilación de extremo a extremo.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2432'
ht-degree: 1%

---

# Bibliotecas de cliente y flujo de trabajo front-end {#client-side-libraries}

Descubra cómo se utilizan las bibliotecas del lado del cliente o clientlibs para implementar y administrar CSS y JavaScript para una implementación de sitios de Adobe Experience Manager (AEM). Este tutorial también explica cómo se puede integrar el módulo [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=es), un proyecto [webpack](https://webpack.js.org/) disociado, en el proceso de compilación de extremo a extremo.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar el tutorial [Conceptos básicos de componentes](component-basics.md#client-side-libraries) para comprender los aspectos básicos de las bibliotecas del lado del cliente y AEM.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la rama `tutorial/client-side-libraries-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implemente una base de código en una instancia de AEM local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o desprotegerlo localmente cambiando a la rama `tutorial/client-side-libraries-solution`.

## Objetivos

1. Comprenda cómo se incluyen las bibliotecas del lado del cliente en una página mediante una plantilla editable.
1. Aprenda a utilizar el módulo `ui.frontend` y un servidor de desarrollo de Webpack para el desarrollo front-end dedicado.
1. Comprenda el flujo de trabajo completo de ofrecer CSS y JavaScript compilados a una implementación de Sites.

## Lo que va a generar {#what-build}

En este capítulo, agrega algunos estilos de línea de base para el sitio WKND y la plantilla de página de artículo para acercar la implementación a las [maquetas de diseño de interfaz de usuario](assets/pages-templates/wknd-article-design.xd). Utilice un flujo de trabajo front-end avanzado para integrar un proyecto de Webpack en una biblioteca de cliente de AEM.

![Estilos finalizados](assets/client-side-libraries/finished-styles.png)

*Página de artículo con estilos de línea de base aplicados*

## Fondo {#background}

Las bibliotecas del lado del cliente proporcionan un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. Los objetivos básicos de las bibliotecas del lado del cliente o clientlibs son:

1. Almacene CSS/JS en pequeños archivos discretos para facilitar el desarrollo y mantenimiento
1. Administrar dependencias en marcos de terceros de forma organizada
1. Minimice el número de solicitudes del lado del cliente concatenando CSS/JS en una o dos solicitudes.

Encontrará más información sobre el uso de [Bibliotecas del lado del cliente aquí.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=es)

Las bibliotecas del lado del cliente tienen algunas limitaciones. Lo más destacable es la compatibilidad limitada con los lenguajes de front-end populares como Sass, LESS y TypeScript. En el tutorial, veamos cómo el módulo **ui.frontend** puede ayudar a resolver esto.

Implemente el código de inicio base en una instancia local de AEM y vaya a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Esta página no tiene estilo. Implementemos bibliotecas del lado del cliente para la marca WKND para agregar CSS y JavaScript a la página.

## Organización de bibliotecas del cliente {#organization}

A continuación, vamos a explorar la organización de clientlibs generados por el [tipo de archivo del proyecto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es).

![Organización de la biblioteca de clientes de alto nivel](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nivel de la organización del cliente y la inclusión de páginas*

>[!NOTE]
>
> La siguiente organización de la biblioteca del lado del cliente la genera el tipo de archivo del proyecto de AEM, pero representa simplemente un punto de partida. La forma en que un proyecto administra y ofrece CSS y JavaScript en última instancia a una implementación de Sites puede variar considerablemente en función de los recursos, los conjuntos de habilidades y los requisitos.

1. Si usa VSCode u otro IDE, abra el módulo **ui.apps**.
1. Expanda la ruta `/apps/wknd/clientlibs` para ver los clientlibs generados por el tipo de archivo.

   ![Clientlibs en ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   En la sección siguiente, estos clientlibs se revisan con más detalle.

1. La siguiente tabla resume las bibliotecas de cliente. Encontrará más detalles sobre [incluidas las bibliotecas de clientes aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=es#developing).

   | Nombre | Descripción | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nivel base de CSS y JavaScript necesario para que funcione el sitio WKND | incrusta las bibliotecas de cliente de componentes principales |
   | `clientlib-grid` | Genera el CSS necesario para que funcione [Modo de diseño](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=es). | Los puntos de interrupción para móviles o tabletas se pueden configurar aquí |
   | `clientlib-site` | Contiene el tema específico del sitio para el sitio WKND | Generado por el módulo `ui.frontend` |
   | `clientlib-dependencies` | Incrusta cualquier dependencia de terceros | Generado por el módulo `ui.frontend` |

1. Observe que `clientlib-site` y `clientlib-dependencies` se omiten desde el control de código fuente. Esto es por diseño, ya que se generan en tiempo de compilación por el módulo `ui.frontend`.

## Actualizar estilos base {#base-styles}

A continuación, actualice los estilos base definidos en el módulo **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=es)**. Los archivos del módulo `ui.frontend` generan las bibliotecas `clientlib-site` y `clientlib-dependecies` que contienen el tema del sitio y cualquier dependencia de terceros.

Las bibliotecas del lado cliente no admiten idiomas más avanzados como [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Hay varias herramientas de código abierto como [NPM](https://www.npmjs.com/) y [webpack](https://webpack.js.org/) que aceleran y optimizan el desarrollo front-end. El objetivo del módulo **ui.frontend** es poder usar estas herramientas para administrar la mayoría de los archivos de origen front-end.

1. Abra el módulo **ui.frontend** y vaya a `src/main/webpack/site`.
1. Abrir el archivo `main.scss`

   ![main.scss - punto de entrada](assets/client-side-libraries/main-scss.png)

   `main.scss` es el punto de entrada a los archivos Sass en el módulo `ui.frontend`. Incluye el archivo `_variables.scss`, que contiene una serie de variables de marca que se utilizarán en distintos archivos Sass del proyecto. El archivo `_base.scss` también se incluye y define algunos estilos básicos para los elementos de HTML. Una expresión regular incluye los estilos para los estilos de componentes individuales en `src/main/webpack/components`. Otra expresión regular incluye los archivos bajo `src/main/webpack/site/styles`.

1. Inspeccione el archivo `main.ts`. Incluye `main.scss` y una expresión regular para recopilar cualquier archivo de `.js` o `.ts` en el proyecto. Los [archivos de configuración del Webpack](https://webpack.js.org/configuration/) utilizan este punto de entrada como punto de entrada para todo el módulo `ui.frontend`.

1. Inspeccione los archivos por debajo de `src/main/webpack/site/styles`:

   ![Archivos de estilo](assets/client-side-libraries/style-files.png)

   Estos archivos contienen estilos para elementos globales de la plantilla, como el encabezado, el pie de página y el contenedor de contenido principal. Las reglas CSS de estos archivos se dirigen a diferentes elementos de HTML `header`, `main` y `footer`. Estos elementos de HTML fueron definidos por directivas del capítulo anterior [Páginas y plantillas](./pages-templates.md).

1. Expanda la carpeta `components` en `src/main/webpack` e inspeccione los archivos.

   ![Archivos Sass de componente](assets/client-side-libraries/component-sass-files.png)

   Cada archivo se asigna a un componente principal como el [componente de acordeón](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=es). Cada componente principal se crea con la notación [Modificador de elemento de bloque](https://getbem.com/) o BEM para facilitar la segmentación de clases CSS específicas con reglas de estilo. El tipo de archivo del proyecto de AEM ha tachado los archivos situados debajo de `/components` con las distintas reglas de BEM para cada componente.

1. Descargue los estilos base de WKND **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** y **descomprima** el archivo.

   ![Estilos base de WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar el tutorial, se proporcionan varios archivos Sass que implementan la marca WKND en función de los componentes principales y la estructura de la plantilla de página de artículo.

1. Sobrescribir el contenido de `ui.frontend/src` con archivos del paso anterior. El contenido del zip debe sobrescribir las siguientes carpetas:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Archivos modificados](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspeccione los archivos modificados para ver los detalles de la implementación de estilo WKND.

## Inspeccione la integración de ui.frontend {#ui-frontend-integration}

Elemento de integración clave integrado en el módulo **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), toma los artefactos CSS y JS compilados de un proyecto webpack/npm y los transforma en bibliotecas del lado del cliente de AEM.

![integración de arquitectura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

El tipo de archivo del proyecto de AEM configura automáticamente esta integración. A continuación, explore cómo funciona.


1. Abra un terminal de línea de comandos e instale el módulo **ui.frontend** con el comando `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >La ejecución de `npm install` solo se necesita una vez, como después de un nuevo clon o generación del proyecto.

1. Abra `ui.frontend/package.json` y en el comando **scripts** **start** agregue `--env writeToDisk=true`.

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. Inicie el servidor de desarrollo de Webpack en modo **watch** ejecutando el siguiente comando:

   ```shell
   $ npm run watch
   ```

1. Esto compila los archivos de origen del módulo `ui.frontend` y sincroniza los cambios con AEM en [http://localhost:4502](http://localhost:4502)

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. El comando `npm run watch` rellena finalmente **clientlib-site** y **clientlib-dependencies** en el módulo **ui.apps**, que luego se sincroniza automáticamente con AEM.

   >[!NOTE]
   >
   >También hay un perfil `npm run prod` que minimiza el JS y el CSS. Esta es la compilación estándar cada vez que la generación del Webpack se activa mediante Maven. Encontrará más detalles sobre el módulo [ui.frontend aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=es).

1. Inspeccione el archivo `site.css` debajo de `ui.frontend/dist/clientlib-site/site.css`. Este es el CSS compilado basado en los archivos fuente Sass.

   ![css de sitio distribuido](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspeccione el archivo `ui.frontend/clientlib.config.js`. Este es el archivo de configuración para un complemento npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator), que transforma el contenido de `/dist` en una biblioteca de cliente y la mueve al módulo `ui.apps`.

1. Inspeccione el archivo `site.css` en el módulo **ui.apps** en `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Debe ser una copia idéntica del archivo `site.css` del módulo **ui.frontend**. Ahora que está en el módulo **ui.apps**, se puede implementar en AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Dado que **clientlib-site** se compila durante el tiempo de compilación, usando **npm** o **maven**, se puede ignorar de forma segura desde el control de código fuente en el módulo **ui.apps**. Inspeccione el archivo de `.gitignore` debajo de **ui.apps**.

1. Abra el artículo de LA Skatepark en AEM en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos base actualizados para el artículo](assets/client-side-libraries/updated-base-styles.png)

   Ahora debería ver los estilos actualizados para el artículo. Es posible que tenga que realizar una actualización brusca para borrar cualquier archivo CSS almacenado en caché por el explorador.

   ¡Está empezando a mirar mucho más cerca de las maquetas!

   >[!NOTE]
   >
   > Los pasos realizados anteriormente para crear e implementar el código ui.frontend en AEM se ejecutan automáticamente cuando se activa una compilación de Maven desde la raíz del proyecto `mvn clean install -PautoInstallSinglePackage`.

## Realizar un cambio de estilo

A continuación, realice un pequeño cambio en el módulo `ui.frontend` para ver cómo `npm run watch` implementa automáticamente los estilos en la instancia local de AEM.

1. En, el módulo `ui.frontend` abre el archivo: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Actualizar la variable de color `$brand-primary`:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Guarde los cambios.

1. Vuelva al explorador y actualice la página de AEM para ver las actualizaciones:

   ![Bibliotecas del cliente](assets/client-side-libraries/style-update-brand-primary.png)

1. Revierta el cambio al color `$brand-primary` y detenga la compilación del Webpack usando el comando `CTRL+C`.

>[!CAUTION]
>
> Puede que no sea necesario el uso del módulo **ui.frontend** para todos los proyectos. El módulo **ui.frontend** agrega complejidad adicional y si no hay necesidad o deseo de usar algunas de estas herramientas avanzadas del front-end (Sass, webpack, npm...) es posible que no sea necesario.

## Inclusión de página y plantilla {#page-inclusion}

A continuación, vamos a revisar cómo se hace referencia a los clientlibs en la página de AEM. Una práctica recomendada común en el desarrollo web es incluir CSS en el encabezado de HTML `<head>` y JavaScript justo antes de cerrar la etiqueta `</body>`.

1. Vaya a la plantilla de la página de artículos en [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Haga clic en el icono **Información de la página** y, en el menú, seleccione **Política de la página** para abrir el cuadro de diálogo **Política de la página**.

   ![Directiva de página de menú de plantilla de artículo](assets/client-side-libraries/template-page-policy.png)

   *Información de página > Directiva de página*

1. Observe que las categorías de `wknd.dependencies` y `wknd.site` se muestran aquí. De forma predeterminada, los clientlibs configurados mediante la directiva de página se dividen para incluir el CSS en el encabezado de la página y el JavaScript en el final del cuerpo. Puede enumerar explícitamente la JavaScript clientlib que se cargará en el encabezado de la página. Este es el caso de `wknd.dependencies`.

   ![Directiva de página de menú de plantilla de artículo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > También es posible hacer referencia a `wknd.site` o `wknd.dependencies` directamente desde el componente de página, utilizando el script `customheaderlibs.html` o `customfooterlibs.html`. El uso de la plantilla ofrece flexibilidad para que pueda elegir qué clientlibs se utilizan por plantilla. Por ejemplo, si tiene una biblioteca JavaScript pesada que solo se va a utilizar en una plantilla seleccionada.

1. Vaya a la página de **LA Skateparks** creada con la **plantilla de página de artículo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Haga clic en el icono **Información de la página** y, en el menú, seleccione **Ver como publicado** para abrir la página del artículo fuera del Editor de AEM.

   ![Ver como aparece publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Vea el origen de página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) y debería poder ver las siguientes referencias clientlib en `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que los clientlibs están usando el extremo del proxy `/etc.clientlibs`. También debe ver que la siguiente clientlib incluye en la parte inferior de la página:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > En AEM 6.5/6.4, las bibliotecas del lado del cliente no se minifican automáticamente. Consulte la documentación sobre [HTML Library Manager para habilitar la minificación (recomendado)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=es#using-preprocessors).

   >[!WARNING]
   >
   >Es fundamental en la publicación que las bibliotecas de cliente **no** se proporcionen desde **/aplicaciones**, ya que esta ruta debe restringirse por motivos de seguridad mediante la [sección de filtros de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es#example-filter-section). La propiedad [allowProxy](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=es#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la biblioteca de cliente garantiza que CSS y JS se proporcionen desde **/etc.clientlibs**.

### Siguientes pasos {#next-steps}

Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. [El desarrollo con el sistema de estilos](style-system.md) cubre el uso del sistema de estilos para ampliar los componentes principales con CSS específicos de la marca y configuraciones de directivas avanzadas del editor de plantillas.

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/client-side-libraries-solution`.

1. Clonar el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consulte la rama `tutorial/client-side-libraries-solution`.

## Herramientas y recursos adicionales {#additional-resources}

### Webpack DevServer: marcado estático {#webpack-dev-static}

En el par de ejercicios anteriores, se actualizaron varios archivos Sass en el módulo **ui.frontend** y, a través de un proceso de compilación, finalmente verá que estos cambios se reflejaron en AEM. A continuación, veamos una técnica que usa un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente los estilos front-end con **static** HTML.

Esta técnica es práctica si la mayoría de los estilos y el código front-end los realiza un desarrollador de front-end dedicado que puede no tener acceso fácil a un entorno de AEM. Esta técnica también permite a la FED realizar modificaciones directamente en la HTML, que luego se puede entregar a un desarrollador de AEM para que las implemente como componentes.

1. Copie el origen de página de la página de artículos del parque de patinaje LA en [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Vuelva a abrir el IDE. Pegue el marcado copiado de AEM en `index.html`, en el módulo **ui.frontend** debajo de `src/main/webpack/static`.
1. Edite el marcado copiado y quite cualquier referencia a **clientlib-site** y **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Elimine estas referencias porque el servidor de desarrollo de Webpack genera estos artefactos automáticamente.

1. Inicie el servidor de desarrollo de Webpack desde un nuevo terminal ejecutando el siguiente comando desde el módulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Esto debería abrir una nueva ventana del explorador en [http://localhost:8080/](http://localhost:8080/) con marcado estático.

1. Edite el archivo `src/main/webpack/site/_variables.scss`. Reemplace la regla `$text-color` con lo siguiente:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Guarde los cambios.

1. Debería ver automáticamente los cambios que se reflejan automáticamente en el explorador en [http://localhost:8080](http://localhost:8080).

   ![Cambios en el servidor de desarrollo de Webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Revise el archivo `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contiene la configuración del Webpack utilizada para iniciar el webpack-dev-server. Proporciona las rutas de acceso `/content` y `/etc.clientlibs` desde una instancia de AEM que se ejecuta localmente. Así es como están disponibles las imágenes y otros clientlibs (no administrados por el código **ui.frontend**).

   >[!CAUTION]
   >
   > El origen de la imagen del marcado estático apunta a un componente de imagen en directo en una instancia de AEM local. Las imágenes aparecen rotas si cambia la ruta a la imagen, si AEM no se ha iniciado o si el explorador no ha iniciado sesión en la instancia local de AEM. Si se transfiere a un recurso externo, también es posible reemplazar las imágenes con referencias estáticas.

1. Puede **detener** el servidor de Webpack desde la línea de comandos escribiendo `CTRL+C`.

### Depuración de bibliotecas del lado del cliente {#debugging-clientlibs}

Puede resultar difícil solucionar problemas si utiliza diferentes métodos de **categories** y **embeds** para incluir varias bibliotecas de cliente. AEM expone varias herramientas para ayudarle con esto. Una de las herramientas más importantes es **Reconstruir bibliotecas de cliente**, lo que obliga a AEM a volver a compilar los archivos LESS y generar el CSS.

* [**Bibliotecas de volcado**](http://localhost:4502/libs/granite/ui/content/dumplibs.html): enumera las bibliotecas de cliente registradas en la instancia de AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Salida de prueba**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html): permite al usuario ver la salida esperada de HTML de clientlib includes según la categoría. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validación de dependencias de bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html): resalta las dependencias o categorías incrustadas que no se pueden encontrar. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruir bibliotecas de cliente**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html): permite al usuario forzar a AEM a reconstruir las bibliotecas de cliente o invalidar la caché de las bibliotecas de cliente. Esta herramienta es eficaz cuando se desarrolla con LESS, ya que esto puede obligar a AEM a recompilar el CSS generado. En general, es más eficaz Invalidar cachés y luego actualizar la página que reconstruir las bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruir biblioteca de cliente](assets/client-side-libraries/rebuild-clientlibs.png)

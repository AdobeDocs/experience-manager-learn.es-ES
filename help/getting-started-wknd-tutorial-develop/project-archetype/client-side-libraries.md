---
title: Bibliotecas de cliente y flujo de trabajo front-end
description: Aprenda a utilizar las bibliotecas de cliente para implementar y administrar la implementación de CSS y JavaScript para una implementación de Adobe Experience Manager AEM () Sites. Descubra cómo el módulo ui.frontend, un proyecto de Webpack, se puede integrar en el proceso de compilación de extremo a extremo.
version: 6.4, 6.5, Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 732
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2546'
ht-degree: 0%

---

# Bibliotecas de cliente y flujo de trabajo front-end {#client-side-libraries}

Descubra cómo se utilizan las bibliotecas del lado del cliente o clientlibs para implementar y administrar la implementación de CSS y JavaScript para una implementación de sitios de Adobe Experience Manager AEM (). Este tutorial también explica cómo [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) módulo, un módulo desacoplado [webpack](https://webpack.js.org/) proyecto, se puede integrar en el proceso de compilación de extremo a extremo.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar el [Conceptos básicos de componentes](component-basics.md#client-side-libraries) AEM tutorial para comprender los aspectos básicos de las bibliotecas del lado del cliente y las bibliotecas de.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la `tutorial/client-side-libraries-start` bifurcar desde [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. AEM Implemente una base de código en una instancia de local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM Si se utiliza la versión 6.5 o 6.4, se debe anexar la variable `classic` de perfil a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o compruebe el código localmente cambiando a la rama `tutorial/client-side-libraries-solution`.

## Objetivos

1. Comprenda cómo se incluyen las bibliotecas del lado del cliente en una página mediante una plantilla editable.
1. Aprenda a utilizar el `ui.frontend` y un servidor de desarrollo de Webpack para el desarrollo front-end dedicado.
1. Comprender el flujo de trabajo completo de entrega de CSS compilado y JavaScript a una implementación de Sites.

## Lo que va a generar {#what-build}

En este capítulo, se agregan algunos estilos de línea de base para el sitio WKND y la plantilla de página de artículo para acercar la implementación al [Maquetas de diseño de IU](assets/pages-templates/wknd-article-design.xd). AEM Puede utilizar un flujo de trabajo front-end avanzado para integrar un proyecto de Webpack en una biblioteca de cliente de.

![Estilos finalizados](assets/client-side-libraries/finished-styles.png)

*Página de artículo con estilos de línea de base aplicados*

## Fondo {#background}

Las bibliotecas del lado del cliente proporcionan un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. Los objetivos básicos de las bibliotecas del lado del cliente o clientlibs son:

1. Almacene CSS/JS en pequeños archivos discretos para facilitar el desarrollo y mantenimiento
1. Administrar dependencias en marcos de terceros de forma organizada
1. Minimice el número de solicitudes del lado del cliente concatenando CSS/JS en una o dos solicitudes.

Más información sobre el uso de [Las bibliotecas del lado del cliente se pueden encontrar aquí.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

Las bibliotecas del lado del cliente tienen algunas limitaciones. Lo más destacable es la compatibilidad limitada con los lenguajes de front-end populares como Sass, LESS y TypeScript. En el tutorial, veamos cómo se usa la variable **ui.frontend** Este módulo puede ayudar a resolver esto.

AEM Implemente la base del código de inicio en una instancia de local y navegue hasta [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Esta página no tiene estilo. Implementemos bibliotecas del lado del cliente para la marca WKND para agregar CSS y JavaScript a la página.

## Organización de bibliotecas del cliente {#organization}

A continuación, vamos a explorar la organización de clientlibs generados por [AEM Tipo de archivo del proyecto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=es).

![Organización de bibliotecas de clientes de alto nivel](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nivel Organización de la biblioteca del lado del cliente e inclusión de páginas*

>[!NOTE]
>
> AEM La siguiente organización de la biblioteca del lado del cliente se genera mediante el tipo de archivo del proyecto, pero representa simplemente un punto de partida. La forma en que un proyecto administra y ofrece CSS y JavaScript en última instancia a una implementación de Sites puede variar considerablemente en función de los recursos, los conjuntos de habilidades y los requisitos.

1. Con VSCode u otro IDE, abra el **ui.apps** módulo.
1. Expandir la ruta `/apps/wknd/clientlibs` para ver los clientlibs generados por el tipo de archivo.

   ![Clientlibs en ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   En la sección siguiente, estos clientlibs se revisan con más detalle.

1. La siguiente tabla resume las bibliotecas de cliente. Más detalles acerca de [Puede encontrar información sobre las bibliotecas de cliente aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nombre | Descripción | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nivel base de CSS y JavaScript necesario para que funcione el sitio WKND | incrusta las bibliotecas de cliente de componentes principales |
   | `clientlib-grid` | Genera el archivo CSS necesario para [Modo de diseño](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) para trabajar. | Los puntos de interrupción para móviles o tabletas se pueden configurar aquí |
   | `clientlib-site` | Contiene el tema específico del sitio para el sitio WKND | Generado por el `ui.frontend` módulo |
   | `clientlib-dependencies` | Incrusta cualquier dependencia de terceros | Generado por el `ui.frontend` módulo |

1. Observe que `clientlib-site` y `clientlib-dependencies` se omiten desde el control de código fuente. Esto es por diseño, ya que se generan en el momento de la compilación mediante `ui.frontend` módulo.

## Actualizar estilos base {#base-styles}

A continuación, actualice los estilos base definidos en la variable **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** módulo. Los archivos en la `ui.frontend` módulo genera el `clientlib-site` y `clientlib-dependecies` bibliotecas que contienen el tema del sitio y cualquier dependencia de terceros.

Las bibliotecas del lado del cliente no admiten lenguajes más avanzados como [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Hay varias herramientas de código abierto como [NPM](https://www.npmjs.com/) y [webpack](https://webpack.js.org/) que aceleran y optimizan el desarrollo front-end. El objetivo del **ui.frontend** es poder utilizar estas herramientas para administrar la mayoría de los archivos de origen front-end.

1. Abra el **ui.frontend** y vaya a `src/main/webpack/site`.
1. Abra el archivo `main.scss`

   ![main.scss: entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` es el punto de entrada a los archivos Sass en la `ui.frontend` módulo. Incluye el `_variables.scss` , que contiene una serie de variables de marca que se utilizarán en los distintos archivos Sass del proyecto. El `_base.scss` también se incluye el archivo y define algunos estilos básicos para los elementos de HTML. Una expresión regular incluye los estilos de los componentes individuales en `src/main/webpack/components`. Otra expresión regular incluye los archivos en `src/main/webpack/site/styles`.

1. Inspect el archivo `main.ts`. Incluye `main.scss` y una expresión regular para recopilar cualquier `.js` o `.ts` archivos en el proyecto. Este punto de entrada lo utiliza la variable [archivos de configuración de webpack](https://webpack.js.org/configuration/) como punto de entrada para todo el `ui.frontend` módulo.

1. Inspect muestra los archivos debajo de `src/main/webpack/site/styles`:

   ![Archivos de estilo](assets/client-side-libraries/style-files.png)

   Estos archivos contienen estilos para elementos globales de la plantilla, como el encabezado, el pie de página y el contenedor de contenido principal. Las reglas CSS de estos archivos se dirigen a diferentes elementos de HTML `header`, `main`, y  `footer`. Estos elementos HTML se definieron mediante directivas en el capítulo anterior [Páginas y plantillas](./pages-templates.md).

1. Expanda el `components` carpeta bajo `src/main/webpack` e inspeccione los archivos.

   ![Archivos Sass de componentes](assets/client-side-libraries/component-sass-files.png)

   Cada archivo se asigna a un componente principal como [Componente Acordeón](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). Cada componente principal se crea con [Modificador de elemento de bloque](https://getbem.com/) o la notación de BEM para facilitar el direccionamiento de clases CSS específicas con reglas de estilo. Los archivos debajo de `/components` AEM han sido tachados por el tipo de archivo del proyecto de la con las diferentes reglas de BEM para cada componente.

1. Descargar los estilos base de WKND **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** y **abrir la cremallera** el archivo.

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

   Inspect muestra los archivos modificados para ver los detalles de la implementación de estilo WKND.

## Integración de Inspect con ui.frontend {#ui-frontend-integration}

Una pieza de integración clave integrada en el **ui.frontend** módulo, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) AEM toma los artefactos CSS y JS compilados de un proyecto webpack/npm y los transforma en bibliotecas del lado del cliente, que son las bibliotecas del cliente.

![integración de arquitectura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

AEM El tipo de archivo del proyecto de configura automáticamente esta integración. A continuación, explore cómo funciona.


1. Abra un terminal de línea de comandos e instale el **ui.frontend** mediante el módulo `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` La ejecución de solo se necesita una vez, como después de un nuevo clon o una nueva generación del proyecto.

1. Inicie el servidor de desarrollo de Webpack en **observar** mediante la ejecución del siguiente comando:

   ```shell
   $ npm run watch
   ```

1. Esto compila los archivos de origen de la variable `ui.frontend` AEM y sincroniza los cambios con los cambios de la manera de hacer un seguimiento de los cambios en el módulo de [http://localhost:4502](http://localhost:4502)

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

1. El comando `npm run watch` finalmente rellena el **clientlib-site** y **clientlib-dependencies** en el **ui.apps** AEM que luego se sincroniza automáticamente con el módulo de.

   >[!NOTE]
   >
   >También hay un `npm run prod` que minifica el JS y CSS. Esta es la compilación estándar cada vez que la generación del Webpack se activa mediante Maven. Más detalles acerca de [El módulo ui.frontend se puede encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect el archivo `site.css` debajo `ui.frontend/dist/clientlib-site/site.css`. Este es el CSS compilado basado en los archivos fuente Sass.

   ![Css de sitio distribuido](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect el archivo `ui.frontend/clientlib.config.js`. Este es el archivo de configuración para un complemento npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma el contenido de `/dist` en una biblioteca de cliente y la mueve a la `ui.apps` módulo.

1. Inspect el archivo `site.css` en el **ui.apps** módulo en `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Debe ser una copia idéntica de la `site.css` desde el **ui.frontend** módulo. Ahora que está en **ui.apps** AEM módulo para el que se puede implementar el módulo de forma.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Desde **clientlib-site** se compila durante el tiempo de compilación, utilizando **npm**, o **maven**, se puede ignorar de forma segura desde el control de código fuente en **ui.apps** módulo. Inspect el `.gitignore` archivo debajo **ui.apps**.

1. AEM Abra el artículo de LA Skatepark en la página de inicio de sesión en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Se han actualizado los estilos base del artículo](assets/client-side-libraries/updated-base-styles.png)

   Ahora debería ver los estilos actualizados para el artículo. Es posible que tenga que realizar una actualización brusca para borrar cualquier archivo CSS almacenado en caché por el explorador.

   ¡Está empezando a mirar mucho más cerca de las maquetas!

   >[!NOTE]
   >
   > AEM Los pasos realizados anteriormente para crear e implementar el código ui.frontend para la creación de páginas se ejecutan automáticamente cuando se activa una generación de Maven desde la raíz del proyecto de `mvn clean install -PautoInstallSinglePackage`.

## Realizar un cambio de estilo

A continuación, realice un pequeño cambio en `ui.frontend` para ver el `npm run watch` AEM implemente automáticamente los estilos en la instancia de la instancia local de.

1. Desde, la variable `ui.frontend` módulo abra el archivo: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Actualice el `$brand-primary` variable de color:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Guarde los cambios.

1. AEM Vuelva al explorador y actualice la página de la para ver las actualizaciones:

   ![Bibliotecas del cliente](assets/client-side-libraries/style-update-brand-primary.png)

1. Revertir el cambio al `$brand-primary` coloree y detenga la compilación del webpack con el comando `CTRL+C`.

>[!CAUTION]
>
> El uso del **ui.frontend** El módulo puede no ser necesario para todos los proyectos. El **ui.frontend** Este módulo añade complejidad adicional y si no hay necesidad/deseo de usar algunas de estas herramientas avanzadas del front-end (Sass, webpack, npm...) puede que no sea necesario.

## Inclusión de página y plantilla {#page-inclusion}

AEM A continuación, vamos a revisar cómo se hace referencia a los clientlibs en la página de la. Una práctica recomendada común en el desarrollo web es incluir CSS en el encabezado del HTML `<head>` y JavaScript justo antes de cerrar `</body>` etiqueta.

1. Navegue hasta la plantilla Página de artículo en [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Haga clic en **Información de página** y en el menú seleccione **Política de página** para abrir **Política de página** diálogo.

   ![Política de página de menú de plantilla de página de artículo](assets/client-side-libraries/template-page-policy.png)

   *Información de página > Política de página*

1. Observe que las categorías de `wknd.dependencies` y `wknd.site` se enumeran aquí. De forma predeterminada, los clientlibs configurados mediante la directiva de página se dividen para incluir el CSS en el encabezado de la página y el JavaScript en el final del cuerpo. Puede enumerar explícitamente el JavaScript clientlib que se cargará en el encabezado de la página. Este es el caso de `wknd.dependencies`.

   ![Política de página de menú de plantilla de página de artículo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > También es posible hacer referencia al `wknd.site` o `wknd.dependencies` desde el componente de página directamente, utilizando `customheaderlibs.html` o `customfooterlibs.html` script. El uso de la plantilla ofrece flexibilidad para que pueda elegir qué clientlibs se utilizan por plantilla. Por ejemplo, si tiene una biblioteca JavaScript pesada que solo se va a utilizar en una plantilla de selección.

1. Vaya a **LA Skateparks** página creada con el **Plantilla de página de artículo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Haga clic en **Información de página** y en el menú seleccione **Ver como aparece publicado** AEM para abrir la página del artículo fuera del Editor de la.

   ![Ver como aparece publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Ver el origen de página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) y debería poder ver las siguientes referencias clientlib en la `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que los clientlibs utilizan el proxy `/etc.clientlibs` punto final. También debe ver que la siguiente clientlib incluye en la parte inferior de la página:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > AEM Para la versión 6.5/6.4 de, las bibliotecas del lado del cliente no se minifican automáticamente. Consulte la documentación de la [Administrador de bibliotecas del HTML para habilitar la minificación (recomendado)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >En el lado de la publicación es fundamental que las bibliotecas de cliente estén **no** servido desde **/apps** ya que esta ruta debe restringirse por motivos de seguridad utilizando [Sección de filtro de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). El [propiedad allowProxy](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la biblioteca de cliente garantiza que CSS y JS se proporcionen desde **/etc.clientlibs**.

### Pasos siguientes {#next-steps}

Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. [Desarrollo con el sistema de estilos](style-system.md) cubre el uso del sistema de estilos para ampliar los componentes principales con CSS específicos de la marca y configuraciones de directiva avanzadas del editor de plantillas.

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/client-side-libraries-solution`.

1. Clonar el [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repositorio.
1. Consulte la `tutorial/client-side-libraries-solution` Rama.

## Herramientas y recursos adicionales {#additional-resources}

### Webpack DevServer: marcado estático {#webpack-dev-static}

En el par de ejercicios anteriores varios archivos Sass en el **ui.frontend** AEM Los módulos de se han actualizado y, a través de un proceso de compilación, finalmente observa que estos cambios se reflejan en los cambios de la versión de la aplicación de la versión en cuestión. A continuación, veamos una técnica que utiliza un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente estilos front-end con **estático** HTML.

AEM Esta técnica es práctica si la mayoría de los estilos y el código front-end los realiza un desarrollador de front-end dedicado que puede no tener acceso fácil a un entorno de. Esta técnica también permite a la FED hacer modificaciones directamente al HTML AEM, que luego pueden entregarse a un desarrollador de la FED para que las implemente como componentes.

1. Copie la fuente de página de la página de artículos de LA skatepark en [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Vuelva a abrir el IDE. AEM Pegue el marcado copiado de la página de inicio de la página de la página de inicio de sesión `index.html` en el **ui.frontend** módulo debajo de `src/main/webpack/static`.
1. Edite el marcado copiado y elimine todas las referencias a **clientlib-site** y **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Elimine estas referencias porque el servidor de desarrollo de Webpack genera estos artefactos automáticamente.

1. Inicie el servidor de desarrollo de Webpack desde un nuevo terminal ejecutando el siguiente comando desde el **ui.frontend** módulo:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Esto debería abrir una nueva ventana del explorador en [http://localhost:8080/](http://localhost:8080/) con marcado estático.

1. Editar el archivo `src/main/webpack/site/_variables.scss` archivo. Reemplace el `$text-color` con lo siguiente:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Guarde los cambios.

1. Debería ver automáticamente los cambios reflejados automáticamente en el explorador el [http://localhost:8080](http://localhost:8080).

   ![Cambios en el servidor de desarrollo de Webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Revise la `/aem-guides-wknd.ui.frontend/webpack.dev.js` archivo. Contiene la configuración del Webpack utilizada para iniciar el webpack-dev-server. Proporciona las rutas como proxy `/content` y `/etc.clientlibs` AEM desde una instancia de ejecución local de. Así es como se muestran las imágenes y otros clientlibs (no administrados por **ui.frontend** código) están disponibles.

   >[!CAUTION]
   >
   > AEM El origen de la imagen del marcado estático apunta a un componente de imagen en directo en una instancia de local. AEM AEM Las imágenes aparecen rotas si cambia la ruta a la imagen, si no se inicia la o si el explorador no ha iniciado sesión en la instancia de la aplicación local de. Si se transfiere a un recurso externo, también es posible reemplazar las imágenes con referencias estáticas.

1. Puede **parada** el servidor de Webpack desde la línea de comandos escribiendo `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** es una herramienta de línea de comandos de código abierto que se puede utilizar para acelerar el desarrollo front-end. Está alimentado por [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/), y [Seguimiento del registro de Sling](https://sling.apache.org/documentation/bundles/log-tracers.html).

En un nivel superior, la `aemfed`está diseñado para escuchar los cambios de archivo dentro de **ui.apps** AEM y los sincronizará automáticamente directamente con una instancia de la aplicación en ejecución de la aplicación de la aplicación de la. En función de los cambios, un explorador local se actualiza automáticamente, lo que acelera el desarrollo del front-end. También está diseñado para funcionar con el rastreador de registros de Sling a fin de mostrar automáticamente cualquier error del lado del servidor directamente en el terminal.

Si está haciendo mucho trabajo dentro de **ui.apps** , modificar scripts HTL y crear componentes personalizados, **aemfed** puede ser una herramienta potente para utilizar. [Puede encontrar toda la documentación aquí](https://github.com/abmaonline/aemfed).

### Depuración de bibliotecas del lado del cliente {#debugging-clientlibs}

Uso de diferentes métodos de **categorías** y **incrustar** para incluir varias bibliotecas de cliente, puede resultar engorroso solucionar problemas. AEM La expone varias herramientas para ayudarle con esto. Una de las herramientas más importantes es **Reconstruir bibliotecas de cliente** AEM lo que obliga a los usuarios a volver a compilar cualquier archivo LESS y generar el CSS.

* [**Volcar bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) AEM - Enumera las bibliotecas de cliente registradas en la instancia de. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Resultado de prueba**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permite al usuario ver la salida de HTML esperada de clientlib includes en función de la categoría. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validación de dependencias de bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : resalta las dependencias o categorías incrustadas que no se pueden encontrar. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruir bibliotecas de cliente**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) AEM : permite a un usuario forzar a los usuarios a que reconstruyan las bibliotecas de cliente o a que invaliden la caché de las bibliotecas de cliente. AEM Esta herramienta es eficaz cuando se desarrolla con LESS, ya que esto puede obligar a los usuarios a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y luego actualizar la página que reconstruir las bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![reconstruir biblioteca de cliente](assets/client-side-libraries/rebuild-clientlibs.png)

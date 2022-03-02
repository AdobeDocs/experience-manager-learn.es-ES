---
title: Bibliotecas del lado del cliente y flujo de trabajo del front-end
description: Descubra cómo se utilizan las bibliotecas del lado del cliente o clientlibs para implementar y administrar CSS y Javascript para una implementación de Adobe Experience Manager (AEM) Sites. Este tutorial también tratará cómo el módulo ui.frontend, un proyecto de webpack, se puede integrar en el proceso de compilación de extremo a extremo.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
source-git-commit: 1a73d7ee1f71a5bd78114398f04e98a894847957
workflow-type: tm+mt
source-wordcount: '2882'
ht-degree: 1%

---

# Bibliotecas del lado del cliente y flujo de trabajo del front-end {#client-side-libraries}

Descubra cómo se utilizan las bibliotecas del lado del cliente o clientlibs para implementar y administrar CSS y Javascript para una implementación de Adobe Experience Manager (AEM) Sites. Este tutorial también tratará sobre cómo se usa la variable [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) módulo, un desacoplado [webpack](https://webpack.js.org/) proyecto, se puede integrar en el proceso de compilación de extremo a extremo.

## Requisitos previos {#prerequisites}

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar la [Conceptos básicos de componentes](component-basics.md#client-side-libraries) tutorial para comprender los aspectos básicos de las bibliotecas y AEM del lado del cliente.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

Consulte el código de línea base sobre el que se basa el tutorial:

1. Consulte la `tutorial/client-side-libraries-start` ramificación desde [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implemente código base en una instancia de AEM local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, añada la variable `classic` perfil a cualquier comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o desproteja el código localmente cambiando a la rama `tutorial/client-side-libraries-solution`.

## Objetivos

1. Comprender cómo se incluyen las bibliotecas del lado del cliente en una página a través de una plantilla editable.
1. Aprenda a utilizar el módulo UI.Frontend y un servidor de desarrollo de webpack para el desarrollo de front-end dedicado.
1. Comprenda el flujo de trabajo completo de entrega de CSS y JavaScript compilados a una implementación de Sites.

## Qué va a generar {#what-you-will-build}

En este capítulo, agregará algunos estilos de línea de base para el sitio WKND y la plantilla de página de artículo en un esfuerzo por acercar la implementación a la [Fragmentos de diseño de la interfaz de usuario](assets/pages-templates/wknd-article-design.xd). Utilizará un flujo de trabajo front-end avanzado para integrar un proyecto de webpack en una biblioteca de cliente AEM.

![Estilos finalizados](assets/client-side-libraries/finished-styles.png)

*Artículo Página con estilos de línea de base aplicados*

## Fondo {#background}

Las bibliotecas del lado del cliente proporcionan un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. Los objetivos básicos para bibliotecas de cliente o clientlibs son:

1. Almacene CSS/JS en pequeños archivos discretos para facilitar el desarrollo y el mantenimiento
1. Administrar dependencias en marcos de terceros de forma organizada
1. Minimice el número de solicitudes del lado del cliente concatenando CSS/JS en una o dos solicitudes.

Más información sobre el uso de [Las bibliotecas del lado del cliente se pueden encontrar aquí.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

Las bibliotecas del lado del cliente tienen algunas limitaciones. Lo más notable es que el soporte para los lenguajes front-end más populares, como Sass, LESS y TypeScript es limitado. En el tutorial veremos cómo se llama a la función **ui.frontend** puede ayudar a resolver esto.

Implemente el código de inicio en una instancia de AEM local y vaya a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Esta página está actualmente sin estilo. A continuación, implementaremos bibliotecas del lado del cliente para la marca WKND para agregar CSS y Javascript a la página.

## Organización de bibliotecas del lado del cliente {#organization}

A continuación, exploraremos la organización de clientlibs generada por el [Tipo de archivo del proyecto AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![Organización clientlibrary de alto nivel](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nivel Organización de biblioteca del lado del cliente e inclusión de página*

>[!NOTE]
>
> El siguiente tipo de archivo del proyecto genera la siguiente organización de biblioteca del lado del cliente, pero representa simplemente un punto de partida. La forma en que un proyecto gestiona y entrega CSS y Javascript a una implementación de Sites puede variar considerablemente en función de los recursos, los conjuntos de habilidades y los requisitos.

1. Si utiliza VSCode u otro IDE, abra el **ui.apps** módulo.
1. Expandir la ruta `/apps/wknd/clientlibs` para ver los clientlibs generados por el tipo de archivo.

   ![Clientlibs en ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Inspeccionaremos estos clientlibs con buenos detalles a continuación.

1. La siguiente tabla resume las bibliotecas de cliente. Más detalles sobre [las bibliotecas de cliente se pueden encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nombre | Descripción | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nivel base de CSS y JavaScript necesarios para que el sitio WKND funcione | incrusta las bibliotecas de cliente de los componentes principales |
   | `clientlib-grid` | Genera la CSS necesaria para [Modo de diseño](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) para trabajar. | Los puntos de interrupción móviles/de Tablet se pueden configurar aquí |
   | `clientlib-site` | Contiene un tema específico del sitio para el sitio WKND | Generado por el `ui.frontend` módulo |
   | `clientlib-dependencies` | Incrusta cualquier dependencia de terceros | Generado por el `ui.frontend` módulo |

1. Observe que `clientlib-site` y `clientlib-dependencies` se ignoran del control de código fuente. Esto se hace por diseño, ya que se generarán en el momento de la compilación mediante la variable `ui.frontend` módulo.

## Actualizar estilos base {#base-styles}

A continuación, actualice los estilos base definidos en la variable **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** módulo. Los archivos de la variable `ui.frontend` generará la variable `clientlib-site` y `clientlib-dependecies` bibliotecas que contienen el tema del sitio y cualquier dependencia de terceros.

Las bibliotecas del lado del cliente tienen algunas limitaciones a la hora de admitir idiomas como [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Existen varias herramientas de código abierto como [NPM](https://www.npmjs.com/) y [webpack](https://webpack.js.org/) que aceleran y optimizan el desarrollo front-end. El objetivo de **ui.frontend** es poder utilizar estas herramientas para administrar la mayoría de los archivos de origen del front-end.

1. Abra el **ui.frontend** y vaya a `src/main/webpack/site`.
1. Abra el archivo `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)
client-side-library/main-scss

   `main.scss` es el punto de entrada a todos los archivos Sass de la variable `ui.frontend` módulo. Incluirá el `_variables.scss` , que contiene una serie de variables de marca que se utilizarán en los distintos archivos de Sass del proyecto. La variable `_base.scss` también se incluye y define algunos estilos básicos para los elementos del HTML. Una expresión regular incluye todos los estilos de los componentes individuales de `src/main/webpack/components`. Otra expresión regular incluye todos los archivos de `src/main/webpack/site/styles`.

1. Inspect el archivo `main.ts`. Incluye `main.scss` y una expresión regular para recopilar cualquier `.js` o `.ts` archivos en el proyecto. Este punto de entrada lo usará el [archivos de configuración de webpack](https://webpack.js.org/configuration/) como punto de entrada para todo el `ui.frontend` módulo.

1. Inspect los archivos situados debajo `src/main/webpack/site/styles`:

   ![Archivos de estilo](assets/client-side-libraries/style-files.png)

   Estos archivos son estilos para elementos globales en la plantilla, como el encabezado, el pie de página y el contenedor de contenido principal. Las reglas CSS de estos archivos se dirigen a diferentes elementos de HTML `header`, `main`y  `footer`. Estos elementos del HTML se definieron mediante políticas en el capítulo anterior [Páginas y plantillas](./pages-templates.md).

1. Expanda el `components` carpeta en `src/main/webpack` e inspeccione los archivos.

   ![Archivos Sass de componentes](assets/client-side-libraries/component-sass-files.png)

   Cada archivo se asigna a un componente principal como el [Componente Acordeón](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Cada componente principal se crea con [Modificador de elemento de bloque](https://getbem.com/) o notación BEM para facilitar la segmentación de clases CSS específicas con reglas de estilo. Los archivos situados debajo de `/components` el tipo de archivo del proyecto de AEM se ha atascado con las distintas reglas de BEM para cada componente.

1. Descargar los estilos base de WKND **[wknd-base-style-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** y **descomprimir** el archivo .

   ![Estilos base de WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar el tutorial, hemos proporcionado varios archivos Sass que implementan la marca WKND en función de los componentes principales y la estructura de la plantilla de página de artículos.

1. Sobrescribir el contenido de `ui.frontend/src` con archivos del paso anterior. El contenido del zip debe sobrescribir las siguientes carpetas:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Archivos modificados](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect los archivos modificados para ver los detalles de la implementación de estilo WKND.

## Inspect: integración de ui.frontend {#ui-frontend-integration}

Una pieza de integración clave integrada en la variable **ui.frontend** módulo, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) toma los artefactos CSS y JS compilados de un proyecto webpack/npm y los transforma en AEM bibliotecas del lado del cliente.

![integración de la arquitectura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

El tipo de archivo del proyecto AEM configura automáticamente esta integración. A continuación, explore cómo funciona.


1. Abra un terminal de línea de comandos e instale la **ui.frontend** usando el `npm install` comando:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` solo debe ejecutarse una vez, después de un nuevo clon o generación del proyecto.

1. Inicie el servidor de desarrollo de webpack en **watch** ejecutando el siguiente comando:

   ```shell
   $ npm run watch
   ```

1. Esto compilará la variable `src` los archivos de `ui.frontend` y sincronice los cambios con AEM en [http://localhost:4502](http://localhost:4502)

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

1. El comando `npm run watch` rellena finalmente la variable **clientlib-site** y **clientlib-dependencias** en el **ui.apps** módulo que se sincroniza automáticamente con AEM.

   >[!NOTE]
   >
   >También hay un `npm run prod` que minificará el JS y la CSS. Esta es la compilación estándar siempre que la compilación del webpack se activa mediante Maven. Más detalles sobre [ui.frontend módulo se puede encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect el archivo `site.css` below `ui.frontend/dist/clientlib-site/site.css`. Esta es la CSS compilada basada en los archivos de origen de Sass.

   ![Distributed Site css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect el archivo `ui.frontend/clientlib.config.js`. Este es el archivo de configuración para un complemento npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma el contenido de `/dist` en una biblioteca de cliente y la mueve a la `ui.apps` módulo.

1. Inspect el archivo `site.css` en el **ui.apps** módulo en `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Debe ser una copia idéntica de la variable `site.css` del **ui.frontend** módulo. Ahora está en **ui.apps** se puede implementar en AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Since **clientlib-site** se compila durante la compilación, con **npm** o **maven**, se puede ignorar de forma segura desde el control de código fuente en el **ui.apps** módulo. Inspect la variable `.gitignore` debajo de **ui.apps**.

1. Abra el artículo de LA Skatepark en AEM en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos base actualizados para el artículo](assets/client-side-libraries/updated-base-styles.png)

   Ahora debería ver los estilos actualizados para el artículo. Es posible que deba realizar una actualización difícil para borrar cualquier archivo CSS almacenado en caché por el explorador.

   ¡Está empezando a mirar mucho más cerca de las maquetas!

   >[!NOTE]
   >
   > Los pasos realizados anteriormente para crear e implementar el código ui.frontend en AEM se ejecutan automáticamente cuando se activa una compilación de Maven desde la raíz del proyecto `mvn clean install -PautoInstallSinglePackage`.

## Realizar un cambio de estilo

A continuación, realice un pequeño cambio en la `ui.frontend` para ver el `npm run watch` implemente automáticamente los estilos en la instancia de AEM local.

1. En el `ui.frontend` abra el archivo : `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Actualice el `$brand-primary` variable de color:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Guarde los cambios.

1. Vuelva al explorador y actualice la página AEM para ver las actualizaciones:

   ![Bibliotecas de cliente](assets/client-side-libraries/style-update-brand-primary.png)

1. Revertir el cambio a la variable `$brand-primary` coloree y detenga la compilación del webpack con el comando `CTRL+C`.

>[!CAUTION]
>
> El uso de **ui.frontend** puede no ser necesario para todos los proyectos. La variable **ui.frontend** agrega complejidad adicional y si no hay necesidad/deseo de usar algunas de estas herramientas avanzadas del front-end (Sass, webpack, npm...) puede que no sea necesario.

## Inclusión de plantilla y página {#page-inclusion}

A continuación, vamos a revisar cómo se hace referencia a clientlibs en la página AEM. Una práctica recomendada común en el desarrollo web es incluir CSS en el encabezado del HTML `<head>` y JavaScript justo antes de cerrar `</body>` etiqueta.

1. Vaya a la plantilla Página del artículo en [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Haga clic en el **Información de la página** y en el menú seleccione **Política de página** para abrir el **Política de página** diálogo.

   ![Política de página del menú Plantilla de página de artículo](assets/client-side-libraries/template-page-policy.png)

   *Información de página > Política de página*

1. Observe que las categorías para `wknd.dependencies` y `wknd.site` se enumeran aquí. De forma predeterminada, los clientlibs configurados mediante la Política de página se dividen para incluir el CSS en el encabezado de página y el JavaScript en el final del cuerpo. Si lo desea, puede enumerar explícitamente que el JavaScript clientlib se debe cargar en el encabezado de la página. Este es el caso de `wknd.dependencies`.

   ![Política de página del menú Plantilla de página de artículo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > También es posible hacer referencia al `wknd.site` o `wknd.dependencies` desde el componente de página directamente, utilizando la variable `customheaderlibs.html` o `customfooterlibs.html` como vimos anteriormente para el `wknd.base` clientlib. El uso de la plantilla le da cierta flexibilidad en cuanto a que puede elegir qué clientlibs se utilizan por plantilla. Por ejemplo, si tiene una biblioteca JavaScript muy pesada que solo se va a usar en una plantilla seleccionada.

1. Vaya a la **Parques de esquí LA** página creada con el **Plantilla de página de artículo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Haga clic en el **Información de la página** y en el menú seleccione **Ver tal y como aparece publicado** para abrir la página de artículos fuera del editor de AEM.

   ![Ver como aparece publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Ver el origen de la página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) y debería poder ver las siguientes referencias clientlib en la variable `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que clientlibs utiliza el proxy `/etc.clientlibs` punto final. También debería ver las siguientes clientlib incluidas en la parte inferior de la página:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Si sigue a la versión 6.5/6.4, las bibliotecas del lado del cliente no se minificarán automáticamente. Consulte la documentación de [Administrador de bibliotecas de HTML para habilitar la minización (recomendado)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Es fundamental en el lado de publicación que las bibliotecas del cliente sean **not** servido de **/apps** ya que esta ruta debe restringirse por motivos de seguridad usando la variable [Sección de filtro de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La variable [allowProxy, propiedad](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la biblioteca de cliente garantiza que CSS y JS se proporcionen desde **/etc.clientlibs**.

### Siguientes pasos {#next-steps}

Aprenda a implementar estilos individuales y a reutilizar componentes principales mediante el sistema de estilos de Experience Manager. [Desarrollo con el sistema de estilos](style-system.md) cubre el uso del sistema de estilos para ampliar los componentes principales con CSS específica de la marca y configuraciones de políticas avanzadas del Editor de plantillas.

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revisar e implementar el código localmente en la rama Git `tutorial/client-side-libraries-solution`.

1. Clonar el [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repositorio.
1. Consulte la `tutorial/client-side-libraries-solution` rama.

## Herramientas y recursos adicionales {#additional-resources}

### Servidor de desarrollo de Webpack: marcado estático {#webpack-dev-static}

En el anterior par de ejercicios pudimos actualizar varios archivos Sass en la **ui.frontend** y a través de un proceso de compilación, finalmente verán estos cambios reflejados en AEM. A continuación, analizaremos las técnicas que aprovechan un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) desarrollar rápidamente nuestros estilos front-end contra **static** HTML.

Esta técnica es útil si la mayoría de los estilos y el código front-end lo realiza un desarrollador de Front-End dedicado que puede no tener acceso fácil a un entorno AEM. Esta técnica también permite a la FED realizar modificaciones directamente al HTML, que luego se puede entregar a un desarrollador AEM para que las implemente como componentes.

1. Copie la fuente de la página del artículo de skatepark de LA en [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Vuelva a abrir el IDE. Pegue el marcado copiado de AEM en el `index.html` en el **ui.frontend** módulo debajo `src/main/webpack/static`.
1. Edite el marcado copiado y elimine las referencias a **clientlib-site** y **clientlib-dependencias**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Podemos eliminar esas referencias porque el servidor de desarrollo de webpack generará estos artefactos automáticamente.

1. Inicie el servidor de desarrollo de webpack desde un nuevo terminal ejecutando el siguiente comando desde el **ui.frontend** módulo:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Esto debería abrir una nueva ventana del explorador en [http://localhost:8080/](http://localhost:8080/) con marcado estático.

1. Editar el archivo `src/main/webpack/site/_variables.scss` archivo. Sustituya el `$text-color` con lo siguiente:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Guarde los cambios.

1. Debería ver automáticamente los cambios reflejados automáticamente en el explorador en [http://localhost:8080](http://localhost:8080).

   ![Cambios en el servidor de desarrollo de webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Consulte la `/aem-guides-wknd.ui.frontend/webpack.dev.js` archivo. Contiene la configuración del webpack utilizada para iniciar el webpack-dev-server. Tenga en cuenta que reemplaza las rutas `/content` y `/etc.clientlibs` desde una instancia de AEM que se esté ejecutando localmente. Así es como las imágenes y otros clientlibs (no administrados por la variable **ui.frontend** ) están disponibles.

   >[!CAUTION]
   >
   > La imagen src del marcado estático apunta a un componente de imagen en vivo en una instancia de AEM local. Las imágenes aparecerán rotas si la ruta a la imagen cambia, si no AEM se inicia o si el explorador no ha iniciado sesión en la instancia de AEM local. Si se entrega a un recurso externo, también es posible reemplazar las imágenes con referencias estáticas.

1. Puede **stop** el servidor webpack desde la línea de comandos escribiendo `CTRL+C`.

### aemfeed {#develop-aemfed}

[**aemfeed**](https://aemfed.io/) es una herramienta de código abierto de línea de comandos que puede utilizarse para acelerar el desarrollo del front-end. Está alimentado por  [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) y [Seguimiento de registros de Sling](https://sling.apache.org/documentation/bundles/log-tracers.html).

A un nivel superior **aemfeed** está diseñado para escuchar los cambios de archivos dentro del **ui.apps** y los sincroniza automáticamente directamente en una instancia de AEM en ejecución. En función de los cambios, un explorador local se actualizará automáticamente, acelerando así el desarrollo del front-end. También está construido para funcionar con el rastreador de registros de Sling para mostrar automáticamente cualquier error del lado del servidor directamente en el terminal.

Si está haciendo mucho trabajo dentro del **ui.apps** módulo, modificación de secuencias de comandos HTL y creación de componentes personalizados, **aemfeed** puede ser una herramienta muy poderosa para usar. [Puede encontrar la documentación completa aquí .](https://github.com/abmaonline/aemfed).

### Depuración de bibliotecas del lado del cliente {#debugging-clientlibs}

Con diferentes métodos de **categories** y **incrustaciones** para incluir varias bibliotecas de cliente, puede resultar complicado solucionar problemas. AEM expone varias herramientas para ayudarle con esto. Una de las herramientas más importantes es **Reconstruir bibliotecas de clientes** lo que obligará a AEM a volver a compilar cualquier archivo LESS y a generar el CSS.

* [**Libros volcados**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : enumera todas las bibliotecas de cliente registradas en la instancia de AEM. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Salida de prueba**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permite a un usuario ver la salida de HTML esperada de clientlib incluye en función de la categoría. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Bibliotecas Validación de dependencias**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : resalta las dependencias o categorías incrustadas que no se pueden encontrar. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruir bibliotecas de clientes**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : permite a un usuario forzar a AEM para reconstruir todas las bibliotecas de cliente o invalidar la caché de las bibliotecas de cliente. Esta herramienta es especialmente eficaz cuando se desarrolla con LESS, ya que esto puede forzar a AEM a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y luego realizar una actualización de página en comparación con la reconstrucción de todas las bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![regenerar biblioteca cliente](assets/client-side-libraries/rebuild-clientlibs.png)

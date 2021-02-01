---
title: Bibliotecas del cliente y flujo de trabajo front-end
description: Conozca cómo se utilizan las bibliotecas del lado del cliente o los clientes para implementar y administrar CSS y Javascript para una implementación de sitios de Adobe Experience Manager (AEM). Este tutorial también explicará cómo el módulo ui.frontender, un proyecto de webpack, se puede integrar en el proceso de generación de extremo a extremo.
sub-product: sitios
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3257'
ht-degree: 3%

---


# Bibliotecas del lado del cliente y flujo de trabajo front-end {#client-side-libraries}

Conozca cómo se utilizan las bibliotecas del lado del cliente o los clientes para implementar y administrar CSS y Javascript para una implementación de sitios de Adobe Experience Manager (AEM). Este tutorial también explicará cómo el módulo [ui.frontender](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/uifrontend.html), un proyecto de [webpack](https://webpack.js.org/) desacoplado, se puede integrar en el proceso de generación de extremo a extremo.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar el tutorial [Conceptos básicos del componente](component-basics.md#client-side-libraries) para comprender los aspectos fundamentales de las bibliotecas y AEM del lado del cliente.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

Consulte el código de línea base que el tutorial genera:

1. Consulte la rama `tutorial/client-side-libraries-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implemente código base en una instancia de AEM local con sus conocimientos Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o extraer el código localmente cambiando a la rama `tutorial/client-side-libraries-solution`.

## Objetivo

1. Comprender cómo se incluyen las bibliotecas del lado del cliente en una página a través de una plantilla editable.
1. Aprenda a utilizar el módulo UI.Frontender y un servidor de desarrollo de webpack para el desarrollo de front-end dedicado.
1. Comprender el flujo de trabajo completo de la entrega de CSS y JavaScript compilados a una implementación de sitios.

## Qué va a generar {#what-you-will-build}

En este capítulo, agregará algunos estilos de línea de base para el sitio WKND y la plantilla de página de artículos en un esfuerzo por acercar la implementación a las [maquetas de diseño de la interfaz de usuario](assets/pages-templates/wknd-article-design.xd). Utilizará un flujo de trabajo front-end avanzado para integrar un proyecto de webpack en una biblioteca cliente AEM.

![Estilos finalizados](assets/client-side-libraries/finished-styles.png)

*Página de artículos con estilos de línea de base aplicados*

## Fondo {#background}

Las bibliotecas del lado del cliente proporcionan un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. Los objetivos básicos de las bibliotecas o clientes son:

1. Almacene CSS/JS en pequeños archivos discretos para facilitar el desarrollo y el mantenimiento
1. Administrar las dependencias en marcos de terceros de forma organizada
1. Minimice el número de solicitudes de cliente concatenando CSS/JS en una o dos solicitudes.

Puede encontrar más información sobre el uso de [bibliotecas del lado del cliente aquí.](https://docs.adobe.com/content/help/es-ES/experience-manager-65/developing/introduction/clientlibs.html)

Las bibliotecas de cliente tienen algunas limitaciones. Lo más notable es que la compatibilidad con los idiomas principales más populares, como Sass, LESS y TypeScript, es limitada. En el tutorial veremos cómo el módulo **ui.front** puede ayudarle a solucionarlo.

Implemente la base de código de inicio en una instancia de AEM local y navegue a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Esta página no tiene estilo. A continuación, implementaremos bibliotecas de cliente para la marca WKND a fin de agregar CSS y Javascript a la página.

## Organización de bibliotecas del lado del cliente {#organization}

Luego exploraremos la organización de clientlibs generada por el [AEM Arquetipo de Proyecto](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/overview.html).

![Organización de biblioteca de clientes de alto nivel](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nivel Organización de la biblioteca del cliente e inclusión de páginas*

>[!NOTE]
>
> La siguiente organización de biblioteca del lado del cliente es generada por AEM arquetipo del proyecto pero representa simplemente un punto de partida. La forma en que un proyecto gestiona y envía CSS y Javascript en última instancia a una implementación de sitios puede variar considerablemente según los recursos, las habilidades y los requisitos.

1. Con VSCode u otro IDE, abra el módulo **ui.apps**.
1. Expanda la ruta `/apps/wknd/clientlibs` para vista de los clientlibs generados por el arquetipo.

   ![Clientlibs en ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Inspeccionaremos a estos clientes con buenos detalles a continuación.

1. La siguiente tabla resume las bibliotecas de cliente. Puede encontrar más detalles sobre [incluyendo las bibliotecas de cliente aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nombre | Descripción | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nivel base de CSS y JavaScript necesario para que el sitio WKND funcione | incrusta bibliotecas de cliente de componentes principales |
   | `clientlib-grid` | Genera la CSS necesaria para que funcione el [modo de diseño](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html). | Aquí se pueden configurar los puntos de interrupción de dispositivos móviles y tabletas |
   | `clientlib-site` | Contiene un tema específico del sitio para el sitio WKND | Generado por el módulo `ui.frontend` |
   | `clientlib-dependencies` | Incrusta cualquier dependencia de terceros | Generado por el módulo `ui.frontend` |

1. Observe que `clientlib-site` y `clientlib-dependencies` se omiten en el control de código fuente. Esto se hace por diseño, ya que el módulo `ui.frontend` los generará en tiempo de compilación.

## Actualizar estilos base {#base-styles}

A continuación, actualice los estilos base definidos en el módulo **[ui.frontender](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. Los archivos del módulo `ui.frontend` generarán las bibliotecas `clientlib-site` y `clientlib-dependecies` que contienen el tema del sitio y las dependencias de terceros.

Las bibliotecas del lado del cliente tienen algunas limitaciones en cuanto a la compatibilidad con lenguajes como [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Existen varias herramientas de código abierto como [NPM](https://www.npmjs.com/) y [webpack](https://webpack.js.org/) que aceleran y optimizan el desarrollo del front-end. El objetivo del módulo **ui.front** es poder utilizar estas herramientas para administrar la mayoría de los archivos de origen front-end.

1. Abra el módulo **ui.front** y vaya a `src/main/webpack/site`.
1. Abra el archivo `main.scss`

   ![main.scs - punto de entrada](assets/client-side-libraries/main-scss.png)

   `main.scss` es el punto de entrada a todos los archivos Sass del  `ui.frontend` módulo. Incluirá el archivo `_variables.scss`, que contiene una serie de variables de marca que se utilizarán en distintos archivos de Sass del proyecto. El archivo `_base.scss` también se incluye y define algunos estilos básicos para elementos HTML. Una expresión normal incluye todos los estilos de los estilos de componentes individuales en `src/main/webpack/components`. Otra expresión regular incluye todos los archivos en `src/main/webpack/site/styles`.

1. Inspect el archivo `main.ts`. `main.ts` incluye  `main.scss` e incluye una expresión regular para recopilar cualquier  `.js` o  `.ts` archivo del proyecto. Este punto de entrada será utilizado por los [archivos de configuración del webpack](https://webpack.js.org/configuration/) como punto de entrada para todo el módulo `ui.frontend`.

1. Inspect los archivos debajo de `src/main/webpack/site/styles`:

   ![Archivos de estilo](assets/client-side-libraries/style-files.png)

   Estos estilos de archivo para elementos globales de la plantilla, como Encabezado, Pie de página y contenedor de contenido principal. Las reglas CSS de estos archivos destinatario diferentes elementos HTML `header`, `main` y `footer`. Estos elementos HTML se definieron mediante políticas en el capítulo anterior [Páginas y plantillas](./pages-templates.md).

1. Expanda la carpeta `components` en `src/main/webpack` e inspeccione los archivos.

   ![Archivos Sass de componentes](assets/client-side-libraries/component-sass-files.png)

   Cada archivo se asigna a un componente principal como el [componente de acordeón](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Cada componente principal se genera con la notación [Modificador de elementos de bloque](https://getbem.com/) o BEM para facilitar el destinatario de clases CSS específicas con reglas de estilo. Los archivos situados debajo de `/components` han sido rellenados por el arquetipo del proyecto AEM con las distintas reglas de BEM para cada componente.

1. Descargue los estilos base WKND **[wknd-base-style-src.zip](./assets/client-side-libraries/wknd-base-styles-src.zip)** y **descomprima** el archivo.

   ![Estilos base WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar el tutorial, hemos proporcionado varios archivos Sass que implementan la marca WKND en función de los componentes principales y la estructura de la plantilla de página de artículos.

1. Sobrescriba el contenido de `ui.frontend/src` con archivos del paso anterior. El contenido del zip debe sobrescribir las siguientes carpetas:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Archivos cambiados](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect los archivos modificados para ver los detalles de la implementación de estilo WKND.

## Inspect la integración de ui.frontender {#ui-frontend-integration}

Un elemento de integración clave integrado en el módulo **ui.frontender**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) toma los artefactos CSS y JS compilados de un proyecto de webpack/npm y los transforma en AEM bibliotecas del lado del cliente.

![Integración de la arquitectura ui.frontenend](assets/client-side-libraries/ui-frontend-architecture.png)

El arquetipo del proyecto AEM configura automáticamente esta integración. Luego, explore cómo funciona.


1. Abra un terminal de la línea de comandos e instale el módulo **ui.frontendr** mediante el comando `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` solo se debe ejecutar una vez, después de un nuevo clon o generación del proyecto.

1. En el mismo terminal, genere e implemente el módulo **ui.front** mediante el comando `npm run dev`:

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > Puede recibir un error como &quot;ERROR en ./src/main/webpack/site/main.scss&quot;.
   > Esto suele suceder porque el entorno ha cambiado desde que se ejecutó `npm install`.
   > Ejecute `npm rebuild node-sass` para solucionar el problema. Esto ocurrirá si la versión de `npm` instalada en el equipo de desarrollo local difiere de la versión utilizada por el Maven `frontend-maven-plugin` en el archivo `aem-guides-wknd/pom.xml`. Esto se puede corregir de forma permanente modificando la versión en el archivo pom para que coincida con la versión local o viceversa.

1. El comando `npm run dev` debe generar y compilar el código fuente para el proyecto de Webpack y, finalmente, rellenar el módulo **clientlib-site** y **clientlib-dependencias** en el módulo **ui.apps**.

   >[!NOTE]
   >
   >También hay un perfil `npm run prod` que minimizará el JS y el CSS. Esta es la compilación estándar siempre que la compilación del webpack se activa mediante Maven. Encontrará más detalles sobre el módulo [ui.frontender aquí](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect el archivo `site.css` debajo de `ui.frontend/dist/clientlib-site/css/site.css`. Se trata del CSS compilado basado en los archivos de origen de Sass.

   ![Css del sitio distribuido](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect el archivo `ui.frontend/clientlib.config.js`. Este es el archivo de configuración para un complemento npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma el contenido de `/dist` en una biblioteca cliente y lo mueve al módulo `ui.apps`.

1. Inspect el archivo `site.css` en el módulo **ui.apps** en `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Debe ser una copia idéntica del archivo `site.css` desde el módulo **ui.frontender**. Ahora que está en el módulo **ui.apps** se puede implementar en AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Dado que **clientlib-site** se compila durante el tiempo de compilación, usando **npm** o **maven**, se puede ignorar sin problemas desde el control de código fuente en el módulo **ui.apps**. Inspect el archivo `.gitignore` debajo de **ui.apps**.

1. Sincronice la biblioteca `clientlib-site` con una instancia local de AEM mediante las herramientas de desarrollador o las habilidades de Maven.

   ![Sincronizar sitio de biblioteca de clientes](assets/client-side-libraries/sync-clientlib-site.png)

1. Abra el artículo de LA Skatepark en AEM en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos base actualizados para el artículo](assets/client-side-libraries/updated-base-styles.png)

   Ahora debería ver los estilos actualizados del artículo. Es posible que deba realizar una actualización difícil para borrar los archivos CSS almacenados en la caché del navegador.

   ¡Está empezando a mirar mucho más cerca de las maquetas!

   >[!NOTE]
   >
   > Los pasos anteriores para crear e implementar el código ui.frontender en AEM se ejecutarán automáticamente cuando se active una compilación de Maven desde la raíz del proyecto `mvn clean install -PautoInstallSinglePackage`.

>[!CAUTION]
>
> Puede que no sea necesario utilizar el módulo **ui.front** para todos los proyectos. El módulo **ui.front** agrega complejidad adicional y si no hay necesidad/deseo de usar algunas de estas herramientas avanzadas del front-end (Sass, webpack, npm...) puede que no sea necesario.

## Inclusión de página y plantilla {#page-inclusion}

A continuación, vamos a revisar cómo se hace referencia a los clientes en la página de AEM. Una práctica recomendada común en el desarrollo Web es incluir CSS en el encabezado HTML `<head>` y JavaScript justo antes de cerrar la etiqueta `</body>`.

1. En el módulo **ui.apps** navegue a `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Componente de página de estructura](assets/client-side-libraries/customheaderlibs-html.png)

   Es el componente `page` que se utiliza para procesar todas las páginas en la implementación de WKND.

1. Abra el archivo `customheaderlibs.html`. Observe las líneas `${clientlib.css @ categories='wknd.base'}`. Esto indica que el CSS para la clientlib con una categoría de `wknd.base` se incluirá a través de este archivo, incluyendo efectivamente **clientlib-base** en el encabezado de todas nuestras páginas.

1. Actualice `customheaderlibs.html` para incluir una referencia a los estilos de fuente de Google que especificamos anteriormente en el módulo **ui.frontender**.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect el archivo `customfooterlibs.html`. Este archivo, como `customheaderlibs.html`, debe sobrescribirse mediante la implementación de proyectos. Aquí la línea `${clientlib.js @ categories='wknd.base'}` significa que el JavaScript de **clientlib-base** se incluirá en la parte inferior de todas nuestras páginas.

1. Exporte el componente `page` al servidor de AEM mediante las herramientas para desarrolladores o con sus conocimientos de Maven.

1. Vaya a la plantilla Página del artículo en [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Haga clic en el icono **Información de página** y, en el menú, seleccione **Política de página** para abrir el cuadro de diálogo **Política de página**.

   ![Política de página del menú Plantilla de página del artículo](assets/client-side-libraries/template-page-policy.png)

   *Información de página > Directiva de página*

1. Observe que las categorías para `wknd.dependencies` y `wknd.site` se enumeran aquí. De forma predeterminada, los clientes configurados mediante la directiva de página se dividen para incluir CSS en el encabezado de página y JavaScript en el extremo del cuerpo. Si lo desea, puede realizar una lista explícita de que el JavaScript clientlib se cargue en el encabezado de página. Este es el caso de `wknd.dependencies`.

   ![Política de página del menú Plantilla de página del artículo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > También es posible hacer referencia a `wknd.site` o `wknd.dependencies` desde el componente de página directamente, utilizando la secuencia de comandos `customheaderlibs.html` o `customfooterlibs.html`, como ya vimos anteriormente para la clientlib `wknd.base`. El uso de la plantilla proporciona cierta flexibilidad en cuanto a que puede elegir qué clientes se utilizan por plantilla. Por ejemplo, si tiene una biblioteca JavaScript muy pesada que solo se va a usar en una plantilla seleccionada.

1. Vaya a la página **Parques de Patinaje de LA** creada con la **Plantilla de página de artículo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Debería ver una diferencia en las fuentes.

1. Haga clic en el icono **Información de página** y, en el menú, seleccione **Vista tal y como se publicó** para abrir la página del artículo fuera del editor de AEM.

   ![Ver como aparece publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Vista el origen de la página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) y debería poder ver las siguientes referencias de clientlib en el `<head>`:

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   Observe que los clientlibs están usando el punto de conexión proxy `/etc.clientlibs`. También debería ver las siguientes variables de clientlib en la parte inferior de la página:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >En el lado de publicación es fundamental que las bibliotecas cliente **no** se proporcionen desde **/apps**, ya que esta ruta debe restringirse por motivos de seguridad mediante la sección [del filtro Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [propiedad allowProxy](https://docs.adobe.com/content/help/es-ES/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la biblioteca del cliente garantiza que CSS y JS se proporcionen desde **/etc.clientlibs**.

## Webpack DevServer - Marcador estático {#webpack-dev-static}

En el par anterior de ejercicios, pudimos actualizar varios archivos Sass en el módulo **ui.front** y a través de un proceso de compilación, finalmente ver estos cambios reflejados en AEM. Luego veremos una técnica que aprovecha un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente nuestros estilos front-end contra **static** HTML.

Esta técnica es útil si la mayoría de los estilos y el código de front-end serán realizados por un desarrollador dedicado de Front-End que puede no tener acceso fácil a un entorno de AEM. Esta técnica también permite a la FED realizar modificaciones directamente en el HTML, que luego se pueden transferir a un desarrollador de AEM para implementarlo como componentes.

1. Copie el origen de página de la página del artículo del skatepark LA en [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Vuelva a abrir su IDE. Pegue el marcado copiado de AEM en el `index.html` del módulo **ui.frontender** debajo de `src/main/webpack/static`.
1. Edite el marcado copiado y elimine todas las referencias a **clientlib-site** y **clientlib-dependencias**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Podemos eliminar esas referencias porque el servidor de desarrollo de webpack generará estos artefactos automáticamente.

1. Inicio el servidor de desarrollo de webpack desde un nuevo terminal mediante la ejecución del siguiente comando desde el módulo **ui.front**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Esto debería abrir una nueva ventana del explorador en [http://localhost:8080/](http://localhost:8080/) con marcado estático.

1. Edite el archivo `src/main/webpack/site/_variables.scss`. Reemplace la regla `$text-color` por lo siguiente:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Guarde los cambios.

1. Debería ver automáticamente los cambios reflejados en el explorador en [http://localhost:8080](http://localhost:8080).

   ![Cambios en el servidor de desarrollo de webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Revise el archivo `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contiene la configuración del webpack utilizada para el inicio del webpack-dev-server. Tenga en cuenta que reemplaza las rutas `/content` y `/etc.clientlibs` desde una instancia de AEM que se esté ejecutando localmente. Así es como están disponibles las imágenes y otros clientlibs (no administrados por el código **ui.front**).

   >[!CAUTION]
   >
   > El src de imagen del marcado estático apunta a un componente de imagen dinámica en una instancia de AEM local. Las imágenes aparecerán rotas si cambia la ruta de la imagen, si no se ha iniciado la AEM o si el explorador no ha iniciado sesión en la instancia de AEM local. Si se transfiere a un recurso externo, también es posible reemplazar las imágenes con referencias estáticas.

1. Puede **detener** el servidor de webpack desde la línea de comandos escribiendo `CTRL+C`.

## Webpack DevServer - Watch y aemsync {#webpack-dev-watch}

Otra técnica consiste en que Node.js observe los cambios de archivos en los archivos src del módulo `ui.frontend`. Siempre que un archivo cambie, compilará rápidamente la biblioteca del cliente y utilizará el módulo [aemsync](https://www.npmjs.com/package/aemsync) npm para sincronizar los cambios en un servidor AEM en ejecución.

1. Inicio el servidor de desarrollo de webpack en el modo **watch** desde un nuevo terminal ejecutando el siguiente comando desde el módulo **ui.front**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. Esto compilará los archivos `src` y sincronizará los cambios con AEM en [http://localhost:4502](http://localhost:4502)

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

1. Navegue hasta AEM y el artículo de parques de patinaje LA: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Cambios implementados en AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Los cambios deben implementarse en AEM. Hay un ligero retraso y tendrá que actualizar el explorador manualmente para ver las actualizaciones. Sin embargo, ver los cambios directamente en AEM resulta beneficioso si está trabajando con nuevos componentes y creación de cuadros de diálogo.

1. Vuelva el cambio a `_variables.scss` y guarde los cambios. Los cambios deben sincronizarse de nuevo con la instancia local de AEM después de un ligero retraso.

1. Detenga el servidor de desarrollo de webpack y realice una compilación completa de Maven desde la raíz del proyecto:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Nuevamente, el módulo `ui.frontend` se compila, se transforma en bibliotecas de clientes y se implementa en AEM mediante el módulo `ui.apps`. Sin embargo, esta vez Maven lo hace todo por nosotros.

## Felicitaciones! {#congratulations}

Felicitaciones, la página de artículos ahora tiene algunos estilos coherentes que coinciden con la marca WKND y usted se ha familiarizado con el módulo **ui.frontendr**.

### Próximos pasos {#next-steps}

Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. [Desarrollar con las portadas de ](style-system.md) sistemas de estilo mediante el sistema de estilos para ampliar los componentes principales con CSS específica de la marca y configuraciones de políticas avanzadas del Editor de plantillas.

Vista el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código de forma local en la plataforma Git `tutorial/client-side-libraries-solution`.

1. Clona el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Compruebe la rama `tutorial/client-side-libraries-solution`.

## Herramientas y recursos adicionales {#additional-resources}

### aemfeed {#develop-aemfed}

[**aemfedis**](https://aemfed.io/) es una herramienta de código abierto de línea de comandos que puede utilizarse para acelerar el desarrollo del front-end. Está equipado con [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) y [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

En un **aemfeed** de alto nivel está diseñado para escuchar los cambios de archivos dentro del módulo **ui.apps** y sincronizarlos automáticamente directamente en una instancia de AEM en ejecución. En función de los cambios, un navegador local se actualizará automáticamente, acelerando así el desarrollo del front-end. También está diseñado para funcionar con el rastreador de registro de Sling para mostrar automáticamente cualquier error del lado del servidor directamente en el terminal.

Si está realizando mucho trabajo dentro del módulo **ui.apps**, modificando las secuencias de comandos HTML y creando componentes personalizados, **aemfeed** puede ser una herramienta muy poderosa para usar. [Puede encontrar la documentación completa aquí.](https://github.com/abmaonline/aemfed).

### Depuración de bibliotecas del lado del cliente {#debugging-clientlibs}

Con diferentes métodos de **categorías** y **incrustaciones** para incluir varias bibliotecas de cliente, puede resultar complicado solucionar problemas. AEM expone varias herramientas para ayudar con esto. Una de las herramientas más importantes es **Reconstruir bibliotecas de clientes**, lo que obligará a AEM a volver a compilar cualquier archivo LESS y generar el CSS.

* [**Libros**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  de volcado: Lista todas las bibliotecas de cliente registradas en la instancia de AEM.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Salida**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  de prueba: permite al usuario ver la salida HTML esperada de clientlib incluye en función de la categoría.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validación**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  de dependencias de bibliotecas: resalta las dependencias o categorías incrustadas que no se pueden encontrar.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruir bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  de cliente: permite al usuario forzar a AEM a reconstruir todas las bibliotecas de cliente o invalidar la caché de las bibliotecas de cliente. Esta herramienta es especialmente eficaz cuando se desarrolla con LESS, ya que esto puede obligar a AEM a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y, a continuación, actualizar la página en lugar de volver a compilar todas las bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![volver a compilar la biblioteca del cliente](assets/client-side-libraries/rebuild-clientlibs.png)

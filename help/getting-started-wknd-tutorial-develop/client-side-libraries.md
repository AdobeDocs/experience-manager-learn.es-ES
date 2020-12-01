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
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 5%

---


# Bibliotecas del lado del cliente y flujo de trabajo front-end {#client-side-libraries}

Conozca cómo se utilizan las bibliotecas del lado del cliente o los clientes para implementar y administrar CSS y Javascript para una implementación de sitios de Adobe Experience Manager (AEM). Este tutorial también explicará cómo el módulo [ui.frontender](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/uifrontend.html), un proyecto de [webpack](https://webpack.js.org/) desacoplado, se puede integrar en el proceso de generación de extremo a extremo.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar el tutorial [Conceptos básicos del componente](component-basics.md#client-side-libraries) para comprender los aspectos fundamentales de las bibliotecas y AEM del lado del cliente.

### Proyecto de inicio

Consulte el código de línea base que el tutorial genera:

1. Clona el repositorio [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Consulte la rama `client-side-libraries/start`

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. Implemente código base en una instancia de AEM local con sus conocimientos Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) o extraer el código localmente cambiando a la rama `client-side-libraries/solution`.

## Objetivo

1. Comprender cómo se incluyen las bibliotecas del lado del cliente en una página a través de una plantilla editable.
1. Aprenda a utilizar el módulo UI.Frontender y un servidor de desarrollo de webpack para el desarrollo de front-end dedicado.
1. Comprender el flujo de trabajo completo de la entrega de CSS y JavaScript compilados a una implementación de sitios.

## Qué va a generar {#what-you-will-build}

En este capítulo, agregará algunos estilos de línea de base para el sitio WKND y la plantilla de página de artículos en un esfuerzo por acercar la implementación a las [maquetas de diseño de la interfaz de usuario](assets/pages-templates/wknd-article-design.xd). Utilizará un flujo de trabajo front-end avanzado para integrar un proyecto de webpack en una biblioteca cliente AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

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

1. Si utiliza Eclipse u otro IDE, abra el módulo **ui.apps**.
1. Expanda la ruta `/apps/wknd/clientlibs` para vista de los clientlibs generados por el arquetipo.

   ![Clientlibs en ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Inspeccionaremos a estos clientes con buenos detalles a continuación.

1. Inspect las propiedades de `clientlibs/clientlib-base`.

   **clientlib-** baserepresenta el nivel base de CSS y JavaScript necesario para que el sitio WKND funcione. Observe la propiedad `categories` que está establecida en `wknd.base`. `categories` es un mecanismo de etiquetado para clientes y es la forma en que se puede hacer referencia a ellos.

   Observe la propiedad `embed` y los valores `String[]`. La propiedad `embed` incrusta otros clientes en función de su categoría. **clientlib-** basewill incluirá todas las bibliotecas de clientes de componentes principales de AEM que sean necesarias. Esto incluye artefactos como javascript para el carrusel, componentes de búsqueda rápida para funcionar. **clientlib-** basewill no incluirá ninguna CSS y Javascript propia, sino que solo incorporará otras bibliotecas de cliente. **clientlib-** baseincrusta la  **clientlib-** gridclientlib con la categoría de  `wknd.grid`.

   Observe la propiedad `allowProxy` establecida en `true`. Se recomienda configurar siempre `allowProxy=true` en clientlibs. La propiedad `allowProxy` nos permite almacenar los clientlibs con nuestro código de aplicación en `/apps` **pero** luego envía a los clientlibs por una ruta con el prefijo `/etc.clientlibs` para evitar exponer cualquier código de aplicación a los usuarios finales. Puede encontrar más información sobre la propiedad [allowProxy aquí.](https://docs.adobe.com/content/help/es-ES/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1. Inspect las propiedades de `clientlibs/clientlib-grid`.

   **clientlib-** gridis es responsable de incluir/generar la CSS necesaria para que el  [modelo de ](https://docs.adobe.com/content/help/es-ES/experience-manager-65/authoring/siteandpage/responsive-layout.html) diseño funcione con el editor de AEM Sites. **clientlib-** gridhad tiene una categoría definida  `wknd.grid` y incrustada mediante  **clientlib-base**.

   La cuadrícula se puede personalizar para utilizar distintas cantidades de columnas y puntos de interrupción. A continuación, se actualizarán los puntos de interrupción predeterminados generados.

1. Actualice el archivo `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   Esto cambiará los puntos de interrupción para que se correspondan con los puntos de interrupción de plantilla establecidos en `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`.

   Observe que este archivo en realidad hace referencia a un archivo `grid_base.less` en `/libs` que contiene una mezcla personalizada para generar la cuadrícula.

1. Inspect las propiedades de `clientlibs/clientlib-site`.

   **clientlib-** site contendrá todos los estilos específicos del sitio para la marca WKND. Observe la categoría de `wknd.site`. El CSS y Javascript que generan esta clientlib se mantendrán en el módulo `ui.frontend`. A continuación exploraremos esta integración.

1. Inspect las propiedades de `clientlibs/clientlib-dependencies`.

   **clientlib-** dependencia se ha diseñado para integrar dependencias de terceros. Es una clientlib independiente para que se pueda cargar en la parte superior de la página HTML si es necesario. Observe la categoría de `wknd.dependencies`. El CSS y Javascript que generan esta clientlib se mantendrán en el módulo `ui.frontend`. Exploraremos esta integración más adelante en el tutorial.

## Uso del módulo ui.front {#ui-frontend}

A continuación, exploraremos el uso del módulo **[ui.frontender](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**.

### Motivación

Las bibliotecas del lado del cliente tienen algunas limitaciones en cuanto a la compatibilidad con lenguajes como [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). También ha habido una explosión de herramientas de código abierto como [NPM](https://www.npmjs.com/) y [webpack](https://webpack.js.org/) que aceleran y optimizan el desarrollo del front-end.

La idea básica detrás del módulo **ui.frontender** es poder utilizar herramientas buenas como NPM y Webpack para administrar la mayoría del desarrollo del front-end. Un elemento de integración clave integrado en el módulo **ui.frontender**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) toma los artefactos CSS y JS compilados de un proyecto de webpack/npm y los transforma en AEM bibliotecas del lado del cliente. Esto brinda a los programadores avanzados buena libertad para elegir diferentes herramientas y tecnologías.

![Integración de la arquitectura ui.frontenend](assets/client-side-libraries/ui-frontend-architecture.png)

### Uso

Ahora agregaremos algunos estilos de base para la marca WKND agregando algunos archivos Sass (`.scss` extensión) a través del módulo **ui.frontendr**.

1. Abra el módulo **ui.front** y vaya a `src/main/webpack/base/sass`.

   ![módulo ui.frontender](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. Cree un nuevo archivo denominado `_variables.scss` debajo de la carpeta `src/main/webpack/base/sass`.
1. Rellene `_variables.scss` con lo siguiente:

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Sass nos permite crear variables, que luego pueden utilizarse en distintos archivos para garantizar la coherencia. Observe las familias de fuentes. Más adelante en el tutorial veremos cómo podemos incluir una llamada a las fuentes web de Google para poder utilizar estas fuentes.

1. Cree otro archivo llamado `_elements.scss` debajo de `src/main/webpack/base/sass` y llénelo con lo siguiente:

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   Observe que el archivo `_elements.scss` hace uso de las variables en `_variables.scss`.

1. Actualice `_shared.scss` debajo de `src/main/webpack/base/sass` para incluir los archivos `_elements.scss` y `_variables.scss`.

   ```css
   @import './variables';
   @import './elements';
   ```

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
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   El comando `npm run dev` debe generar y compilar el código fuente para el proyecto de Webpack y, finalmente, rellenar el módulo **clientlib-site** y **clientlib-dependencias** en el módulo **ui.apps**.

   >[!NOTE]
   >
   >También hay un perfil `npm run prod` que minimizará el JS y el CSS. Esta es la compilación estándar siempre que la compilación del webpack se activa mediante Maven. Encontrará más detalles sobre el módulo [ui.frontender aquí](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect el archivo `site.css` debajo de `ui.frontend/dist/clientlib-site/css/site.css`. Observe que la CSS está compuesta principalmente por el contenido del archivo `_elements.scss` creado anteriormente, pero las variables se han reemplazado por valores reales.

   ![Css del sitio distribuido](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect el archivo `ui.frontend/clientlib.config.js`. Este es el archivo de configuración para un complemento npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-** generatores la herramienta responsable de transformar el código CSS/JavaScript compilado y copiarlo en el archivo  **ui.** appsmoap.

1. Inspect el archivo `site.css` en el módulo **ui.apps** en `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Debe ser una copia idéntica del archivo `site.css` desde el módulo **ui.frontender**. Ahora que está en el módulo **ui.apps** se puede implementar en AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Dado que **clientlib-site** se compila realmente durante el tiempo de compilación, usando **npm** o **maven**, en realidad se puede ignorar desde el control de código fuente en el módulo **ui.apps**. Inspect el archivo `.gitignore` debajo de **ui.apps**.

>[!CAUTION]
>
> Puede que no sea necesario utilizar el módulo **ui.front** para todos los proyectos. El módulo **ui.front** agrega complejidad adicional y si no hay necesidad/deseo de usar algunas de estas herramientas avanzadas del front-end (Sass, webpack, npm...) puede ser excesivo. Por este motivo se considera una parte opcional del Arquetipo de proyecto AEM y el uso de bibliotecas estándar del lado del cliente y CSS vanilla y JavaScript sigue siendo totalmente compatible.

## Inclusión de página y plantilla {#page-inclusion}

A continuación, analizaremos cómo se configura el proyecto para incluir a los clientes en AEM plantillas/páginas. Una práctica recomendada común en el desarrollo Web es incluir CSS en el encabezado HTML `<head>` y JavaScript justo antes de cerrar la etiqueta `</body>`.

1. En el módulo **ui.apps** navegue a `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Componente de página de estructura](assets/client-side-libraries/customheaderlibs-html.png)

   Es el componente `page` que se utiliza para procesar todas las páginas en la implementación de WKND.

1. Abra el archivo `customheaderlibs.html`. Observe las líneas `${clientlib.css @ categories='wknd.base'}`. Esto indica que el CSS para la clientlib con una categoría de `wknd.base` se incluirá a través de este archivo, incluyendo efectivamente **clientlib-base** en el encabezado de todas nuestras páginas.

1. Actualice `customheaderlibs.html` para incluir una referencia a los estilos de fuente de Google que especificamos anteriormente en el módulo **ui.frontender**. También comentaremos ContextHub por ahora...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. Inspect el archivo `customfooterlibs.html`. Este archivo, como `customheaderlibs.html`, debe sobrescribirse mediante la implementación de proyectos. Aquí la línea `${clientlib.js @ categories='wknd.base'}` significa que el JavaScript de **clientlib-base** se incluirá en la parte inferior de todas nuestras páginas.

1. Cree e implemente el proyecto en una instancia de AEM local mediante Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Vaya a las plantillas de WKND en [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Seleccione y abra la **Plantilla de página de artículo** en el Editor de plantillas.

   ![Seleccionar plantilla de página de artículo](assets/client-side-libraries/open-article-page-template.png)

1. Haga clic en el icono **Información de página** y, en el menú, seleccione **Política de página** para abrir el cuadro de diálogo **Política de página**.

   ![Política de página del menú Plantilla de página del artículo](assets/client-side-libraries/template-page-policy.png)

   *Información de página > Directiva de página*

1. Observe que las categorías para `wknd.dependencies` y `wknd.site` se enumeran aquí. De forma predeterminada, los clientes configurados mediante la directiva de página se dividen para incluir CSS en el encabezado de página y JavaScript en el extremo del cuerpo. Si lo desea, puede realizar una lista explícita de que el JavaScript clientlib se cargue en el encabezado de página. Este es el caso de `wknd.dependencies`.

   ![Política de página del menú Plantilla de página del artículo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > También es posible hacer referencia a `wknd.site` o `wknd.dependencies` desde el componente de página directamente, utilizando la secuencia de comandos `customheaderlibs.html` o `customfooterlibs.html`, como ya vimos anteriormente para la clientlib `wknd.base`. El uso de la plantilla proporciona cierta flexibilidad en cuanto a que puede elegir qué clientes se utilizan por plantilla. Por ejemplo, si tiene una biblioteca JavaScript muy pesada que solo se va a usar en una plantilla seleccionada.

1. Vaya a la página **Parques de Patinaje de LA** creada con la **Plantilla de página de artículo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Debe ver una diferencia en las fuentes y algunos estilos básicos aplicados para indicar que el CSS creado en el módulo **ui.frontender** está funcionando.

1. Haga clic en el icono **Información de página** y, en el menú, seleccione **Vista tal y como se publicó** para abrir la página del artículo fuera del editor de AEM.

   ![Ver como aparece publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Vista el origen de la página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) y debería poder ver las siguientes referencias de clientlib en el `<head>`:

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   Observe que los clientlibs están usando el punto de conexión proxy `/etc.clientlibs`. También debería ver las siguientes variables de clientlib en la parte inferior de la página:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >En el lado de publicación es fundamental que las bibliotecas cliente **no** se proporcionen desde **/apps**, ya que esta ruta debe restringirse por motivos de seguridad mediante la sección [del filtro Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [propiedad allowProxy](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la biblioteca del cliente garantiza que CSS y JS se proporcionen desde **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

En el par anterior de ejercicios, pudimos actualizar varios archivos Sass en el módulo **ui.front** y a través de un proceso de compilación, finalmente ver estos cambios reflejados en AEM. Luego analizaremos cómo aprovechar un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente nuestros estilos de front-end.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel que se muestran en el vídeo:

1. Inicio el servidor de desarrollo de webpack ejecutando el siguiente comando desde el módulo **ui.frontender**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Esto debería abrir una nueva ventana del explorador en [http://localhost:8080/](http://localhost:8080/) con marcado estático.
1. Copie el origen de página de la página del artículo del skatepark LA en [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Pegue el marcado copiado de AEM en el `index.html` del módulo **ui.frontender** debajo de `src/main/webpack/static`.
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

1. Edite los archivos `.scss` y vea los cambios reflejados automáticamente en el explorador.
1. Revise el archivo `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contiene la configuración del webpack utilizada para el inicio del webpack-dev-server. Tenga en cuenta que reemplaza las rutas `/content` y `/etc.clientlibs` desde una instancia de AEM que se esté ejecutando localmente. Así es como están disponibles las imágenes y otros clientlibs (no administrados por el código **ui.front**).

   >[!CAUTION]
   >
   > El src de imagen del marcado estático apunta a un componente de imagen dinámica en una instancia de AEM local. Las imágenes aparecerán rotas si cambia la ruta de la imagen, si no se ha iniciado la AEM o si el navegador no ha iniciado sesión en la instancia de AEM local.
1. Puede **detener** el servidor de webpack desde la línea de comandos escribiendo `CTRL+C`.

## Colocarlo en conjunto {#putting-it-together}

Este tutorial se centra en las bibliotecas del lado del cliente y en los posibles flujos de trabajo front-end que se integrarán con AEM. Con esto en mente, aceleraremos la implementación instalando [client-side-library-final-style.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip), que proporciona algunos estilos predeterminados para los componentes principales utilizados en la plantilla de página de artículos:

* [Ruta de navegación](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/breadcrumb.html)
* [Descargar](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Imagen](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/components/image.html)
* [Lista](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/list.html)
* [Navegación](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)
* [Búsqueda rápida](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/quick-search.html)
* [Separador](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel que se muestran en el vídeo:

1. Descargue [client-side-library-final-style.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) y descomprima el contenido debajo de `ui.frontend/src/main/webpack`. El contenido del zip debe sobrescribir las siguientes carpetas:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. Previsualización de los nuevos estilos con el servidor de desarrollo de webpack:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Implemente la base de código en una instancia de AEM local para ver los nuevos estilos aplicados al artículo del parque de patines LA:

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## Felicitaciones! {#congratulations}

Felicitaciones, la página de artículos ahora tiene algunos estilos coherentes que coinciden con la marca WKND y usted se ha familiarizado con el módulo **ui.frontendr**.

### Próximos pasos {#next-steps}

Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. [Desarrollar con las portadas de ](style-system.md) sistemas de estilo mediante el sistema de estilos para ampliar los componentes principales con CSS específica de la marca y configuraciones de políticas avanzadas del Editor de plantillas.

Vista el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código de forma local en la plataforma Git `client-side-libraries/solution`.

1. Clona el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Compruebe la rama `client-side-libraries/solution`.

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

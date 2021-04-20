---
title: Bibliotecas del lado del cliente y flujo de trabajo del front-end
description: Descubra cómo se utilizan las bibliotecas del lado del cliente o clientlibs para implementar y administrar CSS y Javascript para una implementación de Adobe Experience Manager (AEM) Sites. Este tutorial también tratará cómo el módulo ui.frontend, un proyecto de webpack, se puede integrar en el proceso de compilación de extremo a extremo.
sub-product: sitios
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3301'
ht-degree: 3%

---


# Bibliotecas del lado cliente y flujo de trabajo del front-end {#client-side-libraries}

Descubra cómo se utilizan las bibliotecas del lado del cliente o clientlibs para implementar y administrar CSS y Javascript para una implementación de Adobe Experience Manager (AEM) Sites. Este tutorial también tratará cómo el módulo [ui.frontend](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/uifrontend.html), un proyecto [webpack](https://webpack.js.org/) desacoplado, se puede integrar en el proceso de compilación de extremo a extremo.

## Requisitos previos {#prerequisites}

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar el tutorial [Conceptos básicos del componente](component-basics.md#client-side-libraries) para comprender los fundamentos de las bibliotecas del lado del cliente y de AEM.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

Consulte el código de línea base sobre el que se basa el tutorial:

1. Consulte la rama `tutorial/client-side-libraries-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Implemente código base en una instancia local de AEM con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) o extraer el código localmente cambiando a la rama `tutorial/client-side-libraries-solution`.

## Objetivo

1. Comprender cómo se incluyen las bibliotecas del lado del cliente en una página a través de una plantilla editable.
1. Aprenda a utilizar el módulo UI.Frontend y un servidor de desarrollo de webpack para el desarrollo de front-end dedicado.
1. Comprenda el flujo de trabajo completo de entrega de CSS y JavaScript compilados a una implementación de Sites.

## Qué va a generar {#what-you-will-build}

En este capítulo, agregará algunos estilos de línea de base para el sitio WKND y la plantilla de página de artículo en un esfuerzo por acercar la implementación a las [maquetas de diseño de la interfaz de usuario](assets/pages-templates/wknd-article-design.xd). Utilizará un flujo de trabajo front-end avanzado para integrar un proyecto de webpack en una biblioteca de cliente de AEM.

![Estilos finalizados](assets/client-side-libraries/finished-styles.png)

*Artículo Página con estilos de línea de base aplicados*

## Fondo {#background}

Las bibliotecas del lado del cliente proporcionan un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. Los objetivos básicos para bibliotecas de cliente o clientlibs son:

1. Almacene CSS/JS en pequeños archivos discretos para facilitar el desarrollo y el mantenimiento
1. Administrar dependencias en marcos de terceros de forma organizada
1. Minimice el número de solicitudes del lado del cliente concatenando CSS/JS en una o dos solicitudes.

Puede encontrar más información sobre el uso de [bibliotecas del lado del cliente aquí.](https://docs.adobe.com/content/help/es-ES/experience-manager-65/developing/introduction/clientlibs.html)

Las bibliotecas del lado del cliente tienen algunas limitaciones. Lo más notable es que el soporte para los lenguajes front-end más populares, como Sass, LESS y TypeScript es limitado. En el tutorial veremos cómo el módulo **ui.frontend** puede ayudar a resolver esto.

Implemente el código de inicio en una instancia local de AEM y vaya a [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Esta página está actualmente sin estilo. A continuación, implementaremos bibliotecas del lado del cliente para la marca WKND para agregar CSS y Javascript a la página.

## Organización de bibliotecas del lado del cliente {#organization}

A continuación, exploraremos la organización de clientlibs generada por el [Tipo de archivo del proyecto AEM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/developing/archetype/overview.html).

![Organización clientlibrary de alto nivel](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Diagrama de alto nivel Organización de biblioteca del lado del cliente e inclusión de página*

>[!NOTE]
>
> El siguiente tipo de archivo del proyecto AEM genera la siguiente organización de biblioteca del lado del cliente, pero representa simplemente un punto de partida. La forma en que un proyecto gestiona y entrega CSS y Javascript a una implementación de Sites puede variar considerablemente en función de los recursos, los conjuntos de habilidades y los requisitos.

1. Con VSCode u otro IDE, abra el módulo **ui.apps**.
1. Expanda la ruta `/apps/wknd/clientlibs` para ver los clientlibs generados por el tipo de archivo.

   ![Clientlibs en ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Inspeccionaremos estos clientlibs con más detalle a continuación.

1. La siguiente tabla resume las bibliotecas de cliente. Puede encontrar más información sobre [incluidas las bibliotecas de cliente aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Nombre | Descripción | Notas |
   |-------------------| ------------| ------|
   | `clientlib-base` | Nivel base de CSS y JavaScript necesarios para que el sitio WKND funcione | incrusta las bibliotecas de cliente de los componentes principales |
   | `clientlib-grid` | Genera el CSS necesario para que funcione [Layout Mode](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html). | Los puntos de interrupción móviles/de Tablet se pueden configurar aquí |
   | `clientlib-site` | Contiene un tema específico del sitio para el sitio WKND | Generado por el módulo `ui.frontend` |
   | `clientlib-dependencies` | Incrusta cualquier dependencia de terceros | Generado por el módulo `ui.frontend` |

1. Observe que `clientlib-site` y `clientlib-dependencies` se ignoran desde el control de código fuente. Esto se hace por diseño, ya que el módulo `ui.frontend` los generará en el momento de la compilación.

## Actualizar estilos base {#base-styles}

A continuación, actualice los estilos base definidos en el módulo **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. Los archivos del módulo `ui.frontend` generarán las bibliotecas `clientlib-site` y `clientlib-dependecies` que contienen el tema del sitio y cualquier dependencia de terceros.

Las bibliotecas del lado del cliente tienen algunas limitaciones en cuanto a la compatibilidad con lenguajes como [Sass](https://sass-lang.com/) o [TypeScript](https://www.typescriptlang.org/). Existen varias herramientas de código abierto como [NPM](https://www.npmjs.com/) y [webpack](https://webpack.js.org/) que aceleran y optimizan el desarrollo del front-end. El objetivo del módulo **ui.frontend** es poder utilizar estas herramientas para administrar la mayoría de los archivos de origen del front-end.

1. Abra el módulo **ui.frontend** y vaya a `src/main/webpack/site`.
1. Abra el archivo `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` es el punto de entrada a todos los archivos Sass del  `ui.frontend` módulo. Incluirá el archivo `_variables.scss`, que contiene una serie de variables de marca que se utilizarán en los distintos archivos de Sass del proyecto. El archivo `_base.scss` también se incluye y define algunos estilos básicos para los elementos HTML. Una expresión regular incluye todos los estilos de los componentes individuales en `src/main/webpack/components`. Otra expresión regular incluye todos los archivos en `src/main/webpack/site/styles`.

1. Inspeccione el archivo `main.ts`. `main.ts` incluye  `main.scss` y una expresión regular para recopilar  `.js` o  `.ts` archivos del proyecto. Este punto de entrada lo usará el [webpack configuration files](https://webpack.js.org/configuration/) como punto de entrada para todo el módulo `ui.frontend`.

1. Inspeccione los archivos debajo de `src/main/webpack/site/styles`:

   ![Archivos de estilo](assets/client-side-libraries/style-files.png)

   Estos archivos son estilos para elementos globales en la plantilla, como el encabezado, el pie de página y el contenedor de contenido principal. Las reglas CSS de estos archivos se dirigen a diferentes elementos HTML `header`, `main` y `footer`. Estos elementos HTML se definieron mediante políticas en el capítulo anterior [Páginas y plantillas](./pages-templates.md).

1. Expanda la carpeta `components` en `src/main/webpack` e inspeccione los archivos.

   ![Archivos Sass de componentes](assets/client-side-libraries/component-sass-files.png)

   Cada archivo se asigna a un componente principal como el [componente acordeón](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Cada componente principal se crea con la notación [Modificador de elemento de bloque](https://getbem.com/) o BEM para facilitar la segmentación de clases CSS específicas con reglas de estilo. Los archivos situados debajo de `/components` han sido rellenados por el tipo de archivo del proyecto AEM con las diferentes reglas de BEM para cada componente.

1. Descargue los estilos base WKND **[wknd-base-style-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** y **descomprima** el archivo.

   ![Estilos base de WKND](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Para acelerar el tutorial, hemos proporcionado varios archivos Sass que implementan la marca WKND en función de los componentes principales y la estructura de la plantilla de página de artículos.

1. Sobrescriba el contenido de `ui.frontend/src` con archivos del paso anterior. El contenido del zip debe sobrescribir las siguientes carpetas:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Archivos modificados](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspeccione los archivos modificados para ver los detalles de la implementación de estilo WKND.

## Inspeccione la integración de ui.frontend {#ui-frontend-integration}

Un elemento de integración clave integrado en el módulo **ui.frontend**, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) toma los artefactos CSS y JS compilados de un proyecto webpack/npm y los transforma en bibliotecas del lado del cliente de AEM.

![integración de la arquitectura ui.frontend](assets/client-side-libraries/ui-frontend-architecture.png)

El tipo de archivo del proyecto AEM configura automáticamente esta integración. A continuación, explore cómo funciona.


1. Abra un terminal de línea de comandos e instale el módulo **ui.frontend** mediante el comando `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` solo debe ejecutarse una vez, después de un nuevo clon o generación del proyecto.

1. En el mismo terminal, cree e implemente el módulo **ui.frontend** utilizando el comando `npm run dev`:

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > Puede recibir un error como &quot;ERROR en ./src/main/webpack/site/main.scss&quot;.
   > Esto suele ocurrir porque el entorno ha cambiado desde que se ejecutó `npm install`.
   > Ejecute `npm rebuild node-sass` para solucionar el problema. Esto sucederá si la versión de `npm` instalada en el equipo de desarrollo local difiere de la versión utilizada por Maven `frontend-maven-plugin` en el archivo `aem-guides-wknd/pom.xml`. Puede arreglarlo de forma permanente modificando la versión en el archivo pom para que coincida con su versión local o viceversa.

1. El comando `npm run dev` debe generar y compilar el código fuente para el proyecto Webpack y, en última instancia, rellenar el módulo **clientlib-site** y **clientlib-dependencias** en el módulo **ui.apps**.

   >[!NOTE]
   >
   >También hay un perfil `npm run prod` que minificará el JS y el CSS. Esta es la compilación estándar siempre que la compilación del webpack se activa mediante Maven. Puede encontrar más detalles sobre el módulo [ui.frontend aquí](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspeccione el archivo `site.css` debajo de `ui.frontend/dist/clientlib-site/css/site.css`. Esta es la CSS compilada basada en los archivos de origen de Sass.

   ![Distributed Site css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspeccione el archivo `ui.frontend/clientlib.config.js`. Este es el archivo de configuración para un complemento npm, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) que transforma el contenido de `/dist` en una biblioteca de cliente y lo mueve al módulo `ui.apps`.

1. Inspeccione el archivo `site.css` en el módulo **ui.apps** en `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Debe ser una copia idéntica del archivo `site.css` desde el módulo **ui.frontend**. Ahora que está en el módulo **ui.apps**, se puede implementar en AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Dado que **clientlib-site** se compila durante el tiempo de compilación, utilizando **npm** o **maven**, se puede ignorar de forma segura desde el control de código fuente en el módulo **ui.apps**. Inspeccione el archivo `.gitignore` debajo de **ui.apps**.

1. Sincronice la biblioteca `clientlib-site` con una instancia local de AEM mediante las herramientas para desarrolladores o las habilidades con Maven.

   ![Sincronizar sitio de Clientlib](assets/client-side-libraries/sync-clientlib-site.png)

1. Abra el artículo de LA Skatepark en AEM en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Estilos base actualizados para el artículo](assets/client-side-libraries/updated-base-styles.png)

   Ahora debería ver los estilos actualizados para el artículo. Es posible que deba realizar una actualización difícil para borrar cualquier archivo CSS almacenado en caché por el explorador.

   ¡Está empezando a mirar mucho más cerca de las maquetas!

   >[!NOTE]
   >
   > Los pasos realizados anteriormente para crear e implementar el código ui.frontend en AEM se ejecutarán automáticamente cuando se active una compilación de Maven desde la raíz del proyecto `mvn clean install -PautoInstallSinglePackage`.

>[!CAUTION]
>
> El uso del módulo **ui.frontend** puede no ser necesario para todos los proyectos. El módulo **ui.frontend** añade complejidad adicional y si no hay necesidad/deseo de utilizar algunas de estas herramientas avanzadas del front-end (Sass, webpack, npm...) puede que no sea necesario.

## Inclusión de plantilla y página {#page-inclusion}

A continuación, vamos a revisar cómo se hace referencia a clientlibs en la página de AEM. Una práctica recomendada común en el desarrollo web es incluir CSS en el encabezado HTML `<head>` y JavaScript justo antes de cerrar la etiqueta `</body>`.

1. En el módulo **ui.apps** vaya a `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Componente de página de estructura](assets/client-side-libraries/customheaderlibs-html.png)

   Este es el componente `page` que se utiliza para procesar todas las páginas en la implementación de WKND.

1. Abra el archivo `customheaderlibs.html`. Observe las líneas `${clientlib.css @ categories='wknd.base'}`. Esto indica que el CSS para la clientlib con una categoría de `wknd.base` se incluirá a través de este archivo, incluyendo efectivamente **clientlib-base** en el encabezado de todas nuestras páginas.

1. Actualice `customheaderlibs.html` para incluir una referencia a los estilos de fuente de Google que especificamos anteriormente en el módulo **ui.frontend**.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspeccione el archivo `customfooterlibs.html`. Este archivo, como `customheaderlibs.html`, está diseñado para sobrescribirse mediante la implementación de proyectos. Aquí, la línea `${clientlib.js @ categories='wknd.base'}` significa que el JavaScript de **clientlib-base** se incluirá en la parte inferior de todas nuestras páginas.

1. Exporte el componente `page` al servidor de AEM mediante las herramientas para desarrolladores o con sus habilidades con Maven.

1. Vaya a la plantilla Página del artículo en [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Haga clic en el icono **Información de página** y, en el menú, seleccione **Política de página** para abrir el cuadro de diálogo **Política de página**.

   ![Política de página del menú Plantilla de página de artículo](assets/client-side-libraries/template-page-policy.png)

   *Información de página > Política de página*

1. Observe que las categorías de `wknd.dependencies` y `wknd.site` se enumeran aquí. De forma predeterminada, los clientlibs configurados mediante la Política de página se dividen para incluir el CSS en el encabezado de página y el JavaScript en el final del cuerpo. Si lo desea, puede enumerar explícitamente que el JavaScript clientlib se debe cargar en el encabezado de la página. Este es el caso de `wknd.dependencies`.

   ![Política de página del menú Plantilla de página de artículo](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > También es posible hacer referencia a `wknd.site` o `wknd.dependencies` desde el componente de página directamente, utilizando el script `customheaderlibs.html` o `customfooterlibs.html`, como se vio anteriormente para la clientlib `wknd.base`. El uso de la plantilla le da cierta flexibilidad en cuanto a que puede elegir qué clientlibs se utilizan por plantilla. Por ejemplo, si tiene una biblioteca JavaScript muy pesada que solo se va a usar en una plantilla seleccionada.

1. Vaya a la página **LA Skateparks** creada con la **Plantilla de página de artículo**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Debería ver una diferencia en las fuentes.

1. Haga clic en el icono **Información de página** y, en el menú, seleccione **Ver tal y como aparece publicado** para abrir la página de artículos fuera del editor de AEM.

   ![Ver como aparece publicado](assets/client-side-libraries/view-as-published-article-page.png)

1. Vea la fuente de página de [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) y debería poder ver las siguientes referencias clientlib en `<head>`:

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

   Observe que clientlibs utiliza el punto final `/etc.clientlibs` del proxy. También debería ver las siguientes clientlib incluidas en la parte inferior de la página:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Si sigue a la versión 6.5/6.4, las bibliotecas del lado del cliente no se minificarán automáticamente. Consulte la documentación del [Administrador de bibliotecas HTML para habilitar la minimización (recomendado)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Es fundamental en el lado de publicación que las bibliotecas de cliente **no** se proporcionen desde **/apps**, ya que esta ruta debe restringirse por motivos de seguridad utilizando la sección [Filtro de Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). La [allowProxy property](https://docs.adobe.com/content/help/es-ES/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) de la biblioteca del cliente garantiza que el CSS y el JS se proporcionen desde **/etc.clientlibs**.

## Servidor de desarrollo de Webpack: marcado estático {#webpack-dev-static}

En el par anterior de ejercicios, pudimos actualizar varios archivos Sass en el módulo **ui.frontend** y a través de un proceso de compilación, finalmente ver estos cambios reflejados en AEM. A continuación, analizaremos las técnicas que utilizan un [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) para desarrollar rápidamente nuestros estilos front-end frente al HTML **static**.

Esta técnica es útil si la mayoría de los estilos y el código front-end lo realizará un desarrollador de Front-End dedicado que puede no tener acceso fácil a un entorno de AEM. Esta técnica también permite a la FED realizar modificaciones directamente en el HTML, que luego se puede transferir a un desarrollador de AEM para que lo implemente como componentes.

1. Copie la fuente de la página del artículo del parque de patinaje LA en [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Vuelva a abrir el IDE. Pegue el marcado copiado de AEM en `index.html` en el módulo **ui.frontend** debajo de `src/main/webpack/static`.
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

1. Inicie el servidor de desarrollo de webpack desde un nuevo terminal ejecutando el siguiente comando desde el módulo **ui.frontend**:

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

1. Debería ver automáticamente los cambios reflejados automáticamente en el explorador en [http://localhost:8080](http://localhost:8080).

   ![Cambios en el servidor de desarrollo de webpack local](assets/client-side-libraries/local-webpack-dev-server.png)

1. Revise el archivo `/aem-guides-wknd.ui.frontend/webpack.dev.js`. Contiene la configuración del webpack utilizada para iniciar el webpack-dev-server. Tenga en cuenta que reemplaza las rutas `/content` y `/etc.clientlibs` desde una instancia de AEM que se está ejecutando localmente. Así es como están disponibles las imágenes y otros clientlibs (no administrados por el código **ui.frontend**).

   >[!CAUTION]
   >
   > La imagen src del marcado estático apunta a un componente de imagen en vivo en una instancia local de AEM. Las imágenes aparecerán rotas si la ruta a la imagen cambia, si AEM no se inicia o si el explorador no ha iniciado sesión en la instancia local de AEM. Si se entrega a un recurso externo, también es posible reemplazar las imágenes con referencias estáticas.

1. Puede **detener** el servidor de webpack desde la línea de comandos escribiendo `CTRL+C`.

## Servidor de desarrollo de Webpack: Watch y aemsync {#webpack-dev-watch}

Otra técnica es hacer que Node.js observe si hay cambios en los archivos src en el módulo `ui.frontend`. Siempre que cambie un archivo, compilará rápidamente la biblioteca del cliente y utilizará el módulo npm [aemsync](https://www.npmjs.com/package/aemsync) para sincronizar los cambios con un servidor AEM en ejecución.

1. Inicie el servidor de desarrollo de webpack en modo **watch** desde un nuevo terminal ejecutando el siguiente comando desde el módulo **ui.frontend**:

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

1. Vaya al artículo de AEM y los parques de patinaje LA : [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Cambios implementados en AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   Los cambios deben implementarse en AEM. Se produce un ligero retraso y tendrá que actualizar el explorador manualmente para ver las actualizaciones. Sin embargo, es útil ver los cambios directamente en AEM si trabaja con nuevos componentes y creación de cuadros de diálogo.

1. Recupere el cambio en `_variables.scss` y guarde los cambios. Los cambios deben sincronizarse de nuevo con la instancia local de AEM después de un ligero retraso.

1. Detenga el servidor de desarrollo de webpack y realice una compilación completa de Maven desde la raíz del proyecto:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Nuevamente, el módulo `ui.frontend` se compila, se transforma en bibliotecas de clientes y se implementa en AEM a través del módulo `ui.apps`. Sin embargo, esta vez Maven lo hace todo por nosotros.

## Felicitaciones! {#congratulations}

Felicitaciones, la página de artículos ahora tiene algunos estilos coherentes que coinciden con la marca WKND y usted se ha familiarizado con el módulo **ui.frontend**.

### Pasos siguientes {#next-steps}

Obtenga información sobre cómo implementar estilos individuales y reutilizar componentes principales mediante el sistema de estilos de Experience Manager. [Desarrollar con el sistema de estilos ](style-system.md) cubre el uso del sistema de estilos para ampliar los componentes principales con CSS específica de la marca y configuraciones de políticas avanzadas del editor de plantillas.

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama `tutorial/client-side-libraries-solution` de Git.

1. Clona el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consulte la rama `tutorial/client-side-libraries-solution`.

## Herramientas y recursos adicionales {#additional-resources}

### aemfeed {#develop-aemfed}

[****](https://aemfed.io/) aemfedis es una herramienta de código abierto de línea de comandos que se puede utilizar para acelerar el desarrollo del front-end. Está equipado con [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) y [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

En un **aemfeed** de alto nivel está diseñado para escuchar los cambios de archivos dentro del módulo **ui.apps** y sincronizarlos automáticamente directamente con una instancia de AEM en ejecución. En función de los cambios, un explorador local se actualizará automáticamente, acelerando así el desarrollo del front-end. También está construido para funcionar con el rastreador de registros de Sling para mostrar automáticamente cualquier error del lado del servidor directamente en el terminal.

Si está haciendo mucho trabajo dentro del módulo **ui.apps**, modificando los scripts HTL y creando componentes personalizados, **aemfeed** puede ser una herramienta muy poderosa para usar. [Puede encontrar la documentación completa aquí.](https://github.com/abmaonline/aemfed).

### Depuración de bibliotecas del lado del cliente {#debugging-clientlibs}

Con diferentes métodos de **categories** y **embeds** para incluir varias bibliotecas de cliente, puede resultar complicado solucionar problemas. AEM expone varias herramientas para ayudarle con esto. Una de las herramientas más importantes es **Reconstruir bibliotecas de clientes**, lo que obligará a AEM a volver a compilar cualquier archivo LESS y generar el CSS.

* [**Bibliotecas de volcado**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : enumera todas las bibliotecas de cliente registradas en la instancia de AEM.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Salida de prueba**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permite al usuario ver la salida HTML esperada de clientlib incluye en función de la categoría.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validación de dependencias de bibliotecas**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : resalta las dependencias o categorías incrustadas que no se pueden encontrar.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Reconstruir bibliotecas de cliente**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : permite al usuario forzar a AEM a reconstruir todas las bibliotecas de cliente o invalidar la caché de las bibliotecas de cliente. Esta herramienta es especialmente eficaz cuando se desarrolla con LESS, ya que puede obligar a AEM a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y luego realizar una actualización de página en comparación con la reconstrucción de todas las bibliotecas. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![regenerar biblioteca cliente](assets/client-side-libraries/rebuild-clientlibs.png)

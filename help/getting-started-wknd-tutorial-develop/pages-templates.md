---
title: 'Introducción a AEM Sites: Páginas y plantillas'
seo-title: 'Introducción a AEM Sites: Páginas y plantillas'
description: Obtenga información sobre la relación entre un componente de página base y plantillas editables. Comprenda cómo se procesan los componentes principales en el proyecto y aprenda las configuraciones de políticas avanzadas de las plantillas editables para crear una plantilla de página de artículos bien estructurada basada en un simulador de Adobe XD.
sub-product: sitios
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2226'
ht-degree: 2%

---


# Páginas y plantillas {#pages-and-template}

En este capítulo analizaremos la relación entre un componente de página base y plantillas editables. Generaremos una plantilla de artículo sin estilo basada en algunas maquetas de [AdobeXD](https://www.adobe.com/products/xd.html). En el proceso de creación de la plantilla, se tratan los componentes principales y las configuraciones de directiva avanzadas de las plantillas editables.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Proyecto de inicio

Consulte el código de línea base que el tutorial genera:

1. Clona el repositorio [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Compruebe la rama `pages-templates/start`.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout pages-templates/start
   ```

1. Implemente código base en una instancia de AEM local con sus conocimientos Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) o extraer el código localmente cambiando a la rama `pages-templates/solution`.

## Objetivo

1. Inspect es un diseño de página creado en Adobe XD y asignado a Componentes principales.
1. Conocer los detalles de las plantillas editables y cómo se pueden utilizar las políticas para aplicar el control granular del contenido de la página.
1. Conozca cómo se vinculan las plantillas y las páginas

## Qué va a generar {#what-you-will-build}

En esta parte del tutorial, generará una nueva plantilla de página de artículo que se puede utilizar para crear nuevas páginas de artículos y se alinea con una estructura común. La plantilla de página de artículo se basará en diseños y en un kit de interfaz de usuario creado en AdobeXD. Este capítulo se centra únicamente en la creación de la estructura o esqueleto de la plantilla. No se implementarán estilos, pero la plantilla y las páginas funcionarán.

![Diseño de página de artículos y versión sin estilo](assets/pages-templates/what-you-will-build.png)

## Planificación de UI con Adobe XD {#adobexd}

En la mayoría de los casos, la planificación de un nuevo sitio web inicio con maquetas y diseños estáticos. [Adobe ](https://www.adobe.com/products/xd.html) XD es una herramienta de diseño que crea experiencias de usuario. A continuación, inspeccionaremos un kit de IU y las maquetas para ayudarle a planificar la estructura de la plantilla de página de artículos.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

Descargue el [archivo de diseño de artículo WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd).

## Crear un encabezado y un pie de página con fragmentos de experiencia {#experience-fragments}

Una práctica habitual al crear contenido global, como un encabezado o pie de página, es utilizar un [fragmento de experiencia](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Los fragmentos de experiencia nos permiten combinar varios componentes para crear un único componente con referencia. Los fragmentos de experiencia tienen la ventaja de admitir la administración de varios sitios y nos permite administrar diferentes encabezados y pies de página por configuración regional.

A continuación, se actualizará el fragmento de experiencias que se va a utilizar como Encabezado y Pie de página para añadir el logotipo WKND.

>[!VIDEO](https://video.tv.adobe.com/v/30215/?quality=12&learn=on)

>[!NOTE]
>
> ¿Los fragmentos de experiencias tienen un aspecto diferente al del vídeo? Intente eliminarlos y vuelva a instalar la base de código del proyecto de inicio.

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Actualice el encabezado del fragmento de experiencias ubicado en [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html) para incluir el logotipo WKND Dark.

   ![Logotipo oscuro WKND](assets/pages-templates/wknd-logo-dk.png)

   *Logo de WKND Dark*

1. Actualice el encabezado del fragmento de experiencias ubicado [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html) para incluir el logotipo de la luz WKND.

   ![Logotipo de luz WKND](assets/pages-templates/wknd-logo-light.png)

   *Logo de WKND Light*

## Creación de la plantilla de página de artículo

Al crear una página, debe seleccionar una plantilla, que se utilizará como base para crear la página nueva. La plantilla define la estructura de la página resultante, el contenido inicial y los componentes permitidos.

Existen tres áreas principales de [Plantillas editables](https://docs.adobe.com/content/help/es-ES/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Estructura** : define los componentes que forman parte de la plantilla. Los autores de contenido no podrán editarlos.
1. **Contenido**  inicial: define los componentes con los que se realizará el inicio de la plantilla; los autores de contenido pueden editarlos o eliminarlos
1. **Políticas** : define las configuraciones sobre cómo se comportarán los componentes y las opciones que tendrán disponibles los autores.

Lo siguiente que haremos es crear la plantilla de página de artículo. Esto ocurrirá en una instancia local de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30217/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Vaya a la carpeta de plantillas de sitios WKND: **Herramientas** > **General** > **Plantillas** > **Sitio WKND**
1. Cree una nueva plantilla con el tipo de plantilla **Página vacía del sitio de WKND** con el título **Plantilla de página de artículo**
1. En el modo **Estructura**, configure la plantilla para que incluya los siguientes elementos:

   * Encabezado del fragmento de experiencias
   * Imagen
   * Ruta de navegación
   * Contenedor: 8 columnas de ancho Escritorio, 12 columnas de ancho Tablet, Móvil
   * Contenedor: 4 columnas de ancho, 12 columnas de ancho Tablet, Móvil
   * Pie de página del fragmento de experiencias

   ![Modo de estructuraPlantilla de página de artículo](assets/pages-templates/article-page-template-structure.png)

   *Estructura - Plantilla de página de artículo*

1. Cambie al modo **Contenido inicial** y agregue los siguientes componentes como contenido inicial:

   * **Contenedor principal**
      * Título: tamaño predeterminado de H1
      * Título - *&quot;Por nombre de autor&quot;* con un tamaño H4
      * Texto - vacío
   * **Contenedor lateral**
      * Título: *&quot;Compartir esta historia&quot;* con un tamaño H5
      * Compartir en redes sociales
      * Separador
      * Descargar
      * Lista

   ![Modo de contenido inicial Plantilla de página de artículo](assets/pages-templates/article-page-template-initialcontent.png)

   *Contenido inicial: plantilla de página de artículo*

1. Actualice las **Propiedades de página iniciales** para permitir que el usuario comparta las propiedades **Facebook** y **Pinterest**.
1. Cargue una imagen en las propiedades **de la plantilla de página de artículo** para identificarla fácilmente:

   ![Miniatura de plantilla de página de artículo](assets/pages-templates/article-page-template-thumbnail.png)

   *Miniatura de plantilla de página de artículo*

1. Habilite la **Plantilla de página de artículo** en la carpeta [Plantillas de sitio WKND](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd/settings/wcm/templates).

## Crear una página de artículo

Ahora que tenemos una plantilla, creemos una nueva página con esa plantilla.

1. Descargue el siguiente paquete zip, [WKND-PagesTemplates-DAM-Assets.zip](assets/pages-templates/WKND-PagesTemplates-DAM-Assets.zip) e instálelo mediante [Administrador de paquetes CRX](http://localhost:4502/crx/packmgr/index.jsp).

   El paquete anterior instalará varias imágenes y recursos debajo de `/content/dam/wknd/en/magazine/la-skateparks` para usarlos para rellenar una página de artículos en pasos posteriores.

   *Las imágenes y los activos del paquete anterior son cortesía de  [Unsplash.com](https://unsplash.com/).*

   ![Recursos DAM de muestra](assets/pages-templates/sample-assets-la-skatepark.png)

1. Cree una nueva página, debajo de **WKND** > **US** > **en**, con el nombre **Revista**. Utilice la plantilla **Página de contenido**.

   Esta página agregará alguna estructura a nuestro sitio y nos permitirá ver el componente de ruta de navegación en acción.

1. A continuación, cree una nueva página, debajo de **WKND** > **US** > **en** > **Revista**. Utilice la plantilla **Página del artículo**. Utilice un título de **Guía definitiva para los parques de patinaje LA** y un nombre de **guía-la-skateparks**.

   ![Página del artículo creada inicialmente](assets/pages-templates/create-article-page-nocontent.png)

1. Rellene la página con contenido para que coincida con las maquetas inspeccionadas en [UI Planning con AdobeXD](#adobexd). El texto del artículo de muestra se puede [descargar aquí](assets/pages-templates/la-skateparks-copy.txt). Debería poder crear algo similar a esto:

   ![Página del artículo rellenada](assets/pages-templates/article-page-unstyled.png)

   >[!NOTE]
   >
   > El componente Imagen en la parte superior de la página se puede editar pero no eliminar. El componente de ruta de exploración aparece en la página pero no se puede editar ni eliminar.

## Inspect la estructura de nodos {#node-structure}

En este punto, la página del artículo está claramente sin estilo. Sin embargo, la estructura básica está establecida. Luego veremos la estructura de nodos de la página del artículo para comprender mejor la función de la plantilla y del componente de página responsable de procesar el contenido.

Podemos hacerlo con la herramienta CRXDE-Lite en una instancia de AEM local.

1. Abra [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) y utilice la navegación de árbol para desplazarse a `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Haga clic en el nodo `jcr:content` debajo de la página `la-skateparks` y vista las propiedades:

   ![Propiedades de contenido de JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe el valor de `cq:template`, que apunta a `/conf/wknd/settings/wcm/templates/article-page`, la plantilla de página de artículo que hemos creado anteriormente.

   Observe también el valor de `sling:resourceType`, que apunta a `wknd/components/structure/page`. Es el componente de página creado por el arquetipo del proyecto de AEM y es responsable de procesar la página en función de la plantilla.

1. Expanda el nodo `jcr:content` debajo de `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` y vista la jerarquía de nodos:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Debería poder asignar cada uno de los nodos de forma flexible a los componentes creados. Consulte si puede identificar los diferentes Contenedores de diseño utilizados inspeccionando los nodos con el prefijo `responsivegrid`.

1. A continuación, inspeccione el componente de página en `/apps/wknd/components/structure/page`. Vista de las propiedades del componente en CRXDE Lite:

   ![Propiedades del componente de página](assets/pages-templates/page-component-properties.png)

   Tenga en cuenta que el componente de página se encuentra debajo de una carpeta denominada **estructura**. Se trata de una convención que corresponde al modo de estructura del Editor de plantillas y se utiliza para indicar que el componente de página no es algo con lo que los autores interactuarán directamente.

   Tenga en cuenta que solo hay 2 secuencias de comandos HTL, `customfooterlibs.html` y `customheaderlibs.html` debajo del componente de página. Entonces, ¿cómo representa este componente la página?

   Observe la propiedad `sling:resourceSuperType` y el valor de `core/wcm/components/page/v2/page`. Esta propiedad permite que el componente de página de WKND herede toda la funcionalidad del componente de página del componente principal. Este es el primer ejemplo de algo llamado [Patrón de componentes proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Puede encontrar más información [aquí.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect es otro componente dentro de los componentes WKND, el componente `Breadcrumb` ubicado en: `/apps/wknd/components/content/breadcrumb`. Observe que se puede encontrar la misma propiedad `sling:resourceSuperType`, pero esta vez señala a `core/wcm/components/breadcrumb/v2/breadcrumb`. Este es otro ejemplo de uso del patrón de componentes proxy para incluir un componente principal. De hecho, todos los componentes de la base de código WKND son proxies de AEM componentes principales (excepto nuestro famoso componente HelloWorld). Se recomienda intentar reutilizar la mayor parte posible de la funcionalidad de los componentes principales *antes de que* escriba código personalizado.

1. A continuación, inspeccione la página de componentes principales en `/apps/core/wcm/components/page/v2/page` mediante CRXDE Lite:

   ![Página de componentes principales](assets/pages-templates/core-page-component-properties.png)

   Observe que se incluyen muchas más secuencias de comandos debajo de esta página. La página de componentes principales contiene muchas funciones. Esta funcionalidad se divide en varios scripts para facilitar el mantenimiento y la lectura. Puede rastrear la inclusión de las secuencias de comandos HTL abriendo `page.html` y buscando `data-sly-include`:

   ```html
   <!--/* /apps/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
           data-sly-use.head="head.html"
           data-sly-use.footer="footer.html"
           data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}">
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   La otra razón para dividir el HTL en varios scripts es permitir que los componentes proxy anulen los scripts individuales para implementar la lógica empresarial personalizada. Las secuencias de comandos HTL, `customfooterlibs.html` y `customheaderlibs.html`, se crean con el propósito explícito de ser anuladas por la implementación de proyectos.

   Puede obtener más información sobre cómo la plantilla editable influye en la representación de la página de contenido [leyendo este artículo](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/templates/page-templates-editable.html#resultant-content-pages).

1. Inspect es el otro componente principal, como la ruta de exploración en `/apps/core/wcm/components/breadcrumb/v2/breadcrumb`. Vista la secuencia de comandos `breadcrumb.html` para comprender cómo se genera finalmente el marcado para el componente de ruta de navegación.

## Guardar configuraciones en el control de código fuente {#configuration-persistence}

En muchos casos, especialmente al principio de un proyecto AEM, es valioso mantener configuraciones, como plantillas y políticas de contenido relacionadas, para el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.

Por ahora, trataremos las plantillas como otras partes de código y sincronizaremos la **Plantilla de página de artículo** como parte del proyecto. Hasta ahora hemos **insertado** código de nuestro proyecto de AEM a una instancia local de AEM. La **Plantilla de página de artículo** se creó directamente en una instancia local de AEM, por lo que necesitamos **extraer** o importar la plantilla en nuestro proyecto AEM. El módulo **ui.content** se incluye en el proyecto de AEM para este propósito específico.

Los siguientes pasos se llevarán a cabo mediante el IDE de Eclipse, pero podrían estar haciendo uso de cualquier IDE que haya configurado para **extraer** o importar contenido desde una instancia local de AEM.

1. En el IDE de Eclipse, asegúrese de que se ha iniciado un servidor que conecta AEM complemento de herramienta para desarrolladores con la instancia local de AEM y de que el módulo **ui.content** se ha agregado a la configuración del servidor.

   ![Conexión con Eclipse Server](assets/pages-templates/eclipse-server-started.png)

1. Expanda el módulo **ui.content** en el explorador de proyectos. Expanda la carpeta `src` (la que tiene el icono de pequeño gob) y navegue hasta `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Haga clic con el botón derecho ] en el  `templates` nodo y seleccione  **Importar desde el servidor...**:

   ![Plantilla de importación Eclipse](assets/pages-templates/eclipse-import-templates.png)

   Confirme el cuadro de diálogo **Importar del repositorio** y haga clic en **Finalizar**. Ahora debería ver la `article-page-template` debajo de la carpeta `templates`.

1. Repita los pasos para importar contenido pero seleccione el nodo **directivas** ubicado en `/conf/wknd/settings/wcm/policies`.

   ![Directivas de importación de eclipse](assets/pages-templates/policies-article-page-template.png)

1. Inspect el archivo `filter.xml` ubicado en `src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   El archivo `filter.xml` es responsable de identificar las rutas de los nodos que se instalarán con el paquete. Observe el `mode="merge"` en cada uno de los filtros que indica que el contenido existente no se modificará, solo se agregará contenido nuevo. Dado que los autores de contenido pueden estar actualizando estas rutas, es importante que una implementación de código **no** sobrescriba el contenido. Consulte la [documentación de FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obtener más información sobre cómo trabajar con elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` y `ui.apps/src/main/content/META-INF/vault/filter.xml` para comprender los diferentes nodos administrados por cada módulo.

   >[!WARNING]
   >
   > A fin de garantizar implementaciones coherentes para el sitio de referencia WKND, algunas ramas del proyecto están configuradas de manera que `ui.content` sobrescriba cualquier cambio en el JCR. Esto se hace por diseño, es decir, para las ramas de soluciones, ya que el código o los estilos se escribirán para políticas específicas.

## Felicitaciones! {#congratulations}

Enhorabuena, acaba de crear una nueva plantilla y página con Adobe Experience Manager Sites.

### Próximos pasos {#next-steps}

En este punto, la página del artículo está claramente sin estilo. Siga el tutorial [Client-Side Libraries and Front-end Workflow](client-side-libraries.md) para conocer las prácticas recomendadas para incluir CSS y Javascript para aplicar estilos globales al sitio e integrar una compilación dedicada del front-end.

Vista el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código de forma local en la plataforma Git `pages-templates/solution`.

1. Clona el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Compruebe la rama `pages-templates/solution`.

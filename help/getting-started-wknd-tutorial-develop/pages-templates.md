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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1724'
ht-degree: 1%

---


# Páginas y plantillas {#pages-and-template}

En este capítulo analizaremos la relación entre un componente de página base y plantillas editables. Generaremos una plantilla de artículo sin estilo basada en algunas maquetas de [AdobeXD](https://www.adobe.com/products/xd.html). En el proceso de creación de la plantilla, se tratan los componentes principales y las configuraciones de directiva avanzadas de las plantillas editables.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

Consulte el código de línea base que el tutorial genera:

1. Consulte la rama `tutorial/pages-templates-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) o extraer el código localmente cambiando a la rama `tutorial/pages-templates-solution`.

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

**Descargue el archivo [ de diseño de artículos ](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)** WKND.

## Creación de la plantilla de página de artículo

Al crear una página, debe seleccionar una plantilla, que se utilizará como base para crear la página nueva. La plantilla define la estructura de la página resultante, el contenido inicial y los componentes permitidos.

Existen tres áreas principales de [Plantillas editables](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Estructura** : define los componentes que forman parte de la plantilla. Los autores de contenido no podrán editarlos.
1. **Contenido**  inicial: define los componentes con los que se realizará el inicio de la plantilla; los autores de contenido pueden editarlos o eliminarlos
1. **Políticas** : define las configuraciones sobre cómo se comportarán los componentes y las opciones que tendrán disponibles los autores.

A continuación, cree una nueva plantilla en AEM que coincida con la estructura de las maquetas. Esto ocurrirá en una instancia local de AEM. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

## Actualizar el encabezado y el pie de página con fragmentos de experiencia {#experience-fragments}

Una práctica habitual al crear contenido global, como un encabezado o pie de página, es utilizar un [fragmento de experiencia](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Fragmentos de experiencia, permite a los usuarios combinar varios componentes para crear un único componente con referencia. Los fragmentos de experiencia tienen la ventaja de admitir la administración de varios sitios y la [localización](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure).

El arquetipo del proyecto AEM generó un encabezado y un pie de página. A continuación, actualice los fragmentos de experiencias para que coincidan con los bocetos. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Descargue e instale el paquete de contenido de muestra **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**.

## Crear una página de artículo

A continuación, cree una nueva página con la plantilla Página del artículo. Cree el contenido de la página para que coincida con las maquetas del sitio. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

## Inspect la estructura de nodos {#node-structure}

En este punto, la página del artículo está claramente sin estilo. Sin embargo, la estructura básica está establecida. A continuación, inspeccione la estructura de nodos de la página del artículo para comprender mejor la función de la plantilla, la página y los componentes.

Utilice la herramienta CRXDE-Lite en una instancia de AEM local para vista de la estructura de nodos subyacente.

1. Abra [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) y utilice la navegación de árbol para desplazarse a `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Haga clic en el nodo `jcr:content` debajo de la página `la-skateparks` y vista las propiedades:

   ![Propiedades de contenido de JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe el valor de `cq:template`, que apunta a `/conf/wknd/settings/wcm/templates/article-page`, la plantilla de página de artículo que hemos creado anteriormente.

   Observe también el valor de `sling:resourceType`, que apunta a `wknd/components/page`. Es el componente de página creado por el arquetipo del proyecto de AEM y es responsable de procesar la página en función de la plantilla.

1. Expanda el nodo `jcr:content` debajo de `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` y vista la jerarquía de nodos:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Debería poder asignar cada uno de los nodos de forma flexible a los componentes creados. Consulte si puede identificar los diferentes Contenedores de diseño utilizados inspeccionando los nodos con el prefijo `container`.

1. A continuación, inspeccione el componente de página en `/apps/wknd/components/page`. Vista de las propiedades del componente en CRXDE Lite:

   ![Propiedades del componente de página](assets/pages-templates/page-component-properties.png)

   Tenga en cuenta que solo hay 2 secuencias de comandos HTL, `customfooterlibs.html` y `customheaderlibs.html` debajo del componente de página. *Entonces, ¿cómo representa este componente la página?*

   La propiedad `sling:resourceSuperType` apunta a `core/wcm/components/page/v2/page`. Esta propiedad permite que el componente de página de WKND herede **todo** de la funcionalidad del componente de página del componente principal. Este es el primer ejemplo de algo llamado [Patrón de componentes proxy](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Puede encontrar más información [aquí.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect es otro componente dentro de los componentes WKND, el componente `Breadcrumb` ubicado en: `/apps/wknd/components/breadcrumb`. Observe que se puede encontrar la misma propiedad `sling:resourceSuperType`, pero esta vez señala a `core/wcm/components/breadcrumb/v2/breadcrumb`. Este es otro ejemplo de uso del patrón de componentes proxy para incluir un componente principal. De hecho, todos los componentes de la base de código WKND son proxies de AEM componentes principales (excepto nuestro famoso componente HelloWorld). Se recomienda intentar reutilizar la mayor parte posible de la funcionalidad de los componentes principales *antes de que* escriba código personalizado.

1. A continuación, inspeccione la página de componentes principales en `/libs/core/wcm/components/page/v2/page` mediante CRXDE Lite:

   >[!NOTE]
   >
   > En AEM 6.5/6.4, los componentes principales se encuentran en `/apps/core/wcm/components`. En AEM como Cloud Service, los componentes principales se encuentran en `/libs` y se actualizan automáticamente.

   ![Página de componentes principales](assets/pages-templates/core-page-component-properties.png)

   Observe que se incluyen muchas más secuencias de comandos debajo de esta página. La página de componentes principales contiene muchas funciones. Esta funcionalidad se divide en varios scripts para facilitar el mantenimiento y la lectura. Puede rastrear la inclusión de las secuencias de comandos HTL abriendo `page.html` y buscando `data-sly-include`:

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   La otra razón para dividir el HTL en varios scripts es permitir que los componentes proxy anulen los scripts individuales para implementar la lógica empresarial personalizada. Las secuencias de comandos HTL, `customfooterlibs.html` y `customheaderlibs.html`, se crean con el propósito explícito de ser anuladas por la implementación de proyectos.

   Puede obtener más información sobre cómo la plantilla editable influye en la representación de la página de contenido [leyendo este artículo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect es el otro componente principal, como la ruta de exploración en `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Vista la secuencia de comandos `breadcrumb.html` para comprender cómo se genera finalmente el marcado para el componente de ruta de navegación.

## Guardar configuraciones en el control de código fuente {#configuration-persistence}

En muchos casos, especialmente al principio de un proyecto AEM, es valioso mantener configuraciones, como plantillas y políticas de contenido relacionadas, para el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.

Por ahora, trataremos las plantillas como otras partes de código y sincronizaremos la **Plantilla de página de artículo** como parte del proyecto. Hasta ahora hemos **insertado** código de nuestro proyecto de AEM a una instancia local de AEM. La **Plantilla de página de artículo** se creó directamente en una instancia local de AEM, por lo que necesitamos **importar** la plantilla en nuestro proyecto AEM. El módulo **ui.content** se incluye en el proyecto de AEM para este propósito específico.

Los siguientes pasos se llevarán a cabo usando el IDE de VSCode usando el complemento [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) pero podría estar utilizando cualquier IDE que haya configurado para **importar** o importar contenido desde una instancia local de AEM.

1. En VSCode, abra el proyecto `aem-guides-wknd`.

1. Expanda el módulo **ui.content** en el explorador de proyectos. Expanda la carpeta `src` y vaya a `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Haga clic con el botón derecho ] en la  `templates` carpeta y seleccione  **Importar desde AEM servidor**:

   ![Plantilla de importación VSCode](assets/pages-templates/vscode-import-templates.png)

   Se debe importar `article-page` y actualizar también las plantillas `page-content`, `xf-web-variation`.

   ![Plantillas actualizadas](assets/pages-templates/updated-templates.png)

1. Repita los pasos para importar contenido pero seleccione la carpeta **políticas** ubicada en `/conf/wknd/settings/wcm/policies`.

   ![Directivas de importación de VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect el archivo `filter.xml` ubicado en `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

Vista el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código de forma local en la plataforma Git `tutorial/pages-templates-solution`.

1. Clona el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Compruebe la rama `tutorial/pages-templates-solution`.

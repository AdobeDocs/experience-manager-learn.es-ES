---
title: 'Introducción a AEM Sites: Páginas y plantillas'
description: Obtenga información acerca de la relación entre un componente de página base y las plantillas editables. Descubra cómo se procesan como proxy los componentes principales en el proyecto. Conozca las configuraciones de directiva avanzadas de las plantillas editables para crear una plantilla de página de artículo bien estructurada basada en una maqueta de Adobe XD.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '2898'
ht-degree: 100%

---

# Páginas y plantillas {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

En este capítulo, vamos a explorar la relación entre un componente de página base y las plantillas editables. Aprenda a crear una plantilla de artículo sin estilo basada en algunas maquetas de [Adobe XD](https://helpx.adobe.com/es/support/xd.html). En el proceso de creación de la plantilla, se tratan los componentes principales y las configuraciones de política avanzadas de las plantillas editables.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para consultar el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la rama `tutorial/pages-templates-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
   ```

1. Implemente una base de código en una instancia de AEM local usando sus conocimientos de Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) o consultarlo localmente cambiando a la rama `tutorial/pages-templates-solution`.

## Objetivo

1. Inspeccione un diseño de página creado en Adobe XD y asígnelo a los componentes principales.
1. Conozca los detalles de las plantillas editables y cómo se pueden utilizar las políticas para aplicar un control granular del contenido de la página.
1. Descubra cómo se vinculan las plantillas y las páginas

## Lo que va a generar {#what-build}

En esta parte del tutorial, creará una nueva plantilla de página de artículo que se puede utilizar para crear páginas de artículo y se alineará con una estructura común. La plantilla de página de artículo se basa en diseños y un kit de interfaz de usuario producido en Adobe XD. Este capítulo solo se centra en crear la estructura o el esqueleto de la plantilla. No se han implementado estilos, pero la plantilla y las páginas funcionan correctamente.

![Diseño de página de artículo y versión sin estilo](assets/pages-templates/what-you-will-build.png)

## Planificación de la IU con Adobe XD {#adobexd}

Por lo general, la planificación de un nuevo sitio web comienza con maquetas y diseños estáticos. [Adobe XD](https://helpx.adobe.com/es/support/xd.html) es una herramienta de diseño para mejorar la experiencia del usuario. A continuación, vamos a inspeccionar un kit de interfaz de usuario y maquetas para ayudar a planificar la estructura de la plantilla de página de artículo.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Descargar el archivo de diseño de artículo de [WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> También hay disponible [un kit de interfaz de usuario para componentes principales de AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=es) genérico como punto de partida para proyectos personalizados.

## Crear la plantilla de página de artículo

Al crear una página, debe seleccionar una plantilla, que se utiliza como base para crear la nueva página. La plantilla define la estructura de la página resultante, el contenido inicial y los componentes permitidos.

Hay tres áreas principales de [plantillas editables](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=es):

1. **Estructura**: define los componentes que forman parte de la plantilla. Los autores de contenido no pueden editarlos.
1. **Contenido inicial**: define componentes con los que comienza la plantilla, que los autores de contenido pueden editar o eliminar
1. **Directivas**: define configuraciones sobre cómo se comportan los componentes y qué opciones tienen los autores.

A continuación, cree una plantilla en AEM que coincida con la estructura de las maquetas. Esto ocurre en una instancia local de AEM. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Pasos de alto nivel para el vídeo anterior:

### Configuraciones de la estructura

1. Cree una plantilla con el **Tipo de plantilla de página**, denominada **Página de artículo**.
1. Cambie a modo **Estructura**.
1. Añada un componente **Fragmento de experiencia** para que actúe como **Encabezado** en la parte superior de la plantilla.
   * Configure el componente para que apunte a `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Establezca la directiva en **Encabezado de página** y asegúrese de que el **Elemento predeterminado** está establecido en `header`. El elemento `header` se ha segmentado con CSS en el capítulo siguiente.
1. Añada un componente de **Fragmento de experiencia** para que actúe como **Pie de página** en la parte inferior de la plantilla.
   * Configure el componente para que apunte a `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Establezca la directiva en **Pie de página** y asegúrese de que el **Elemento predeterminado** está establecido en `footer`. El elemento `footer` se ha segmentado con CSS en el capítulo siguiente.
1. Bloquee el contenedor **main** que se incluyó cuando se creó inicialmente la plantilla.
   * Establezca la directiva en **Página principal** y asegúrese de que el **Elemento predeterminado** está establecido en `main`. El elemento `main` se ha segmentado con CSS en el capítulo siguiente.
1. Añada un componente **Imagen** al contenedor **principal**.
   * Desbloquee el componente **Imagen**.
1. Añada un componente **Ruta de exploración** debajo del componente **Imagen** en el contenedor principal.
   * Cree una directiva para el componente **Ruta de exploración** llamada **Página de artículo - Ruta de exploración**. Establezca el **nivel de inicio de navegación** en **4**.
1. Añada un componente **Contenedor** debajo del componente **Ruta de exploración** y dentro del contenedor **principal**. Actúa como **contenedor de contenido** para la plantilla.
   * Desbloquee el contenedor **Contenido**.
   * Establezca la directiva en **Contenido de la página**.
1. Añada otro componente **Contenedor** debajo del **Contenedor de contenido**. Actúa como el contenedor de **Carril lateral** para la plantilla.
   * Desbloquee el contenedor de **Carril lateral**.
   * Cree una directiva denominada **Página de artículo - Carril lateral**.
   * Configure los **Componentes permitidos** en el **Proyecto de sitios WKND - Contenido** para que incluyan: **Botón**, **Descargar**, **Imagen**, **Lista**, **Separador**, **Uso compartido de medios sociales**, **Texto** y **Título**.
1. Actualice la directiva del contenedor raíz de página. Este es el contenedor exterior de la plantilla. Establezca la directiva en **Raíz de página**.
   * En **Configuración de contenedor**, establezca **Diseño** en **Cuadrícula interactiva**.
1. Active el modo diseño para el **contenedor de contenido**. Arrastre el controlador de derecha a izquierda y reduzca el contenedor a ocho columnas de ancho.
1. Active el modo de diseño para el **contenedor del carril lateral**. Arrastre el controlador de derecha a izquierda y reduzca el tamaño del contenedor a cuatro columnas de ancho. A continuación, arrastre el controlador izquierdo de izquierda a derecha una columna para que el contenedor tenga 3 columnas de ancho y deje un espacio de 1 columna entre el **contenedor de contenido**.
1. Abra el emulador móvil y cambie a un punto de interrupción móvil. Vuelva a activar el modo de diseño y haga que el **contenedor de contenido** y el **contenedor de carril lateral** tengan el ancho completo de la página. Esto apila los contenedores verticalmente en el punto de interrupción móvil.
1. Actualice la directiva del componente **Texto** en el **contenedor de contenido**.
   * Establezca la directiva en **Texto de contenido**.
   * En **Complementos** > **Estilos de párrafo**, marque **Habilitar estilos de párrafo** y asegúrese de que la opción **Cita en bloque** esté habilitada.

### Configuraciones iniciales de contenido

1. Cambie al modo **Contenido inicial**.
1. Añada un componente **Título** al **Contenedor de contenido**. Actúa como título del artículo. Cuando se deja vacío, muestra automáticamente el título de la página actual.
1. Añada un segundo componente **Título** debajo del primer componente Título.
   * Configure el componente con el texto: “Por autor”. Es un marcador de posición de texto.
   * Establezca el tipo en `H4`.
1. Añada un componente **Texto** debajo del componente Título de **Por autor**.
1. Añada un componente **Título** al **Contenedor de carril lateral**.
   * Configure el componente con el texto: “Compartir esta historia”.
   * Establezca el tipo en `H5`.
1. Añada un componente **Compartir en redes sociales** debajo del componente Título de **Compartir esta historia**.
1. Añada un componente **Separador** debajo del componente **Compartir en redes sociales**.
1. Añada un componente **Descarga** debajo del componente **Separador**.
1. Añada un componente **Lista** debajo del componente **Descarga**.
1. Actualice las **propiedades de la página inicial** para la plantilla.
   * En **Redes sociales** > **Compartido en redes sociales**, marque **Facebook** y **Pinterest**.

### Habilitar la plantilla y añadir una miniatura

1. Ver la plantilla en la consola Plantilla yendo a [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Habilite** la plantilla Página de artículo.
1. Edite las propiedades de la plantilla Página de artículo y cargue la siguiente miniatura para identificar rápidamente las páginas creadas con la plantilla Página de artículo:

   ![Miniatura de la plantilla Página de artículo](assets/pages-templates/article-page-template-thumbnail.png)

## Actualizar el encabezado y el pie de página con fragmentos de experiencias {#experience-fragments}

Una práctica común al crear contenido global, como un encabezado o pie de página, es usar un [fragmento de experiencia](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=es). Los fragmentos de experiencia permiten a los usuarios combinar varios componentes para crear un único componente referenciable. Los fragmentos de experiencia tienen la ventaja de admitir la administración de varios sitios y la [localización](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=es).

El arquetipo del proyecto de AEM generó un encabezado y un pie de página. A continuación, actualice los fragmentos de experiencia para que coincidan con las maquetas. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Pasos de alto nivel para el vídeo anterior:

1. Descargue el paquete de contenido de ejemplo **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Cargar e instalar el paquete de contenido mediante el Administrador de paquetes en [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Actualizar la plantilla Variación web, que es la plantilla utilizada para los fragmentos de experiencia en [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html?lang=es)
   * Actualice la directiva del componente **Contenedor** en la plantilla.
   * Establezca la directiva en **Raíz XF**.
   * En **Componentes permitidos**, seleccione el grupo de componentes **Proyecto de sitios WKND - Estructura** para que incluya los componentes **Navegación de idioma**, **Navegación** y **Búsqueda rápida**.

### Actualizar fragmento de experiencia de encabezado

1. Abra el fragmento de experiencia que representa el encabezado en [http://localhost:4502/editor.html/content/experience-fragments/wknd/es/es/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configure la raíz **Contenedor** del fragmento. Este es el **contenedor** más externo.
   * Definir el **diseño** en **Cuadrícula interactiva**
1. Añada el **logotipo oscuro de WKND** como imagen a la parte superior del **Contenedor**. El logotipo se incluyó en el paquete instalado en un paso anterior.
   * Modifique el diseño del **logotipo oscuro de WKND** para que tenga **dos** columnas de ancho. Arrastre los controladores de derecha a izquierda.
   * Configure el logotipo con **Texto alternativo** de “Logotipo de WKND”.
   * Configure el logotipo a **Vínculo** a `/content/wknd/us/en` la página de inicio.
1. Configure el componente **Navegación** que ya se ha colocado en la página.
   * Establezca **Excluir niveles de raíz** en **1**.
   * Establezca la **profundidad de la estructura de navegación** en **1**.
   * Modifique el diseño del componente **Navegación** para que tenga **ocho** columnas de ancho. Arrastre los controladores de derecha a izquierda.
1. Quite el componente **Navegación de idioma**.
1. Modifique el diseño del componente **Búsqueda** para que tenga **dos** columnas de ancho. Arrastre los controladores de derecha a izquierda. Ahora todos los componentes deben alinearse horizontalmente en una sola fila.

### Actualizar pie de página fragmento de experiencia

1. Abra el fragmento de experiencia que procesa el pie de página en [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configure la raíz **Contenedor** del fragmento. Este es el **contenedor** más externo.
   * Establezca el **Diseño** en **Cuadrícula interactiva**
1. Añada el **logotipo WKND Light** como imagen a la parte superior del **Contenedor**. El logotipo se incluyó en el paquete instalado en un paso anterior.
   * Modifique el diseño del **logotipo ligero de WKND** para que tenga **dos** columnas de ancho. Arrastre los controladores de derecha a izquierda.
   * Configure el logotipo con el **Texto alternativo** de “WKND Logo Light”.
   * Configure el logotipo en **Vínculo** a `/content/wknd/us/en` la página de inicio.
1. Añada un componente de **Navegación** debajo del logotipo. Configure el componente **Navegación**:
   * Establezca **Excluir niveles de raíz** en **1**.
   * Desmarque **Recopilar todas las páginas secundarias**.
   * Establezca la **Profundidad de la estructura de navegación** en **1**.
   * Modifique el diseño del componente **Navegación** para que tenga **ocho** columnas de ancho. Arrastre los controladores de derecha a izquierda.

## Creación de una página de artículo

A continuación, cree una página con la plantilla Página de artículo. Cree el contenido de la página para que coincida con las maquetas del sitio. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Pasos de alto nivel para el vídeo anterior:

1. Vaya a la consola Sitios en [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/es/es/magazine).
1. Cree una página debajo de **WKND** > **US** > **EN** > **Magazine**.
   * Elija la plantilla **Página de artículo**.
   * En **Propiedades** establezca el **Título** en “Guía definitiva para parques de patinaje de Los Ángeles”
   * Establezca **Nombre** en “guía-la-parques-de-patinaje”
1. Reemplace el título de **Del autor** por el texto “Stacey Roswells”.
1. Actualice el componente **Texto** para incluir un párrafo para rellenar el artículo. Puede usar el siguiente archivo de texto como copia: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Añada otro componente **Texto**.
   * Actualice el componente para incluir la cita: “No hay mejor lugar para hacer shredding que Los Ángeles”.
   * Edite el Editor de texto enriquecido en modo de pantalla completa y modifique la cita anterior para utilizar el elemento **Bloque de cita**.
1. Siga rellenando el cuerpo del artículo para que coincida con las maquetas.
1. Configure el componente **Descargar** para que use una versión PDF del artículo.
   * En **Descargar** > **Propiedades**, haga clic en la casilla de verificación para **obtener el título del recurso DAM**.
   * Establezca **Descripción** en: “Obtener la historia completa”.
   * Establezca **Texto de acción** en: “Descargar PDF”.
1. Configure el componente **Lista**.
   * En **Configuración de lista** > **Lista de compilación que usa**, seleccione **Páginas secundarias**.
   * Establezca la **Página principal** en `/content/wknd/us/en/magazine`.
   * En **Configuración de elemento** marque **Vincular elementos** y marque **Mostrar fecha**.

## Inspeccione la estructura del nodo {#node-structure}

En este punto, es evidente que la página del artículo no tiene estilo. Sin embargo, la estructura básica está en su sitio. A continuación, inspeccione la estructura de nodos de la página de artículo para comprender mejor la función de la plantilla, la página y los componentes.

Utilice la herramienta CRXDE-Lite en una instancia de AEM local para ver la estructura de nodos subyacente.

1. Abra [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/es/es/magazine/guide-la-skateparks/jcr%3Acontent) y utilice la navegación de árbol para navegar hasta `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Haga clic en el nodo `jcr:content` debajo de la página `la-skateparks` y vea las propiedades:

   ![Propiedades del contenido JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe el valor de `cq:template`, que apunta a `/conf/wknd/settings/wcm/templates/article-page`, la plantilla de página de artículo creada anteriormente.

   Observe también el valor de `sling:resourceType`, que apunta a `wknd/components/page`. Este es el componente de página creado por el arquetipo de proyecto de AEM y es responsable de procesar la página basada en la plantilla.

1. Expanda el nodo `jcr:content` por debajo de `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` y vea la jerarquía de nodos:

   ![Parques de patinaje de LA Contenido JCR](assets/pages-templates/page-jcr-structure.png)

   Debe poder asignar de forma temporal cada uno de los nodos a los componentes creados. Vea si puede identificar los diferentes contenedores de diseño utilizados inspeccionando los nodos con el prefijo `container`.

1. A continuación, inspeccione el componente de página en `/apps/wknd/components/page`. Vea las propiedades del componente en CRXDE Lite:

   ![Propiedades del componente de página](assets/pages-templates/page-component-properties.png)

   Solo hay dos scripts HTL, `customfooterlibs.html` y `customheaderlibs.html`, debajo del componente de página. *¿Cómo procesa este componente la página?*

   La propiedad `sling:resourceSuperType` señala a `core/wcm/components/page/v2/page`. Esta propiedad permite que el componente de página de WKND herede **toda** la funcionalidad del componente de página del componente principal. Este es el primer ejemplo de algo llamado [Patrón de componente proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=es#ProxyComponentPattern). Puede encontrar más información [aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=es).

1. Inspeccione otro componente dentro de los componentes WKND, el componente `Breadcrumb` desde: `/apps/wknd/components/breadcrumb`. Observe que se puede encontrar la misma propiedad `sling:resourceSuperType`, pero esta vez apunta a `core/wcm/components/breadcrumb/v2/breadcrumb`. Este es otro ejemplo de uso del patrón de componentes proxy para incluir un componente principal. De hecho, todos los componentes de la base de código WKND son proxies de los componentes principales de AEM (excepto el componente HelloWorld de demostración personalizado). Se recomienda reutilizar tanta funcionalidad de los componentes principales como sea posible *antes de* escribir código personalizado.

1. A continuación, inspeccione la página de componentes principales en `/libs/core/wcm/components/page/v2/page` mediante CRXDE Lite:

   >[!NOTE]
   >
   > En AEM 6.5/6.4 los componentes principales se encuentran en `/apps/core/wcm/components`. En AEM as a Cloud Service, los componentes principales se encuentran en `/libs` y se actualizan automáticamente.

   ![Página de componente principal](assets/pages-templates/core-page-component-properties.png)

   Observe que debajo de esta página se incluyen muchos archivos de script. La página Componente principal contiene numerosas funcionalidades. Esta funcionalidad se divide en varios scripts para facilitar el mantenimiento y la legibilidad. Puede rastrear la inclusión de los scripts HTL abriendo `page.html` y buscando `data-sly-include`:

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

   La otra razón para dividir el HTL en varios scripts es permitir que los componentes proxy anulen los scripts individuales para implementar la lógica empresarial personalizada. Los scripts HTL `customfooterlibs.html` y `customheaderlibs.html` se han creado con el propósito explícito de que se anulen cuando se implementen los proyectos.

   Puede obtener más información sobre cómo la plantilla editable influye en la representación de la [página de contenido en este artículo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=es).

1. Inspeccione otro componente principal, como la ruta de exploración de `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Vea el script `breadcrumb.html` para comprender cómo se genera finalmente el marcado para el componente de ruta de exploración.

## Guardado de las configuraciones en el control de código fuente {#configuration-persistence}

A menudo, especialmente al principio de un proyecto de AEM, es importante mantener las configuraciones, como las plantillas y las directivas de contenido relacionadas, en el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.


Por ahora, las plantillas se tratan como otros fragmentos de código y sincronizan la **plantilla de página del artículo** como parte del proyecto.
Hasta ahora, el código se insertaba desde el proyecto de AEM en una instancia local de AEM. La **plantilla de página del artículo** se creó directamente en una instancia local de AEM, por lo que es necesario **importar** la plantilla en el proyecto de AEM. El módulo **ui.content** está incluido en el proyecto de AEM con este propósito específico.

Los siguientes pasos se realizan en el IDE de VSCode mediante el complemento [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&ssr=false#overview). Pero podrían estar utilizando cualquier IDE que haya configurado para **importar** o importar contenido desde una instancia local de AEM.

1. En, el VSCode abra el proyecto `aem-guides-wknd`.

1. Expanda el módulo **ui.content** en el explorador de proyectos. Expanda la carpeta `src` y vaya hasta `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Haga clic con el botón derecho del ratón] en la carpeta `templates` y seleccione **Importar desde el servidor de AEM**:

   ![Plantilla de importación de VSCode](assets/pages-templates/vscode-import-templates.png)

   `article-page` debe importarse, y las plantillas `page-content`, `xf-web-variation` también deben actualizarse.

   ![Plantillas actualizadas](assets/pages-templates/updated-templates.png)

1. Repita los pasos para importar contenido, pero seleccione la carpeta **policies** de `/conf/wknd/settings/wcm/policies`.

   ![Políticas de importación de VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspeccione el archivo `filter.xml` de `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   El archivo `filter.xml` es responsable de identificar las rutas de acceso de los nodos instalados con el paquete. Observe el `mode="merge"` en cada uno de los filtros lo que indica que el contenido existente no debe modificarse, solo se añade contenido nuevo. Dado que los autores de contenido pueden estar actualizando estas rutas, es importante que una implementación de código **no** sobrescriba el contenido. Consulte la [documentación de FileVault](https://jackrabbit.apache.org/filevault/filter.html?lang=es) para obtener más información sobre cómo trabajar con elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` y `ui.apps/src/main/content/META-INF/vault/filter.xml` para comprender los diferentes nodos administrados por cada módulo.

   >[!WARNING]
   >
   > Para garantizar implementaciones coherentes para el sitio de referencia de WKND, algunas ramas del proyecto se han configurado de modo que `ui.content` sobrescriba cualquier cambio en el JCR. Esto es intencionado, es decir, para ramas de soluciones, ya que el código o los estilos se escriben para directivas específicas.

## Enhorabuena. {#congratulations}

Enhorabuena. Ha creado una plantilla y una página con Adobe Experience Manager Sites.

### Próximos pasos {#next-steps}

En este punto, es evidente que la página del artículo no tiene estilo. Siga el tutorial de [Bibliotecas del lado del cliente y flujo de trabajo front-end](client-side-libraries.md) para conocer las prácticas recomendadas para incluir CSS y JavaScript a fin de aplicar estilos globales al sitio e integrar una compilación de front-end dedicada.

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/pages-templates-solution`.

1. Clone el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consulte la rama `tutorial/pages-templates-solution`.

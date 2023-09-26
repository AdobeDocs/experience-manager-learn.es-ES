---
title: 'Introducción a AEM Sites: Páginas y plantillas'
description: Obtenga información acerca de la relación entre un componente de página base y las plantillas editables. Descubra cómo se procesan como proxy los componentes principales en el proyecto. Conozca las configuraciones de directiva avanzadas de las plantillas editables para crear una plantilla de página de artículo bien estructurada basada en una maqueta de Adobe XD.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '3040'
ht-degree: 1%

---

# Páginas y plantillas {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

En este capítulo, vamos a explorar la relación entre un componente de página base y las plantillas editables. Aprenda a crear una plantilla de artículo sin estilo basada en algunas maquetas de [Adobe XD](https://helpx.adobe.com/support/xd.html). En el proceso de creación de la plantilla, se tratan los componentes principales y las configuraciones de política avanzadas de las plantillas editables.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment).

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la `tutorial/pages-templates-start` bifurcar desde [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) o compruebe el código localmente cambiando a la rama `tutorial/pages-templates-solution`.

## Objetivo

1. Inspect es un diseño de página creado en Adobe XD y lo asigna a los componentes principales.
1. Conozca los detalles de las plantillas editables y cómo se pueden utilizar las políticas para aplicar un control granular del contenido de la página.
1. Descubra cómo se vinculan las plantillas y las páginas

## Lo que va a generar {#what-build}

En esta parte del tutorial, creará una nueva plantilla de página de artículo que se puede utilizar para crear páginas de artículo y se alineará con una estructura común. La plantilla de página de artículo se basa en diseños y un kit de interfaz de usuario producido en Adobe XD. Este capítulo solo se centra en crear la estructura o el esqueleto de la plantilla. No se han implementado estilos, pero la plantilla y las páginas funcionan.

![Diseño de página de artículo y versión sin estilo](assets/pages-templates/what-you-will-build.png)

## Planificación de IU con Adobe XD {#adobexd}

Por lo general, la planificación de un nuevo sitio web comienza con maquetas y diseños estáticos. [Adobe XD](https://helpx.adobe.com/support/xd.html) es una herramienta de diseño que genera experiencia de usuario. A continuación, vamos a inspeccionar un kit de interfaz de usuario y maquetas para ayudar a planificar la estructura de la plantilla de página de artículo.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Descargue la [Archivo de diseño de artículo WKND](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Un genérico [AEM También está disponible el kit de interfaz de usuario para componentes principales](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) como punto de partida para proyectos personalizados.

## Crear la plantilla de página de artículo

Al crear una página, debe seleccionar una plantilla, que se utiliza como base para crear la página. La plantilla define la estructura de la página resultante, el contenido inicial y los componentes permitidos.

Hay tres áreas principales de [Plantillas editables](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=es):

1. **Estructura** : define los componentes que forman parte de la plantilla. Los autores de contenido no pueden editarlos.
1. **Contenido inicial** : define los componentes con los que comienza la plantilla, que los autores de contenido pueden editar o eliminar
1. **Políticas** : define configuraciones sobre cómo se comportan los componentes y qué opciones tienen los autores.

AEM A continuación, cree una plantilla en que se ajuste a la estructura de las pruebas. AEM Esto ocurre en una instancia local de. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Pasos de alto nivel para el vídeo anterior:

### Configuraciones de estructura

1. Cree una plantilla con el **Tipo de plantilla de página**, con nombre **Página de artículo**.
1. Cambiar a **Estructura** modo.
1. Añadir un **Fragmento de experiencia** componente para que actúe como **Header** en la parte superior de la plantilla.
   * Configurar el componente para que apunte a `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Establezca la directiva en **Encabezado de página** y asegurarse de que las **Elemento predeterminado** se establece en `header`. El `header`se identifica con CSS en el capítulo siguiente.
1. Añadir un **Fragmento de experiencia** componente para que actúe como **Pie** en la parte inferior de la plantilla.
   * Configurar el componente para que apunte a `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Establezca la directiva en **Pie de página** y asegurarse de que las **Elemento predeterminado** se establece en `footer`. El `footer` se identifica con CSS en el capítulo siguiente.
1. Bloquee el **main** contenedor que se incluyó cuando se creó inicialmente la plantilla.
   * Establezca la directiva en **Página principal** y asegurarse de que las **Elemento predeterminado** se establece en `main`. El `main` se identifica con CSS en el capítulo siguiente.
1. Añadir un **Imagen** al componente de **main** contenedor.
   * Desbloquee la **Imagen** componente.
1. Añadir un **Ruta** debajo del componente **Imagen** en el contenedor principal.
   * Cree una política para **Ruta** componente denominado **Página de artículo - Ruta**. Configure las variables **Nivel de inicio de navegación** hasta **4**.
1. Añadir un **Contenedor** debajo del componente **Ruta** y dentro del **main** contenedor. Esto actúa como el **Contenedor de contenido** para la plantilla.
   * Desbloquee la **Contenido** contenedor.
   * Establezca la directiva en **Contenido de página**.
1. Añadir otro **Contenedor** debajo del componente **Contenedor de contenido**. Esto actúa como el **Carril lateral** contenedor para la plantilla.
   * Desbloquee la **Carril lateral** contenedor.
   * Cree una directiva con el nombre **Página de artículos - Carril lateral**.
   * Configure las variables **Componentes permitidos** bajo **Proyecto de sitios WKND: contenido** para incluir: **Botón**, **Descargar**, **Imagen**, **Lista**, **Separador**, **Compartir en redes sociales**, **Texto**, y **Título**.
1. Actualice la directiva del contenedor raíz de página. Este es el contenedor exterior de la plantilla. Establezca la directiva en **Raíz de página**.
   * En **Configuración de contenedor**, configure el **Diseño** hasta **Cuadrícula interactiva**.
1. Activar modo de diseño para **Contenedor de contenido**. Arrastre el controlador de derecha a izquierda y reduzca el contenedor a ocho columnas de ancho.
1. Activar modo de diseño para **Contenedor del raíl lateral**. Arrastre el controlador de derecha a izquierda y reduzca el tamaño del contenedor a cuatro columnas de ancho. A continuación, arrastre el controlador izquierdo de izquierda a derecha una columna para que el contenedor tenga 3 columnas de ancho y deje un espacio de 1 columna entre las columnas **Contenedor de contenido**.
1. Abra el emulador móvil y cambie a un punto de interrupción móvil. Vuelva a activar el modo de diseño y haga que las **Contenedor de contenido** y el **Contenedor del raíl lateral** el ancho completo de la página. Esto apila los contenedores verticalmente en el punto de interrupción móvil.
1. Actualizar la directiva de **Texto** componente en la **Contenedor de contenido**.
   * Establezca la directiva en **Texto de contenido**.
   * En **Complementos** > **Estilos de párrafo**, marque **Activar estilos de párrafo** y asegurarse de que las **Bloque de oferta** está activada.

### Configuraciones del contenido inicial

1. Cambiar a **Contenido inicial** modo.
1. Añadir un **Título** al componente de **Contenedor de contenido**. Actúa como título del artículo. Cuando se deja vacío, muestra automáticamente el Título de la página actual.
1. Añada un segundo **Título** debajo del primer componente Título.
   * Configure el componente con el texto: &quot;Por autor&quot;. Es un marcador de posición de texto.
   * Establezca el tipo en `H4`.
1. Añadir un **Texto** debajo del componente **Por autor** Componente Título.
1. Añadir un **Título** al componente de **Contenedor del raíl lateral**.
   * Configure el componente con el texto: &quot;Compartir esta historia&quot;.
   * Establezca el tipo en `H5`.
1. Añadir un **Compartir en redes sociales** debajo del componente **Compartir esta historia** Componente Título.
1. Añadir un **Separador** debajo del componente **Compartir en redes sociales** componente.
1. Añadir un **Descargar** debajo del componente **Separador** componente.
1. Añadir un **Lista** debajo del componente **Descargar** componente.
1. Actualice el **Propiedades de la página inicial** para la plantilla.
   * En **Medios sociales** > **Compartir en redes sociales**, marque **Facebook** y **Pinterest**

### Habilitar la plantilla y agregar una miniatura

1. Vea la plantilla en la consola Plantilla navegando hasta [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Activar** la plantilla Página de artículo.
1. Edite las propiedades de la plantilla Página de artículo y cargue la siguiente miniatura para identificar rápidamente las páginas creadas con la plantilla Página de artículo:

   ![Miniatura de plantilla de página de artículo](assets/pages-templates/article-page-template-thumbnail.png)

## Actualizar el encabezado y el pie de página con fragmentos de experiencias {#experience-fragments}

Una práctica habitual al crear contenido global, como un encabezado o pie de página, es utilizar un [Fragmento de experiencia](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Los fragmentos de experiencias permiten a los usuarios combinar varios componentes para crear un único componente referenciable. Los fragmentos de experiencias tienen la ventaja de admitir la administración de varios sitios y [localización](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en).

AEM El tipo de archivo del proyecto de generó un encabezado y un pie de página. A continuación, actualice los fragmentos de experiencias para que coincidan con las maquetas. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Pasos de alto nivel para el vídeo anterior:

1. Descargar el paquete de contenido de muestra **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Cargue e instale el paquete de contenido mediante el Administrador de paquetes en [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Actualice la plantilla Variación web, que es la plantilla utilizada para Fragmentos de experiencias en [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Actualice la directiva en **Contenedor** en la plantilla.
   * Establezca la directiva en **Raíz XF**.
   * En, la variable **Componentes permitidos** seleccionar el grupo de componentes **Proyecto de sitios WKND: estructura** para incluir **Navegación por idiomas**, **Navegación**, y **Búsqueda rápida** componentes.

### Actualizar fragmento de experiencia de encabezado

1. Abra el fragmento de experiencia que representa el encabezado en [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. Configuración de la raíz **Contenedor** del fragmento. Esta es la más externa **Contenedor**.
   * Configure las variables **Diseño** hasta **Cuadrícula interactiva**
1. Añada el **Logotipo oscuro de WKND** como una imagen en la parte superior de la **Contenedor**. El logotipo se incluyó en el paquete instalado en un paso anterior.
   * Modificación del diseño de la **Logotipo oscuro de WKND** para ser **dos** ancho de columnas. Arrastre los controladores de derecha a izquierda.
   * Configuración del logotipo con **Texto alternativo** de &quot;WKND Logo&quot;.
   * Configure el logotipo para que **Vínculo** hasta `/content/wknd/us/en` la Página de inicio.
1. Configure las variables **Navegación** componente que ya se ha colocado en la página.
   * Configure las variables **Excluir niveles de raíz** hasta **1**.
   * Configure las variables **Profundidad de estructura de navegación** hasta **1**.
   * Modificación del diseño de la **Navegación** componente a ser **ocho** ancho de columnas. Arrastre los controladores de derecha a izquierda.
1. Retire el **Navegación por idiomas** componente.
1. Modificación del diseño de la **Buscar** componente a ser **dos** ancho de columnas. Arrastre los controladores de derecha a izquierda. Ahora todos los componentes deben alinearse horizontalmente en una sola fila.

### Actualizar fragmento de experiencia de pie

1. Abra el fragmento de experiencia que procesa el pie de página en [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configuración de la raíz **Contenedor** del fragmento. Esta es la más externa **Contenedor**.
   * Configure las variables **Diseño** hasta **Cuadrícula interactiva**
1. Añada el **Logotipo ligero de WKND** como una imagen en la parte superior de la **Contenedor**. El logotipo se incluyó en el paquete instalado en un paso anterior.
   * Modificación del diseño de la **Logotipo ligero de WKND** para ser **dos** ancho de columnas. Arrastre los controladores de derecha a izquierda.
   * Configuración del logotipo con **Texto alternativo** de &quot;WKND Logo Light&quot;.
   * Configure el logotipo para que **Vínculo** hasta `/content/wknd/us/en` la Página de inicio.
1. Añadir un **Navegación** debajo del logotipo. Configure las variables **Navegación** componente:
   * Configure las variables **Excluir niveles de raíz** hasta **1**.
   * Desmarcar **Recopilar todas las páginas secundarias**.
   * Configure las variables **Profundidad de estructura de navegación** hasta **1**.
   * Modificación del diseño de la **Navegación** componente a ser **ocho** ancho de columnas. Arrastre los controladores de derecha a izquierda.

## Crear una página de artículo

A continuación, cree una página con la plantilla Página de artículo. Cree el contenido de la página para que coincida con las maquetas del sitio. Siga los pasos del siguiente vídeo:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Pasos de alto nivel para el vídeo anterior:

1. Vaya a la consola Sitios en [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Cree una página debajo de **WKND** > **US** > **EN** > **Revista**.
   * Elija la **Página de artículo** plantilla.
   * En **Propiedades** configure el **Título** &quot;Guía definitiva de LA Skateparks&quot;
   * Configure las variables **Nombre** a &quot;guide-la-skateparks&quot;
1. Reemplazar **Por autor** Título con el texto &quot;Por Stacey Roswells&quot;.
1. Actualice el **Texto** para incluir un párrafo para rellenar el artículo. Puede utilizar el siguiente archivo de texto como copia: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Añadir otro **Texto** componente.
   * Actualice el componente para incluir la cita: &quot;No hay mejor lugar para compartir que Los Ángeles&quot;.
   * Edite el Editor de texto enriquecido en modo de pantalla completa y modifique la cita anterior para utilizar el **Bloque de oferta** Elemento.
1. Siga rellenando el cuerpo del artículo para que coincida con las maquetas.
1. Configure las variables **Descargar** para utilizar una versión PDF del artículo.
   * En **Descargar** > **Propiedades**, haga clic en la casilla para **Obtener el título del recurso DAM**.
   * Configure las variables **Descripción** a: &quot;Obtener la historia completa&quot;.
   * Configure las variables **Texto de acción** hasta: &quot;Descargar PDF&quot;.
1. Configure las variables **Lista** componente.
   * En **Configuración de lista** > **Lista de creación que utiliza**, seleccione **Páginas secundarias**.
   * Configure las variables **Página principal** hasta `/content/wknd/us/en/magazine`.
   * En, la variable **Configuración del elemento** check **Vincular elementos** y compruebe **Mostrar fecha**.

## Inspect la estructura de nodos {#node-structure}

En este punto, es evidente que la página del artículo no tiene estilo. Sin embargo, la estructura básica está en su lugar. A continuación, inspeccione la estructura de nodos de la página de artículo para comprender mejor la función de la plantilla, la página y los componentes.

AEM Utilice la herramienta CRXDE-Lite en una instancia de local para ver la estructura del nodo subyacente.

1. Abrir [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) y utilice la navegación de árbol para navegar hasta `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Haga clic en `jcr:content` debajo del nodo `la-skateparks` y vea las propiedades:

   ![Propiedades de contenido JCR](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Observe el valor de `cq:template`, que apunta a `/conf/wknd/settings/wcm/templates/article-page`, la plantilla de página de artículo creada anteriormente.

   Observe también el valor de `sling:resourceType`, que apunta a `wknd/components/page`. AEM Este es el componente de página creado por el tipo de archivo del proyecto de y es responsable de procesar la página en función de la plantilla.

1. Expanda el `jcr:content` nodo debajo `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` y vea la jerarquía de nodos:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   Debe poder asignar de forma temporal cada uno de los nodos a los componentes creados. Compruebe si puede identificar los diferentes contenedores de diseño utilizados inspeccionando los nodos con el prefijo `container`.

1. A continuación, inspeccione el componente de página en `/apps/wknd/components/page`. Vea las propiedades del componente en CRXDE Lite:

   ![Propiedades del componente Página](assets/pages-templates/page-component-properties.png)

   Solo hay dos scripts HTL, `customfooterlibs.html` y `customheaderlibs.html` debajo del componente Página. *Entonces, ¿cómo procesa este componente la página?*

   El `sling:resourceSuperType` La propiedad apunta a `core/wcm/components/page/v2/page`. Esta propiedad permite que el componente de página de WKND herede **todo** la funcionalidad del componente principal página componente. Este es el primer ejemplo de algo llamado [Patrón de componentes proxy](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Puede encontrar más información [aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect es otro componente dentro de los componentes de WKND, el `Breadcrumb` componente de: `/apps/wknd/components/breadcrumb`. Observe que el mismo `sling:resourceSuperType` La propiedad de se puede encontrar, pero esta vez apunta a `core/wcm/components/breadcrumb/v2/breadcrumb`. Este es otro ejemplo de uso del patrón de componentes proxy para incluir un componente principal. AEM De hecho, todos los componentes de la base de código de WKND son proxies de los componentes principales de la (excepto el componente HelloWorld de demostración personalizado). Se recomienda reutilizar la mayor parte posible de la funcionalidad de los componentes principales *antes* escribir código personalizado.

1. A continuación, inspeccione la página de componentes principales en `/libs/core/wcm/components/page/v2/page` uso del CRXDE Lite:

   >[!NOTE]
   >
   > AEM En, 6.5/6.4, los componentes principales se encuentran en `/apps/core/wcm/components`. AEM En, as a Cloud Service, los componentes principales se encuentran en `/libs` y se actualizan automáticamente.

   ![Página de componentes principales](assets/pages-templates/core-page-component-properties.png)

   Observe que debajo de esta página se incluyen muchos archivos de script. La página Componente principal contiene numerosas funcionalidades. Esta funcionalidad se divide en varios scripts para facilitar el mantenimiento y la legibilidad. Puede rastrear la inclusión de las secuencias de comandos HTL abriendo `page.html` y buscando `data-sly-include`:

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

   La otra razón para dividir el HTL en varios scripts es permitir que los componentes proxy anulen los scripts individuales para implementar la lógica empresarial personalizada. Los scripts HTL `customfooterlibs.html`, y `customheaderlibs.html`, se crean con el propósito explícito de que se anulen al implementar proyectos.

   Puede obtener más información sobre cómo la plantilla editable influye en la renderización del [página de contenido leyendo este artículo](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=es).

1. Inspect es otro componente principal, como la ruta de exploración en `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Ver el `breadcrumb.html` para comprender cómo se genera finalmente el marcado para el componente Ruta de exploración.

## Guardar configuraciones en el control de código fuente {#configuration-persistence}

AEM A menudo, sobre todo al principio de un proyecto de, es importante mantener configuraciones, como plantillas y directivas de contenido relacionadas, para el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.


Por ahora, las plantillas se tratan como otros fragmentos de código y sincronizan con **Plantilla de página de artículo** como parte del proyecto.
AEM AEM Hasta ahora, el código se insertaba desde el proyecto de a una instancia local de. El **Plantilla de página de artículo** AEM se ha creado directamente en una instancia local de, por lo que debe **importar** AEM Inserte la plantilla en el proyecto de. El **ui.content** AEM Este módulo se incluye en el proyecto de la para este propósito específico.

Los siguientes pasos se realizan en el IDE de VSCode mediante la variable [AEM Sincronización de VSCode con la](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) plugin. Pero podrían estar utilizando cualquier IDE al que se haya configurado **importar** AEM o importar contenido desde una instancia local de.

1. En, el VSCode abre el `aem-guides-wknd` proyecto.

1. Expanda el **ui.content** en el explorador del proyecto. Expanda el `src` y vaya a `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Haga clic con el botón derecho] el `templates` carpeta y seleccione **AEM Importar desde servidor de**:

   ![Plantilla de importación de VSCode](assets/pages-templates/vscode-import-templates.png)

   El `article-page` debe importarse, y la variable `page-content`, `xf-web-variation` las plantillas también deben actualizarse.

   ![Plantillas actualizadas](assets/pages-templates/updated-templates.png)

1. Repita los pasos para importar contenido, pero seleccione **directivas** carpeta de `/conf/wknd/settings/wcm/policies`.

   ![Políticas de importación de VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect el `filter.xml` archivo de `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   El `filter.xml` es responsable de identificar las rutas de los nodos que se instalan con el paquete. Observe el `mode="merge"` en cada uno de los filtros, lo que indica que el contenido existente no se debe modificar, solo se añade contenido nuevo. Dado que los autores de contenido pueden estar actualizando estas rutas, es importante que la implementación de código haga lo siguiente **no** sobrescribir contenido. Consulte la [Documentación de FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obtener más información sobre cómo trabajar con elementos de filtro.

   Comparar `ui.content/src/main/content/META-INF/vault/filter.xml` y `ui.apps/src/main/content/META-INF/vault/filter.xml` para comprender los diferentes nodos administrados por cada módulo.

   >[!WARNING]
   >
   > Para garantizar implementaciones coherentes en el sitio de referencia de WKND, algunas ramas del proyecto están configuradas de modo que `ui.content` sobrescribe cualquier cambio en el JCR. Esto es por diseño, es decir, para ramas de soluciones, ya que el código o los estilos se escriben para directivas específicas.

## Enhorabuena. {#congratulations}

Ha creado una plantilla y una página con Adobe Experience Manager Sites.

### Pasos siguientes {#next-steps}

En este punto, es evidente que la página del artículo no tiene estilo. Siga las [Bibliotecas del lado cliente y flujo de trabajo front-end](client-side-libraries.md) tutorial para conocer las prácticas recomendadas para incluir CSS y JavaScript a fin de aplicar estilos globales al sitio e integrar una compilación de front-end dedicada.

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/pages-templates-solution`.

1. Clonar el [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repositorio.
1. Consulte la `tutorial/pages-templates-solution` Rama.

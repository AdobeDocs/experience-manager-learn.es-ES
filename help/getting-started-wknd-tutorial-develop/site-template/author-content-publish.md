---
title: Introducción a la creación y publicación | Creación rápida de sitios de AEM
description: Utilice el Editor de páginas en Adobe Experience Manager, AEM, para actualizar el contenido del sitio web. Descubra cómo se utilizan los componentes para facilitar la creación. Comprenda la diferencia entre los entornos de AEM Author y Publish, y aprenda a publicar cambios en el sitio en directo.
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 1%

---

# Introducción a la creación y publicación {#author-content-publish}

Es importante comprender cómo actualizará un usuario el contenido del sitio web. En este capítulo adoptaremos la personalidad de un **Autor de contenido** y haremos algunas actualizaciones editoriales en el sitio generadas en el capítulo anterior. Al final del capítulo, publicaremos los cambios para comprender cómo se actualiza el sitio en directo.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en el capítulo [Crear un sitio](./create-site.md) se han completado.

## Objetivo {#objective}

1. Comprenda los conceptos de **Páginas** y **Componentes** en AEM Sites.
1. Obtenga información sobre cómo actualizar el contenido del sitio web.
1. Obtenga información sobre cómo publicar cambios en el sitio activo.

## Crear una nueva página {#create-page}

Un sitio web generalmente se divide en páginas para formar una experiencia de varias páginas. AEM estructura el contenido del mismo modo. A continuación, cree una nueva página para el sitio.

1. Inicie sesión en el servicio de AEM **Author** utilizado en el capítulo anterior.
1. En la pantalla de inicio de AEM, haga clic en **Sitios** > **Sitio WKND** > **Inglés** > **Artículo**
1. En la esquina superior derecha, haga clic en **Crear** > **Página**.

   ![Crear página](assets/author-content-publish/create-page-button.png)

   Esto abrirá el asistente para **Crear página**.

1. Elija la plantilla **Página de artículo** y haga clic en **Siguiente**.

   Las páginas de AEM se crean a partir de una plantilla de página. Las plantillas de página se exploran en detalle en el capítulo [Plantillas de página](page-templates.md).

1. En **Propiedades**, escriba un **Título** de &quot;Hello World&quot;.
1. Establezca **Name** como `hello-world` y haga clic en **Crear**.

   ![Propiedades de la página inicial](assets/author-content-publish/initial-page-properties.png)

1. En el cuadro de diálogo emergente, haga clic en **Abrir** para abrir la página recién creada.

## Crear un componente {#author-component}

Los componentes de AEM se pueden considerar como pequeños componentes modulares de una página web. Al dividir la interfaz de usuario en fragmentos lógicos o componentes, resulta mucho más fácil administrarla. Para reutilizar componentes, estos deben poder configurarse. Esto se realiza a través del cuadro de diálogo de autor.

AEM proporciona un conjunto de [componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es) que están listos para su uso en producción. Los **componentes principales** van desde elementos básicos como [Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=es) e [Imagen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=es) hasta elementos de IU más complejos como un [Carrusel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=es).

A continuación, cree algunos componentes con el Editor de páginas de AEM.

1. Vaya a la página **Hello World** creada en el ejercicio anterior.
1. Asegúrese de que está en modo **Editar** y, en el carril lateral izquierdo, haga clic en el icono **Componentes**.

   ![Carril lateral del editor de páginas](assets/author-content-publish/page-editor-siderail.png)

   Se abrirá la biblioteca de componentes y se enumerarán los componentes disponibles que se pueden utilizar en la página.

1. Desplácese hacia abajo y **arrastre y suelte** un componente **Texto (v2)** en la región editable principal de la página.

   ![Arrastrar y soltar componente de texto](assets/author-content-publish/drag-drop-text-cmp.png)

1. Haga clic en el componente **Texto** que desee resaltar y, a continuación, haga clic en el icono **llave inglesa** ![Icono de llave inglesa](assets/author-content-publish/wrench-icon.png) para abrir el cuadro de diálogo del componente. Introduzca texto y guarde los cambios en el cuadro de diálogo.

   ![Componente Texto enriquecido](assets/author-content-publish/rich-text-populated-component.png)

   El componente **Texto** ahora debería mostrar el texto enriquecido en la página.

1. Repita los pasos anteriores, excepto que arrastre una instancia del componente **Image(v2)** a la página. Abra el cuadro de diálogo del componente **Image**.

1. En el carril izquierdo, cambie a **Buscador de recursos** haciendo clic en el icono de **Assets** ![icono de recurso](assets/author-content-publish/asset-icon.png).
1. **Arrastre y suelte** una imagen en el cuadro de diálogo del componente y haga clic en **Listo** para guardar los cambios.

   ![Agregar recurso al cuadro de diálogo](assets/author-content-publish/add-asset-dialog.png)

1. Observe que hay componentes en la página, como **Title**, **Navigation**, **Search**, que son fijos. Estas áreas se configuran como parte de la plantilla de página y no se pueden modificar en una página individual. Esto se analiza más en el capítulo siguiente.

Siéntase libre de experimentar con algunos de los otros componentes. La documentación sobre cada [componente principal se encuentra aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es). Encontrará una serie de vídeos detallada sobre la creación de [páginas aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html?lang=es).

## Publicar actualizaciones {#publish-updates}

Los entornos de AEM se dividen entre **Servicio de creación** y **Servicio de publicación**. En este capítulo hemos realizado varias modificaciones en el sitio en el **servicio de creación**. Para que los visitantes del sitio puedan ver los cambios, debemos publicarlos en **Servicio de publicación**.

![Diagrama de alto nivel](assets/author-content-publish/author-publish-high-level-flow.png)

*Flujo de contenido de alto nivel de autor a publicación*

**1.** autores de contenido realizan actualizaciones del contenido del sitio. Las actualizaciones se pueden previsualizar, revisar y aprobar para su publicación.

**2.El contenido de** se ha publicado. La publicación se puede realizar bajo demanda o programarse para una fecha futura.

**3.** Los visitantes del sitio verán los cambios reflejados en el servicio de publicación.

### Publicación de los cambios

A continuación, vamos a publicar los cambios.

1. En la pantalla de inicio de AEM, vaya a **Sitios** y seleccione el **Sitio WKND**.
1. Haga clic en **Administrar publicación** en la barra de menús.

   ![Administrar publicación](assets/author-content-publish/click-manage-publiciation.png)

   Como se trata de un sitio completamente nuevo, queremos publicar todas las páginas y podemos utilizar el asistente Administrar publicación para definir exactamente lo que debe publicarse.

1. En **Opciones**, deje la configuración predeterminada en **Publicar** y programe para **Ahora**. Haga clic en **Siguiente**.
1. En **Ámbito**, seleccione el **sitio WKND** y haga clic en **Incluir configuración secundaria**. En el cuadro de diálogo, marque **Incluir elementos secundarios**. Desmarque el resto de las casillas para asegurarse de que se publique todo el sitio.

   ![Actualizar ámbito de publicación](assets/author-content-publish/update-scope-publish.png)

1. Haga clic en el botón **Referencias publicadas**. En el cuadro de diálogo, compruebe que todo está marcado. Esto incluirá la **Plantilla de sitio estándar** y varias configuraciones generadas por la Plantilla de sitio. Haga clic en **Listo** para actualizar.

   ![Referencias de publicación](assets/author-content-publish/publish-references.png)

1. Finalmente, marque la casilla junto al **Sitio WKND** y haga clic en **Siguiente** en la esquina superior derecha.
1. En el paso **Flujos de trabajo**, escriba un **título de flujo de trabajo**. Puede ser cualquier texto y puede resultar útil como parte de una pista de auditoría más adelante. Escriba &quot;Publicación inicial&quot; y haga clic en **Publicar**.

![Publicación inicial del paso de flujo de trabajo](assets/author-content-publish/workflow-step-publish.png)

## Ver contenido publicado {#publish}

A continuación, vaya al servicio Publicación para ver los cambios.

1. Una manera sencilla de obtener la dirección URL del servicio de publicación es copiar la dirección URL del autor y reemplazar la palabra `author` por `publish`. Por ejemplo:

   * **URL del autor** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL de publicación** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Agregar `/content/wknd.html` a la dirección URL de publicación para que la dirección URL final tenga el siguiente aspecto: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Cambie `wknd.html` para que coincida con el nombre de su sitio, si proporcionó un nombre único durante [la creación del sitio](create-site.md).

1. Si navega a la URL de publicación, debería ver el sitio sin ninguna de las funcionalidades de creación de AEM.

   ![Sitio publicado](assets/author-content-publish/publish-url-update.png)

1. Usando el menú **Navegación**, haga clic en **Artículo** > **Hello World** para navegar a la página Hello World creada anteriormente.
1. Vuelva al **Servicio de autor de AEM** y realice algunos cambios de contenido adicionales en el editor de páginas.
1. Publique estos cambios directamente desde el editor de páginas haciendo clic en el icono **Propiedades de página** > **Publicar página**

   ![publicar directamente](assets/author-content-publish/page-editor-publish.png)

1. Vuelva al **Servicio de publicación de AEM** para ver los cambios. Lo más probable es que **no** vea inmediatamente las actualizaciones. Esto se debe a que el **servicio de publicación de AEM** incluye el almacenamiento en caché de [mediante un servidor web Apache y CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html?lang=es). De forma predeterminada, el contenido de HTML se almacena en caché durante ~5 minutos.

1. Para omitir la caché con fines de prueba o depuración, simplemente agregue un parámetro de consulta como `?nocache=true`. La dirección URL sería `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Encontrará más detalles sobre la estrategia de almacenamiento en caché y las configuraciones disponibles [aquí](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html?lang=es).

1. También puede encontrar la dirección URL del servicio de publicación en Cloud Manager. Vaya a **Programa de Cloud Manager** > **Entornos** > **Entorno**.

   ![Ver servicio de publicación](assets/author-content-publish/view-environment-segments.png)

   En **Segmentos de entorno** encontrará vínculos a los servicios **Autor** y **Publicar**.

## Enhorabuena. {#congratulations}

¡Felicidades, acaba de crear y publicar cambios en su sitio de AEM!

### Siguientes pasos {#next-steps}

En una implementación real, planificar un sitio con maquetas y diseños de interfaz de usuario suele preceder a la creación del sitio. Aprenda cómo se pueden usar los kits de IU de Adobe XD para diseñar y acelerar la implementación de Adobe Experience Manager Sites en la [planificación de la IU con Adobe XD](./ui-planning-adobe-xd.md).

¿Quiere seguir explorando las funcionalidades de AEM Sites? No dude en ir directamente al capítulo de [Plantillas de página](./page-templates.md) para entender la relación entre una Plantilla de página y una Página.



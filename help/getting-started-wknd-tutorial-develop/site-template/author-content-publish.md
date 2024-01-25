---
title: Introducción a la creación y publicación | AEM Creación rápida de sitios de
description: Utilice el editor de páginas de Adobe Experience Manager AEM, en el que se puede actualizar el contenido del sitio web, en el menú de la página. Descubra cómo se utilizan los componentes para facilitar la creación. AEM Comprenda la diferencia entre los entornos de creación y publicación de un, y aprenda a publicar cambios en el sitio en directo.
version: Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 355
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 1%

---

# Introducción a la creación y publicación {#author-content-publish}

Es importante comprender cómo actualizará un usuario el contenido del sitio web. En este capítulo adoptaremos la personalidad de un **Autor de contenido** y realizar algunas actualizaciones editoriales al sitio generadas en el capítulo anterior. Al final del capítulo, publicaremos los cambios para comprender cómo se actualiza el sitio en directo.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que los pasos descritos en la sección [Crear un sitio](./create-site.md) se han completado los capítulos.

## Objetivo {#objective}

1. Comprender los conceptos de **Páginas** y **Componentes** en AEM Sites.
1. Obtenga información sobre cómo actualizar el contenido del sitio web.
1. Obtenga información sobre cómo publicar cambios en el sitio activo.

## Crear una nueva página {#create-page}

Un sitio web generalmente se divide en páginas para formar una experiencia de varias páginas. AEM estructura el contenido de la misma manera. A continuación, cree una nueva página para el sitio.

1. AEM Inicie sesión en la **Autor** Servicio utilizado en el capítulo anterior.
1. AEM En la pantalla Inicio de la, haga clic en **Sites** > **Sitio WKND** > **Inglés** > **Artículo**
1. En la esquina superior derecha, haga clic en **Crear** > **Página**.

   ![Crear página](assets/author-content-publish/create-page-button.png)

   Esto mostrará el **Crear página** asistente.

1. Elija la **Página de artículo** y haga clic en **Siguiente**.

   AEM Las páginas de la plantilla de página se crean a partir de una plantilla de página. Las plantillas de página se exploran en detalle en la [Plantillas de página](page-templates.md) capítulo.

1. En **Propiedades** introduzca un **Título** de &quot;Hello World&quot;.
1. Configure las variables **Nombre** para ser `hello-world` y haga clic en **Crear**.

   ![Propiedades de la página inicial](assets/author-content-publish/initial-page-properties.png)

1. En el cuadro de diálogo emergente, haga clic en **Abrir** para abrir la página recién creada.

## Crear un componente {#author-component}

AEM Los componentes se pueden considerar como pequeños componentes modulares de una página web. Al dividir la interfaz de usuario en fragmentos lógicos o componentes, resulta mucho más fácil administrarla. Para reutilizar componentes, estos deben poder configurarse. Esto se realiza a través del cuadro de diálogo de autor.

AEM proporciona un conjunto de [Componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es) que están listos para su uso en producción. El **Componentes principales** abarca desde elementos básicos como [Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) y [Imagen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=es) a elementos de interfaz de usuario más complejos, como [Carrusel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html?lang=es).

AEM A continuación, cree algunos componentes con el Editor de páginas de.

1. Vaya a **Hello World** página creada en el ejercicio anterior.
1. Asegúrese de que está en **Editar** y en el carril lateral izquierdo, haga clic en el botón **Componentes** icono.

   ![Carril lateral del editor de páginas](assets/author-content-publish/page-editor-siderail.png)

   Se abrirá la biblioteca de componentes y se enumerarán los componentes disponibles que se pueden utilizar en la página.

1. Desplazarse hacia abajo y **Arrastrar y soltar** a **Texto (v2)** componente en la región editable principal de la página.

   ![Arrastrar y soltar componente de texto](assets/author-content-publish/drag-drop-text-cmp.png)

1. Haga clic en **Texto** componente para resaltar y, a continuación, haga clic en **llave** icono ![Icono de llave inglesa](assets/author-content-publish/wrench-icon.png) para abrir el cuadro de diálogo del componente. Introduzca texto y guarde los cambios en el cuadro de diálogo.

   ![Componente Texto enriquecido](assets/author-content-publish/rich-text-populated-component.png)

   El **Texto** ahora debe mostrar el texto enriquecido en la página.

1. Repita los pasos anteriores, excepto que arrastre una instancia del **Imagen (v2)** componente en la página. Abra el **Imagen** cuadro de diálogo del componente.

1. En el carril izquierdo, cambie a **Buscador de recursos** haciendo clic en **Assets** icono ![icono de recurso](assets/author-content-publish/asset-icon.png).
1. **Arrastrar y soltar** Seleccione una imagen en el cuadro de diálogo del componente y haga clic en **Listo** para guardar los cambios.

   ![Agregar recurso al cuadro de diálogo](assets/author-content-publish/add-asset-dialog.png)

1. Observe que hay componentes en la página, como **Título**, **Navegación**, **Buscar** que son fijos. Estas áreas se configuran como parte de la plantilla de página y no se pueden modificar en una página individual. Esto se analiza más en el capítulo siguiente.

Siéntase libre de experimentar con algunos de los otros componentes. Documentación sobre cada [El componente principal se puede encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es). Una serie de vídeos detallada sobre [Puede encontrar la creación de páginas aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## Publicar actualizaciones {#publish-updates}

AEM Los entornos de se dividen entre una **Servicio de creación** y una **Servicio de publicación**. En este capítulo hemos realizado varias modificaciones en el sitio en la **Servicio de creación**. Para que los visitantes del sitio vean los cambios, debemos publicarlos en el **Servicio de publicación**.

![Diagrama de alto nivel](assets/author-content-publish/author-publish-high-level-flow.png)

*Flujo de contenido de alto nivel de Autor a Publicar*

**1.** Los autores de contenido realizan actualizaciones del contenido del sitio. Las actualizaciones se pueden previsualizar, revisar y aprobar para su publicación.

**2.** El contenido se ha publicado. La publicación se puede realizar bajo demanda o programarse para una fecha futura.

**3.** Los visitantes del sitio verán los cambios reflejados en el servicio de publicación.

### Publicación de los cambios

A continuación, vamos a publicar los cambios.

1. AEM En la pantalla Inicio de la, vaya a **Sites** y seleccione la **Sitio WKND**.
1. Haga clic en **Administrar publicación** en la barra de menús.

   ![Administrar publicación](assets/author-content-publish/click-manage-publiciation.png)

   Como se trata de un sitio completamente nuevo, queremos publicar todas las páginas y podemos utilizar el asistente Administrar publicación para definir exactamente lo que debe publicarse.

1. En **Opciones** deje la configuración predeterminada en **Publish** y programarlo para **Ahora**. Haga clic en **Siguiente**.
1. En **Ámbito**, seleccione la **Sitio WKND** y haga clic en **Incluir configuración secundaria**. En el cuadro de diálogo, marque **Incluir elementos secundarios**. Desmarque el resto de las casillas para asegurarse de que se publique todo el sitio.

   ![Actualizar ámbito de publicación](assets/author-content-publish/update-scope-publish.png)

1. Haga clic en **Referencias publicadas** botón. En el cuadro de diálogo, compruebe que todo está marcado. Esto incluirá lo siguiente **Plantilla de sitio estándar** y varias configuraciones generadas por la plantilla del sitio. Clic **Listo** para actualizar.

   ![Referencias de publicación](assets/author-content-publish/publish-references.png)

1. Finalmente, marque la casilla junto a **Sitio WKND** y haga clic en **Siguiente** en la esquina superior derecha.
1. En el **Flujos de trabajo** paso, introduzca un **Título del flujo de trabajo**. Puede ser cualquier texto y puede resultar útil como parte de una pista de auditoría más adelante. Introduzca &quot;Initial publish&quot; y haga clic en **Publish**.

![Publicación inicial del paso de flujo de trabajo](assets/author-content-publish/workflow-step-publish.png)

## Ver contenido publicado {#publish}

A continuación, vaya al servicio Publicación para ver los cambios.

1. Una manera sencilla de obtener la URL del servicio de publicación es copiar la URL del autor y reemplazar el `author` palabra con `publish`. Por ejemplo:

   * **URL del autor** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **URL de publicación** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. Añadir `/content/wknd.html` a la URL de publicación para que la URL final tenga el siguiente aspecto: `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > Cambiar `wknd.html` para que coincida con el nombre del sitio, si ha proporcionado un nombre único durante la [creación del sitio](create-site.md).

1. AEM Si navega a la URL de publicación, debería ver el sitio sin ninguna de las funcionalidades de creación de.

   ![Sitio publicado](assets/author-content-publish/publish-url-update.png)

1. Uso del **Navegación** clic de menú **Artículo** > **Hello World** para navegar a la página Hello World creada anteriormente.
1. Vuelva a la **AEM Servicio de autor de** y realice algunos cambios de contenido adicionales en el Editor de páginas.
1. Publique estos cambios directamente desde el editor de páginas haciendo clic en **Propiedades de página** icono > **Publicar página**

   ![publicar directamente](assets/author-content-publish/page-editor-publish.png)

1. Vuelva a la **AEM Servicio de publicación de** para ver los cambios. Lo más probable es que lo haga **no** ver inmediatamente las actualizaciones. Esto se debe a que **AEM Servicio de publicación de** incluye [almacenamiento en caché a través de un servidor web Apache y CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). De forma predeterminada, el contenido del HTML se almacena en caché durante ~5 minutos.

1. Para omitir la caché con fines de prueba o depuración, simplemente agregue un parámetro de consulta como `?nocache=true`. La dirección URL tendría el siguiente aspecto `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. Más detalles acerca de la estrategia de almacenamiento en caché y las configuraciones disponibles [se puede encontrar aquí](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. También puede encontrar la URL del servicio de publicación en Cloud Manager. Vaya a **Programa de Cloud Manager** > **Entornos** > **Entorno**.

   ![Ver servicio de publicación](assets/author-content-publish/view-environment-segments.png)

   En **Segmentos de entorno** puede encontrar vínculos a la **Autor** y **Publish** servicios.

## Enhorabuena. {#congratulations}

AEM ¡Felicidades, acaba de crear y publicar cambios en su sitio de la!

### Pasos siguientes {#next-steps}

En una implementación real, planificar un sitio con maquetas y diseños de interfaz de usuario suele preceder a la creación del sitio. Descubra cómo se pueden utilizar los Kits de IU de Adobe XD para diseñar y acelerar su implementación de Adobe Experience Manager Sites en [Planificación de la IU con Adobe XD](./ui-planning-adobe-xd.md).

¿Quiere seguir explorando las funcionalidades de AEM Sites? Siéntase libre de saltar directamente en el capítulo en [Plantillas de página](./page-templates.md) para comprender la relación entre una Plantilla de página y una Página.



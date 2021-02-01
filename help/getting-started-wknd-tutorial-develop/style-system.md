---
title: Desarrollo con el sistema de estilos
seo-title: Desarrollo con el sistema de estilos
description: Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. Este tutorial trata el desarrollo del sistema de estilos para ampliar los componentes principales con CSS específica de la marca y configuraciones de políticas avanzadas del Editor de plantillas.
sub-product: sitios
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1996'
ht-degree: 1%

---


# Desarrollo con el sistema de estilos {#developing-with-the-style-system}

Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. Este tutorial trata el desarrollo del sistema de estilos para ampliar los componentes principales con CSS específica de la marca y configuraciones de políticas avanzadas del Editor de plantillas.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar el tutorial [Client-side Libraries and Front-end Workflow](client-side-libraries.md) para comprender los aspectos fundamentales de las bibliotecas del lado del cliente y las distintas herramientas front-end incorporadas en el proyecto de AEM.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

Consulte el código de línea base que el tutorial genera:

1. Consulte la rama `tutorial/style-system-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) o extraer el código localmente cambiando a la rama `tutorial/style-system-solution`.

## Objetivo

1. Obtenga información sobre cómo utilizar el sistema de estilos para aplicar CSS específica de la marca a AEM componentes principales.
1. Obtenga información sobre la notación BEM y cómo se puede utilizar para aplicar un ámbito a los estilos con cuidado.
1. Aplique configuraciones de directiva avanzadas con plantillas editables.

## Qué va a generar {#what-you-will-build}

En este capítulo utilizaremos la función [Sistema de estilo](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) para crear variaciones de los componentes **Título** y **Texto** utilizados en la página Artículo.

![Estilos disponibles para el título](assets/style-system/styles-added-title.png)

*Estilo de subrayado disponible para usar en el componente Título*

## Fondo {#background}

El [sistema de estilos](https://docs.adobe.com/content/help/es-ES/experience-manager-65/developing/components/style-system.html) permite a los desarrolladores y editores de plantillas crear varias variaciones visuales de un componente. A su vez, los autores pueden decidir qué estilo utilizar al componer una página. Aprovecharemos el sistema de estilos en el resto del tutorial para lograr varios estilos únicos, mientras aprovechamos los componentes principales en un enfoque de código bajo.

La idea general del sistema de estilos es que los autores puedan elegir distintos estilos de aspecto de un componente. Los &quot;estilos&quot; están respaldados por clases CSS adicionales que se insertan en el div exterior de un componente. En las bibliotecas de cliente, las reglas CSS se agregan en función de estas clases de estilo para que el componente cambie su aspecto.

Puede encontrar [documentación detallada para Style System aquí](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html). También hay un bueno [vídeo técnico para comprender el sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Estilo de subrayado - Título {#underline-style}

El [Componente de título](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html) se ha procesado como proxy en el proyecto en `/apps/wknd/components/title` como parte del módulo **ui.apps**. Los estilos predeterminados de los elementos Heading (`H1`, `H2`, `H3`...) ya se han implementado en el módulo **ui.frontendr**.

Los [diseños de artículo de WKND](assets/pages-templates/wknd-article-design.xd) contienen un estilo único para el componente Título con un subrayado. En lugar de crear dos componentes o modificar el cuadro de diálogo del componente, se puede utilizar el sistema de estilos para permitir a los autores añadir un estilo de subrayado.

![Estilo de subrayado - Componente de título](assets/style-system/title-underline-style.png)

### Marcado de título de Inspect

Como desarrollador front-end, el primer paso para diseñar un componente principal es comprender el marcado generado por el componente.

1. Abra un navegador nuevo y vista el componente Título en el sitio de la biblioteca de componentes principales de AEM: [https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html)

1. A continuación se muestra el marcado del componente Título:

   ```html
   <div class="cmp-title">
       <h1 class="cmp-title__text">Lorem Ipsum</h1>
   </div>
   ```

   Notación BEM del componente Título:

   ```plain
   BLOCK cmp-title
       ELEMENT cmp-title__text
   ```

1. El sistema de estilos agrega una clase CSS al div exterior que rodea el componente. Por lo tanto, el marcado al que nos dirigiremos se parecerá a lo siguiente:

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-title">
           <h1 class="cmp-title__text">Lorem Ipsum</h1>
       </div>
   </div>
   ```

### Implementar el estilo de subrayado - ui.frontender

A continuación, implemente el estilo Subrayado utilizando el módulo **ui.frontender** de nuestro proyecto. Utilizaremos el servidor de desarrollo de webpack que se incluye con el módulo **ui.front** para previsualización de los estilos *antes de* implementarlos en una instancia local de AEM.

1. Inicio el servidor de desarrollo de webpack ejecutando el siguiente comando desde el módulo **ui.frontender**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

   Esto debería abrir un explorador en [http://localhost:8080](http://localhost:8080).

   >[!NOTE]
   >
   > Si las imágenes aparecen dañadas, asegúrese de que el proyecto de inicio se haya implementado en una instancia local de AEM (que se ejecuta en el puerto 4502) y de que el explorador utilizado también haya iniciado sesión en la instancia de AEM local.

   ![Servidor de desarrollo de Webpack](assets/style-system/static-webpack-server.png)

1. En el IDE, abra el archivo `index.html` ubicado en: `ui.frontend/src/main/webpack/static/index.html`. Este es el código estático utilizado por el servidor de desarrollo de webpack.
1. En `index.html` busque una instancia del componente Título para agregar el estilo subrayado a la cual buscar en el documento *cmp-title*. Elija el componente Título con el texto *&quot;Vans off the Wall Skatepark&quot;* (línea 218). Añada la clase `cmp-title--underline` al div circundante:

   ```diff
   - <div class="title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-title--underline title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;title-8bea562fa0&#34;:{&#34;@type&#34;:&#34;wknd/components/title&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T18:54:20Z&#34;,&#34;dc:title&#34;:&#34;Vans Off the Wall&#34;}}" id="title-8bea562fa0" class="cmp-title">
            <h2 class="cmp-title__text">Vans Off the Wall</h2>
        </div>
    </div>
   ```

1. Vuelva al explorador y compruebe que la clase adicional se refleja en el marcado.
1. Vuelva al módulo **ui.front** y actualice el archivo `title.scss` ubicado en: `ui.frontend/src/main/webpack/components/_title.scss`:

   ```css
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >Se considera una práctica recomendada definir siempre los estilos de ámbito estrictamente para el componente destinatario. Esto garantiza que los estilos adicionales no afecten a otras áreas de la página.
   >
   >Todos los componentes principales se adhieren a **[notación de BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. Se recomienda el destinatario de la clase CSS exterior al crear un estilo predeterminado para un componente. Otra práctica recomendada es utilizar nombres de clase de destinatario especificados por la notación BEM de componentes principales en lugar de elementos HTML.

1. Vuelva al explorador una vez más y debería ver el estilo Subrayado agregado:

   ![Estilo de subrayado visible en el servidor de desarrollo de webpack](assets/style-system/underline-implemented-webpack.png)

1. Detenga el servidor de desarrollo de webpack.

### Añadir una directiva de título

A continuación, debemos añadir una nueva directiva para los componentes Título para permitir a los autores de contenido elegir el estilo Subrayado que se aplicará a componentes específicos. Esto se realiza con el Editor de plantillas de AEM.

1. Implemente la base de código en una instancia de AEM local utilizando sus conocimientos Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Vaya a la plantilla **Página del artículo** ubicada en: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. En el modo **Estructura**, en el Contenedor principal **Diseño**, seleccione el icono **Política** junto al componente **Título** que se encuentra en *Componentes permitidos*:

   ![Configuración de directiva de título](assets/style-system/article-template-title-policy-icon.png)

1. Cree una nueva directiva para el componente Título con los siguientes valores:

   *Título de directiva **:  **Título WKND**

   *Propiedades* > Ficha  *Estilos* >  *Añadir un nuevo estilo*

   **Subrayado** :  `cmp-title--underline`

   ![Configuración de directiva de estilo para título](assets/style-system/title-style-policy.png)

   Haga clic en **Listo** para guardar los cambios en la directiva Título.

   >[!NOTE]
   >
   > El valor `cmp-title--underline` coincide con la clase CSS a la que nos dirigimos anteriormente cuando se desarrolla en el módulo **ui.front**.

### Aplicar estilo de subrayado

Por último, como autor, podemos optar por aplicar el estilo de subrayado a determinados Componentes del título.

1. Vaya al artículo **La Skateparks** del editor de AEM Sites en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. En el modo **Editar**, elija un componente Título. Haga clic en el icono **pincel** y seleccione el estilo **Subrayado**:

   ![Aplicar estilo de subrayado](assets/style-system/apply-underline-style-title.png)

   Como autor, debería poder activar o desactivar el estilo.

1. Haga clic en el icono **Información de página** > **Vista tal como se publicó** para inspeccionar la página fuera de AEM editor.

   ![Ver como aparece publicado](assets/style-system/view-as-published.png)

   Utilice las herramientas de desarrollador del explorador para verificar que el marcado alrededor del componente Título tenga la clase CSS `cmp-title--underline` aplicada al div exterior.

## Estilo de bloque de comillas - Texto {#text-component}

A continuación, repita pasos similares para aplicar un estilo único al [Componente de texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). El componente Texto se ha procesado como proxy en el proyecto en `/apps/wknd/components/text` como parte del módulo **ui.apps**. Los estilos predeterminados de los elementos de párrafo ya se han implementado en **ui.frontender**.

Los [diseños de artículo de WKND](assets/pages-templates/wknd-article-design.xd) contienen un estilo único para el componente Texto con un bloque de comillas:

![Estilo de bloque de presupuesto - Componente de texto](assets/style-system/quote-block-style.png)

### Inspect Text Component Markup

Una vez más inspeccionaremos el marcado del componente Texto.

1. Revise el marcado del componente Texto en: [https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)

1. A continuación se muestra el marcado del componente Texto:

   ```html
   <div class="text">
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

   Notación BEM del componente de texto:

   ```plain
   BLOCK cmp-text
       ELEMENT
   ```

1. El sistema de estilos agrega una clase CSS al div exterior que rodea el componente. Por lo tanto, el marcado al que nos dirigiremos se parecerá a lo siguiente:

   ```html
   <div class="text STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

### Implementar el estilo de bloque de cotización - ui.frontender

A continuación, implementaremos el estilo de Bloque de cotización usando el módulo **ui.frontender** de nuestro proyecto.

1. Inicio el servidor de desarrollo de webpack ejecutando el siguiente comando desde el módulo **ui.frontender**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. En el IDE, abra el archivo `index.html` ubicado en: `ui.frontend/src/main/webpack/static/index.html`.
1. En `index.html` busque una instancia del componente de texto buscando el texto *&quot;Jacob Wester&quot;* (línea 210). Añada la clase `cmp-text--quote` al div circundante:

   ```diff
   - <div class="text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-text--quote text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;text-a15f39a83a&#34;:{&#34;@type&#34;:&#34;wknd/components/text&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T00:23:27Z&#34;,&#34;xdm:text&#34;:&#34;&lt;blockquote>&amp;quot;There is no better place to shred then Los Angeles.”&lt;/blockquote>\r\n&lt;p>- Jacob Wester, Pro Skater&lt;/p>\r\n&#34;}}" id="text-a15f39a83a" class="cmp-text">
            <blockquote>&quot;There is no better place to shred then Los Angeles.”</blockquote>
            <p>- Jacob Wester, Pro Skater</p>
        </div>
    </div>
   ```

1. Actualice el archivo `text.scss` ubicado en: `ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > En este caso, los elementos HTML sin procesar se dirigen a los estilos. Esto se debe a que el componente Texto proporciona un editor de texto enriquecido para los autores de contenido. La creación de estilos directamente en contenido RTE debe realizarse con cuidado y es aún más importante definir los estilos con un alcance estricto.

1. Vuelva al explorador una vez más y verá el estilo de bloque Cita agregado:

   ![Estilo de bloque de presupuesto visible en el servidor de desarrollo de webpack](assets/style-system/quoteblock-implemented-webpack.png)

1. Detenga el servidor de desarrollo de webpack.

### Añadir una directiva de texto

A continuación, agregue una nueva directiva para los componentes de Texto.

1. Implemente código base en una instancia de AEM local con sus conocimientos Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Vaya a la **Plantilla de página de artículo** ubicada en: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)).

1. En el modo **Estructura**, en el Contenedor principal **Diseño**, seleccione el icono **Política** junto al componente **Texto** que se encuentra en *Componentes permitidos*:

   ![Configuración de directiva de texto](assets/style-system/article-template-text-policy-icon.png)

1. Actualice la directiva del componente Texto con los siguientes valores:

   *Título de directiva **:  **Texto de contenido**

   *Complementos* > Estilos ** de párrafo>  *Activar estilos de párrafo*

   *Ficha*  Estilos >  *Añadir un nuevo estilo*

   **Bloque**  de cotización:  `cmp-text--quote`

   ![Directiva de componentes de texto](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Directiva de componentes de texto 2](assets/style-system/text-policy-enable-quotestyle.png)

   Haga clic en **Listo** para guardar los cambios en la directiva de texto.

### Aplicar el estilo de bloque de cotización

1. Vaya al artículo **La Skateparks** del editor de AEM Sites en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. En el modo **Editar**, elija un componente de texto. Edite el componente para incluir un elemento de comillas:

   ![Configuración del componente de texto](assets/style-system/configure-text-component.png)

1. Seleccione el componente de texto y haga clic en el icono **pincel** y seleccione el estilo **Bloque de comillas**:

   ![Aplicar el estilo de bloque de cotización](assets/style-system/quote-block-style-applied.png)

   Como autor, debería poder activar o desactivar el estilo.

## Ancho fijo: Contenedor (bono) {#layout-container}

Los componentes de contenedor se han utilizado para crear la estructura básica de la plantilla de página de artículo y proporcionar las zonas de colocación para que los autores de contenido agreguen contenido a una página. Los contenedores también pueden aprovechar el sistema de estilos, proporcionando a los autores de contenido aún más opciones para diseñar diseños.

El **Contenedor principal** de la plantilla de página de artículo contiene los dos contenedores que se pueden crear y tiene un ancho fijo.

![Contenedor principal](assets/style-system/main-container-article-page-template.png)

*Contenedor principal en la plantilla* de página de artículos.

La directiva del **Contenedor principal** establece el elemento predeterminado como `main`:

![Política de Contenedor principal](assets/style-system/main-container-policy.png)

El CSS que hace que el **Contenedor principal** se corrija se establece en el módulo **ui.frontendr** en `ui.frontend/src/main/webpack/site/styles/container_main.scss`:

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

En lugar de destinar el elemento HTML `main`, el sistema de estilos podría utilizarse para crear un estilo **Ancho fijo** como parte de la directiva de Contenedor. El sistema de estilos podría ofrecer a los usuarios la opción de alternar entre **contenedores de anchura fija** y **anchura de fluido**.

1. **Desafío**  de bonificación: utilice las lecciones aprendidas de ejercicios anteriores y utilice el Sistema de estilo para implementar un  **ancho** fijo y  **estilos de** ancho de fluido para el componente de Contenedor.

## Felicitaciones! {#congratulations}

Enhorabuena, la página de artículos está casi completamente diseñada y ha adquirido una experiencia práctica con el sistema de estilo de AEM.

### Próximos pasos {#next-steps}

Conozca los pasos end-to-end para crear un [componente de AEM personalizado](custom-component.md) que muestre el contenido creado en un cuadro de diálogo y explore el desarrollo de un modelo Sling para encapsular la lógica empresarial que rellena el HTL del componente.

Vista el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código de forma local en la plataforma Git `tutorial/style-system-solution`.

1. Clona el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Compruebe la rama `tutorial/style-system-solution`.

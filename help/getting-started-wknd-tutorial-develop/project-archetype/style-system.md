---
title: Desarrollo con el sistema de estilos
description: Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. Este tutorial cubre el desarrollo para el sistema de estilos para ampliar los componentes principales con CSS específicos de la marca y configuraciones de directiva avanzadas del editor de plantillas.
version: 6.5, Cloud Service
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4128
mini-toc-levels: 1
thumbnail: 30386.jpg
doc-type: Tutorial
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
duration: 358
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# Desarrollo con el sistema de estilos {#developing-with-the-style-system}

Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. Este tutorial cubre el desarrollo para el sistema de estilos para ampliar los componentes principales con CSS específicos de la marca y configuraciones de directiva avanzadas del editor de plantillas.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

AEM También se recomienda revisar el tutorial de [Bibliotecas del lado del cliente y flujo de trabajo front-end](client-side-libraries.md) para comprender los aspectos básicos de las bibliotecas del lado del cliente y las diversas herramientas front-end integradas en el proyecto de.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la rama `tutorial/style-system-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. AEM Implemente una base de código en una instancia de local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM Si utiliza la versión 6.5 o 6.4 de la aplicación, anexe el perfil `classic` a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) o desprotegerlo localmente cambiando a la rama `tutorial/style-system-solution`.

## Objetivo

1. AEM Aprenda a utilizar el sistema de estilos para aplicar CSS específicas de la marca a los componentes principales de la.
1. Obtenga información acerca de la notación de BEM y cómo se puede utilizar para definir con cuidado los estilos.
1. Aplicar configuraciones de directiva avanzadas con plantillas editables.

## Lo que va a generar {#what-build}

Este capítulo usa la característica [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=es) para crear variaciones de los componentes **Título** y **Texto** utilizados en la página de artículo.

![Estilos disponibles para el título](assets/style-system/styles-added-title.png)

*Estilo de subrayado disponible para el componente Título*

## Fondo {#background}

El [sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) permite a los desarrolladores y editores de plantillas crear múltiples variaciones visuales de un componente. A su vez, los autores pueden decidir qué estilo utilizar al maquetar una página. El sistema de estilos se utiliza en el resto del tutorial para lograr varios estilos únicos mientras se utilizan los componentes principales en un enfoque de código bajo.

La idea general del sistema de estilos es que los autores pueden elegir varios estilos del aspecto que debe tener un componente. Los &quot;estilos&quot; están respaldados por clases CSS adicionales que se insertan en el div exterior de un componente. En las bibliotecas de cliente, se agregan reglas CSS basadas en estas clases de estilo para que el componente cambie de aspecto.

Encontrará [documentación detallada para el sistema de estilos aquí](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=es). También hay un excelente [vídeo técnico para comprender el sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Estilo de subrayado: título {#underline-style}

El [componente Título](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html) se ha procesado como proxy en el proyecto en `/apps/wknd/components/title` como parte del módulo **ui.apps**. Los estilos predeterminados de los elementos de encabezado (`H1`, `H2`, `H3`...) ya se han implementado en el módulo **ui.frontend**.

Los [diseños de artículo de WKND](assets/pages-templates/wknd-article-design.xd) contienen un estilo único para el componente Título con un subrayado. En lugar de crear dos componentes o modificar el cuadro de diálogo del componente, se puede utilizar el sistema de estilos para permitir a los autores añadir un estilo de subrayado.

![Estilo de subrayado - Componente de título](assets/style-system/title-underline-style.png)

### Agregar una directiva de título

Añadamos una política para los componentes Título para permitir que los autores de contenido elijan el estilo Subrayado que se aplicará a componentes específicos. AEM Esto se realiza mediante el Editor de plantillas dentro de la opción de.

1. Vaya a la plantilla **Página de artículo** desde: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. En el modo **Estructura**, en el **Contenedor de diseño** principal, seleccione el icono **Directiva** junto al componente **Título** que aparece en *Componentes permitidos*:

   ![Configurar directiva de título](assets/style-system/article-template-title-policy-icon.png)

1. Cree una política para el componente Título con los siguientes valores:

   *Título de directiva&#42;*: **Título de WKND**

   *Propiedades* > *Pestaña Estilos* > *Agregar un nuevo estilo*

   **Subrayado** : `cmp-title--underline`

   ![Configuración de directiva de estilo para el título](assets/style-system/title-style-policy.png)

   Haga clic en **Listo** para guardar los cambios en la directiva de título.

   >[!NOTE]
   >
   > El valor `cmp-title--underline` rellena la clase CSS en el div exterior del marcado de HTML del componente.

### Aplicar el estilo de subrayado

Como autor, vamos a aplicar el estilo de subrayado a determinados componentes de título.

1. Vaya al artículo de **La Skateparks** en el editor de AEM Sites en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. En el modo **Editar**, elija un componente Título. Haga clic en el icono **pincel** y seleccione el estilo **Subrayado**:

   ![Aplicar el estilo de subrayado](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > En este punto, no se produce ningún cambio visible ya que el estilo `underline` no se ha implementado. En el siguiente ejercicio, se implementa este estilo.

1. AEM Haga clic en el icono **Información de la página** > **Ver tal y como se publicó** para inspeccionar la página fuera del editor que se va a.
1. Utilice las herramientas para desarrolladores del explorador para comprobar que el marcado alrededor del componente Título tiene la clase CSS `cmp-title--underline` aplicada al div externo.

   ![Div con clase de subrayado aplicada](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### Implementar el estilo de subrayado: ui.frontend

AEM A continuación, implemente el estilo Subrayado utilizando el módulo **ui.frontend** del proyecto de. AEM Se utiliza el servidor de desarrollo de Webpack empaquetado con el módulo **ui.frontend** para obtener una vista previa de los estilos *antes de* implementar en una instancia local de.

1. Iniciar el proceso `watch` desde el módulo **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   AEM Esto inicia un proceso que supervisa los cambios en el módulo `ui.frontend` y los sincroniza con la instancia de la instancia de la.


1. Devuelva su IDE y abra el archivo `_title.scss` desde: `ui.frontend/src/main/webpack/components/_title.scss`.
1. Introduzca una nueva regla para la clase `cmp-title--underline`:

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
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
   >Se considera una práctica recomendada aplicar siempre estilos de ámbito estrecho al componente de destino. Esto garantiza que los estilos adicionales no afecten a otras áreas de la página.
   >
   >Todos los componentes principales cumplen con **[la notación BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. Se recomienda dirigirse a la clase CSS externa al crear un estilo predeterminado para un componente. Otra práctica recomendada es segmentar los nombres de clase especificados por la notación de BEM de componentes principales en lugar de los elementos de HTML.

1. AEM Vuelva al explorador y a la página de la. Debería ver que el estilo Subrayado ha agregado:

   ![Estilo de subrayado visible en el servidor de desarrollo de Webpack](assets/style-system/underline-implemented-webpack.png)

1. AEM En el Editor de la, debería poder activar y desactivar el estilo **Subrayado** y ver que los cambios se reflejen visualmente.

## Estilo del bloque de comillas: texto {#text-component}

A continuación, repita pasos similares para aplicar un estilo único al [componente Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html). El componente Texto se ha procesado como proxy en el proyecto en `/apps/wknd/components/text` como parte del módulo **ui.apps**. Los estilos predeterminados de los elementos de párrafo ya se han implementado en **ui.frontend**.

Los [diseños de artículo de WKND](assets/pages-templates/wknd-article-design.xd) contienen un estilo único para el componente Texto con un bloque de comillas:

![Estilo de bloque de presupuesto - Componente de texto](assets/style-system/quote-block-style.png)

### Agregar una política de texto

A continuación, añada una directiva para los componentes Texto.

1. Vaya a la **Plantilla de página de artículo** desde: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. En el modo **Estructura**, en el **Contenedor de diseño** principal, seleccione el icono **Directiva** junto al componente **Texto** que aparece en *Componentes permitidos*:

   ![Configurar directiva de texto](assets/style-system/article-template-text-policy-icon.png)

1. Actualice la directiva del componente Texto con los siguientes valores:

   *Título de directiva&#42;*: **Texto de contenido**

   *Complementos* > *Estilos de párrafo* > *Habilitar estilos de párrafo*

   *Pestaña Estilos* > *Agregar un nuevo estilo*

   **Bloque de presupuesto** : `cmp-text--quote`

   ![Directiva de componente de texto](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Directiva de componente de texto 2](assets/style-system/text-policy-enable-quotestyle.png)

   Haga clic en **Listo** para guardar los cambios en la directiva de texto.

### Aplicar el estilo de bloque de oferta

1. Vaya al artículo de **La Skateparks** en el editor de AEM Sites en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. En el modo **Editar**, elija un componente Texto. Edite el componente para incluir un elemento de oferta:

   ![Configuración del componente Texto](assets/style-system/configure-text-component.png)

1. Seleccione el componente de texto, haga clic en el icono **pincel** y seleccione el estilo **Bloque de comillas**:

   ![Aplicar el estilo de bloque de oferta](assets/style-system/quote-block-style-applied.png)

1. Utilice las herramientas para desarrolladores del explorador para inspeccionar el marcado. Debería ver que el nombre de clase `cmp-text--quote` se ha agregado al div exterior del componente:

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### Implementar el estilo de bloque de cotización: ui.frontend

AEM A continuación, vamos a implementar el estilo de bloque de cotización usando el módulo **ui.frontend** del proyecto de.

1. Si aún no se está ejecutando, inicie el proceso `watch` desde el módulo **ui.frontend**:

   ```shell
   $ npm run watch
   ```

1. Actualizar el archivo `text.scss` desde: `ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
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
   > En este caso, los elementos de HTML sin procesar se dirigen a los estilos. Esto se debe a que el componente Texto proporciona un Editor de texto enriquecido para los autores de contenido. La creación de estilos directamente contra el contenido RTE debe hacerse con cuidado y es aún más importante enmarcar de manera ajustada los estilos.

1. Vuelva al navegador una vez más y debería ver que el estilo de bloque de cotización ha añadido:

   ![Estilo de bloque de cotización visible](assets/style-system/quoteblock-implemented.png)

1. Detenga el servidor de desarrollo de Webpack.

## Anchura fija: contenedor (adicional) {#layout-container}

Los componentes de contenedor se han utilizado para crear la estructura básica de la plantilla de página de artículo y proporcionar las zonas de colocación para que los autores de contenido añadan contenido en una página. Los contenedores también pueden utilizar el sistema de estilos, lo que proporciona a los autores de contenido aún más opciones para diseñar diseños.

El **contenedor principal** de la plantilla de la página de artículos contiene los dos contenedores con capacidad de creación y tiene un ancho fijo.

![Contenedor principal](assets/style-system/main-container-article-page-template.png)

*Contenedor principal en la plantilla de página de artículo*.

La directiva del **contenedor principal** establece el elemento predeterminado como `main`:

![Política del contenedor principal](assets/style-system/main-container-policy.png)

El CSS que hace que el **Contenedor principal** se corrija se establece en el módulo **ui.frontend** en `ui.frontend/src/main/webpack/site/styles/container_main.scss`:

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

En lugar de segmentar el elemento HTML `main`, se podría usar el sistema de estilos para crear un estilo de **ancho fijo** como parte de la directiva de contenedor. El sistema de estilos podría dar a los usuarios la opción de alternar entre **contenedores de ancho fijo** y **ancho fluido**.

1. **Desafío adicional**: usa las lecciones aprendidas de los ejercicios anteriores y usa el sistema de estilos para implementar los estilos **Anchura fija** y **Anchura fluida** para el componente Contenedor.

## Enhorabuena. {#congratulations}

AEM Enhorabuena, la página de artículos está casi diseñada y ha adquirido experiencia práctica con el sistema de estilos de la.

### Siguientes pasos {#next-steps}

AEM Conozca los pasos de extremo a extremo para crear un [componente de personalizado](custom-component.md) que muestre el contenido creado en un cuadro de diálogo y explore el desarrollo de un modelo Sling para encapsular la lógica empresarial que rellena el HTL del componente.

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/style-system-solution`.

1. Clonar el repositorio [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd).
1. Consulte la rama `tutorial/style-system-solution`.

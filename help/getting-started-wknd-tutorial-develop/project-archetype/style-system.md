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
duration: 494
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# Desarrollo con el sistema de estilos {#developing-with-the-style-system}

Aprenda a implementar estilos individuales y a reutilizar los componentes principales mediante el sistema de estilos de Experience Manager. Este tutorial cubre el desarrollo para el sistema de estilos para ampliar los componentes principales con CSS específicos de la marca y configuraciones de directiva avanzadas del editor de plantillas.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment).

También se recomienda revisar el [Bibliotecas del lado cliente y flujo de trabajo front-end](client-side-libraries.md) AEM tutorial para comprender los aspectos básicos de las bibliotecas del lado del cliente y las distintas herramientas front-end integradas en el proyecto de la.

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la `tutorial/style-system-start` bifurcar desde [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > AEM Si se utiliza la versión 6.5 o 6.4, se debe anexar la variable `classic` de perfil a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) o compruebe el código localmente cambiando a la rama `tutorial/style-system-solution`.

## Objetivo

1. AEM Aprenda a utilizar el sistema de estilos para aplicar CSS específicas de la marca a los componentes principales de la.
1. Obtenga información acerca de la notación de BEM y cómo se puede utilizar para definir con cuidado los estilos.
1. Aplicar configuraciones de directiva avanzadas con plantillas editables.

## Lo que va a generar {#what-build}

Este capítulo utiliza las [Función Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=es) para crear variaciones de **Título** y **Texto** componentes utilizados en la página Artículo.

![Estilos disponibles para el título](assets/style-system/styles-added-title.png)

*Estilo de subrayado disponible para usar en el componente Título*

## Fondo {#background}

El [Sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) permite a los desarrolladores y editores de plantillas crear varias variaciones visuales de un componente. A su vez, los autores pueden decidir qué estilo utilizar al maquetar una página. El sistema de estilos se utiliza en el resto del tutorial para lograr varios estilos únicos mientras se utilizan los componentes principales en un enfoque de código bajo.

La idea general del sistema de estilos es que los autores pueden elegir varios estilos del aspecto que debe tener un componente. Los &quot;estilos&quot; están respaldados por clases CSS adicionales que se insertan en el div exterior de un componente. En las bibliotecas de cliente, se agregan reglas CSS basadas en estas clases de estilo para que el componente cambie de aspecto.

Puede encontrar [documentación detallada para el sistema de estilos aquí](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=es). También hay un gran [Vídeo técnico para comprender el sistema de estilos](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## Estilo de subrayado: título {#underline-style}

El [Componente Título](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html) se ha proxy en el proyecto en `/apps/wknd/components/title` como parte de **ui.apps** módulo. Estilos predeterminados de los elementos de encabezado (`H1`, `H2`, `H3`...) ya se han implementado en el **ui.frontend** módulo.

El [Diseños de artículo de WKND](assets/pages-templates/wknd-article-design.xd) contener un estilo único para el componente Título con un subrayado. En lugar de crear dos componentes o modificar el cuadro de diálogo del componente, se puede utilizar el sistema de estilos para permitir a los autores añadir un estilo de subrayado.

![Estilo de subrayado: componente Título](assets/style-system/title-underline-style.png)

### Agregar una directiva de título

Añadamos una política para los componentes Título para permitir que los autores de contenido elijan el estilo Subrayado que se aplicará a componentes específicos. AEM Esto se realiza mediante el Editor de plantillas dentro de la opción de.

1. Vaya a **Página de artículo** plantilla de: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Entrada **Estructura** modo, en el **Contenedor de diseño**, seleccione la **Política** junto al icono **Título** componente enumerado en *Componentes permitidos*:

   ![Configuración de directiva de título](assets/style-system/article-template-title-policy-icon.png)

1. Cree una política para el componente Título con los siguientes valores:

   *Título de política&#42;*: **Título de WKND**

   *Propiedades* > *Pestaña Estilos* > *Añadir un nuevo estilo*

   **Subrayado** : `cmp-title--underline`

   ![Configuración de directiva de estilo para el título](assets/style-system/title-style-policy.png)

   Clic **Listo** para guardar los cambios en la directiva Título.

   >[!NOTE]
   >
   > El valor `cmp-title--underline` rellena la clase CSS en el div exterior del marcado del HTML del componente.

### Aplicar el estilo de subrayado

Como autor, vamos a aplicar el estilo de subrayado a determinados componentes de título.

1. Vaya a **La Skateparks** artículo en el editor de AEM Sites en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Entrada **Editar** , elija un componente Título. Haga clic en **pincel** y seleccione el icono **Subrayado** estilo:

   ![Aplicar el estilo de subrayado](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > En este punto, no se produce ningún cambio visible ya que la variable `underline` no se ha implementado el estilo. En el siguiente ejercicio, se implementa este estilo.

1. Haga clic en **Información de página** icono > **Ver como aparece publicado** AEM para inspeccionar la página desde fuera del editor de.
1. Utilice las herramientas para desarrolladores del explorador para comprobar que el marcado alrededor del componente Título tiene la clase CSS `cmp-title--underline` se aplica al div externo.

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

A continuación, implemente el estilo Subrayado utilizando **ui.frontend** AEM módulo del proyecto de. El servidor de desarrollo de Webpack que está empaquetado con **ui.frontend** para obtener una vista previa de los estilos *antes* AEM se utiliza la implementación en una instancia local de la.

1. Inicie el `watch` procesar desde el **ui.frontend** módulo:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   Esto inicia un proceso que supervisa los cambios en la `ui.frontend` AEM y sincronice los cambios en la instancia de.


1. Devuelve el IDE y abre el archivo `_title.scss` de: `ui.frontend/src/main/webpack/components/_title.scss`.
1. Introduzca una nueva regla para segmentar el `cmp-title--underline` clase:

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
   >Todos los componentes principales se adhieren a **[Notación BEM](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. Se recomienda dirigirse a la clase CSS externa al crear un estilo predeterminado para un componente. Otra práctica recomendada es segmentar los nombres de clase especificados por la notación de BEM de componentes principales en lugar de los elementos de HTML.

1. AEM Vuelva al explorador y a la página de la. Debería ver que el estilo Subrayado ha agregado:

   ![Estilo de subrayado visible en el servidor de desarrollo de Webpack](assets/style-system/underline-implemented-webpack.png)

1. AEM En el Editor de segmentos, debería poder activar y desactivar la opción de **Subrayado** y ver que los cambios se reflejaban visualmente.

## Estilo del bloque de comillas: texto {#text-component}

A continuación, repita pasos similares para aplicar un estilo único a la [Componente Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html). El componente Texto se ha proxy al proyecto en `/apps/wknd/components/text` como parte de **ui.apps** módulo. Los estilos predeterminados de los elementos de párrafo ya se han implementado en **ui.frontend**.

El [Diseños de artículo de WKND](assets/pages-templates/wknd-article-design.xd) contener un estilo único para el componente Texto con un bloque de comillas:

![Estilo de bloque de oferta: componente Texto](assets/style-system/quote-block-style.png)

### Agregar una política de texto

A continuación, añada una directiva para los componentes Texto.

1. Vaya a **Plantilla de página de artículo** de: [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. Entrada **Estructura** modo, en el **Contenedor de diseño**, seleccione la **Política** junto al icono **Texto** componente enumerado en *Componentes permitidos*:

   ![Configurar directiva de texto](assets/style-system/article-template-text-policy-icon.png)

1. Actualice la directiva del componente Texto con los siguientes valores:

   *Título de política&#42;*: **Texto de contenido**

   *Complementos* > *Estilos de párrafo* > *Activar estilos de párrafo*

   *Pestaña Estilos* > *Añadir un nuevo estilo*

   **Bloque de oferta** : `cmp-text--quote`

   ![Directiva de componente de texto](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![Directiva de componente de texto 2](assets/style-system/text-policy-enable-quotestyle.png)

   Clic **Listo** para guardar los cambios en la directiva de texto.

### Aplicar el estilo de bloque de oferta

1. Vaya a **La Skateparks** artículo en el editor de AEM Sites en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Entrada **Editar** , elija un componente Texto. Edite el componente para incluir un elemento de oferta:

   ![Configuración del componente Texto](assets/style-system/configure-text-component.png)

1. Seleccione el componente de texto y haga clic en **pincel** y seleccione el icono **Bloque de oferta** estilo:

   ![Aplicar el estilo de bloque de oferta](assets/style-system/quote-block-style-applied.png)

1. Utilice las herramientas para desarrolladores del explorador para inspeccionar el marcado. Debería ver el nombre de la clase `cmp-text--quote` se ha añadido al div exterior del componente:

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

A continuación, vamos a implementar el estilo de bloque de cotización utilizando **ui.frontend** AEM módulo del proyecto de.

1. Si no se está ejecutando, inicie el `watch` procesar desde el **ui.frontend** módulo:

   ```shell
   $ npm run watch
   ```

1. Actualizar el archivo `text.scss` de: `ui.frontend/src/main/webpack/components/_text.scss`:

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

El **Contenedor principal** de la plantilla Página de artículo contiene los dos contenedores que se pueden crear y tiene un ancho fijo.

![Contenedor principal](assets/style-system/main-container-article-page-template.png)

*Contenedor principal en la plantilla de la página del artículo*.

La política de la **Contenedor principal** establece el elemento predeterminado como `main`:

![Política del contenedor principal](assets/style-system/main-container-policy.png)

El CSS que hace que el **Contenedor principal** fijo se establece en la variable **ui.frontend** módulo en `ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

En lugar de segmentar el `main` HTML, se podría utilizar el sistema de estilos para crear un **Anchura fija** estilo como parte de la política de contenedor. El sistema de estilos podría proporcionar a los usuarios la opción de alternar entre **Anchura fija** y **Anchura del líquido** contenedores.

1. **Desafío de bonificación** - utilizar las lecciones aprendidas de los ejercicios anteriores y utilizar el sistema de estilos para implementar un **Anchura fija** y **Anchura del líquido** estilos del componente Contenedor.

## Enhorabuena. {#congratulations}

AEM Enhorabuena, la página de artículos está casi diseñada y ha adquirido experiencia práctica con el sistema de estilos de la.

### Pasos siguientes {#next-steps}

Conozca los pasos de extremo a extremo para crear una [AEM Componente personalizado](custom-component.md) que muestra el contenido creado en un cuadro de diálogo y explora el desarrollo de un modelo Sling para encapsular la lógica empresarial que rellena el HTL del componente.

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/style-system-solution`.

1. Clonar el [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repositorio.
1. Consulte la `tutorial/style-system-solution` Rama.

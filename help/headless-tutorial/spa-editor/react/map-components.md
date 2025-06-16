---
title: Asignación de componentes de SPA a componentes de AEM | Introducción al Editor de SPA de AEM y React
description: Obtenga información sobre cómo asignar componentes de React a componentes de Adobe Experience Manager (AEM) con AEM SPA Editor JS SDK. La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas de los componentes de la SPA en el Editor de SPA de AEM, de forma similar a la creación tradicional de AEM. También aprenderá a utilizar los componentes principales de AEM React de forma predeterminada.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
duration: 477
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '2123'
ht-degree: 0%

---

# Asignación de componentes de SPA a componentes de AEM {#map-components}

{{spa-editor-deprecation}}

Obtenga información sobre cómo asignar componentes de React a componentes de Adobe Experience Manager (AEM) con AEM SPA Editor JS SDK. La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas de los componentes de la SPA en el Editor de SPA de AEM, de forma similar a la creación tradicional de AEM.

Este capítulo profundiza en la API del modelo JSON de AEM y en cómo el contenido JSON expuesto por un componente de AEM se puede insertar automáticamente en un componente de React como props.

## Objetivo

1. Obtenga información sobre cómo asignar componentes de AEM a componentes de SPA.
1. Inspeccione cómo utiliza un componente de React las propiedades dinámicas pasadas desde AEM.
1. Aprenda a utilizar los [componentes principales de AEM de React](https://github.com/adobe/aem-react-core-wcm-components-examples) de forma predeterminada.

## Qué va a generar

Este capítulo inspecciona cómo se asigna el componente de SPA `Text` proporcionado al componente `Text` de AEM. Los componentes principales de React como el componente de SPA `Image` se utilizan en la SPA y se crean en AEM. Las características predeterminadas de las directivas **Contenedor de diseño** y **Editor de plantillas** también se pueden usar para crear una vista que tenga un aspecto un poco más variado.

![Creación final de ejemplo de capítulo](./assets/map-components/final-page.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Este capítulo es una continuación del capítulo [Integrar el SPA](integrate-spa.md); sin embargo, para continuar, todo lo que necesita es un proyecto de AEM habilitado para SPA.

## Método de asignación

El concepto básico es asignar un componente de SPA a un componente de AEM. Los componentes de AEM, del lado del servidor de ejecución, exportan contenido como parte de la API del modelo JSON. La SPA consume el contenido JSON, que se ejecuta en el lado del cliente en el explorador. Se crea una asignación 1:1 entre los componentes de SPA y un componente de AEM.

![Información general de alto nivel sobre la asignación de un componente de AEM a un componente de React](./assets/map-components/high-level-approach.png)

*Información general de alto nivel sobre la asignación de un componente de AEM a un componente de React*

## Inspeccionar el componente Texto

El [tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype) proporciona un componente `Text` asignado al [componente Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html?lang=es) de AEM. Este es un ejemplo de un componente **content**, ya que procesa *content* de AEM.

Veamos cómo funciona el componente.

### Inspeccione el modelo JSON

1. Antes de saltar al código SPA, es importante comprender el modelo JSON que proporciona AEM. Vaya a la [Biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) y vea la página del componente Texto. La biblioteca de componentes principales proporciona ejemplos de todos los componentes principales de AEM.
1. Seleccione la ficha **JSON** para uno de los ejemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Debería ver tres propiedades: `text`, `richText` y `:type`.

   `:type` es una propiedad reservada que enumera `sling:resourceType` (o ruta de acceso) del componente AEM. El valor de `:type` es lo que se usa para asignar el componente AEM al componente SPA.

   `text` y `richText` son propiedades adicionales que se exponen al componente SPA.

1. Vea la salida de JSON en [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Debería poder encontrar una entrada similar a:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspeccionar el componente SPA de texto

1. En el IDE de su elección, abra el proyecto de AEM para la SPA. Expanda el módulo `ui.frontend` y abra el archivo `Text.js` en `ui.frontend/src/components/Text/Text.js`.

1. El primer área que inspeccionaremos es la `class Text` en ~line 40:

   ```js
   class Text extends Component {
   
       get richTextContent() {
           return (<div
                   id={extractModelId(this.props.cqPath)}
                   data-rte-editelement
                   dangerouslySetInnerHTML={{__html: DOMPurify.sanitize(this.props.text)}} />
                   );
       }
   
       get textContent() {
           return <div>{this.props.text}</div>;
       }
   
       render() {
           return this.props.richText ? this.richTextContent : this.textContent;
       }
   }
   ```

   `Text` es un componente React estándar. El componente utiliza `this.props.richText` para determinar si el contenido que se va a representar será texto enriquecido o texto sin formato. El &quot;contenido&quot; real utilizado proviene de `this.props.text`.

   Para evitar un posible ataque XSS, el texto enriquecido se escapa a través de `DOMPurify` antes de usar [peligrosamenteSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) para procesar el contenido. Recupere las propiedades `richText` y `text` del modelo JSON anteriormente en el ejercicio.

1. A continuación, abra `ui.frontend/src/components/import-components.js` y eche un vistazo a `TextEditConfig` en ~line 86:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   El código anterior es responsable de determinar cuándo procesar el marcador de posición en el entorno de creación de AEM. Si el método `isEmpty` devuelve **true**, se procesará el marcador de posición.

1. Finalmente, eche un vistazo a la llamada de `MapTo` en la línea 94:

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` lo ha proporcionado AEM SPA Editor JS SDK (`@adobe/aem-react-editable-components`). La ruta de acceso `wknd-spa-react/components/text` representa el `sling:resourceType` del componente AEM. Esta ruta de acceso coincide con el `:type` expuesto por el modelo JSON observado anteriormente. `MapTo` se encarga de analizar la respuesta del modelo JSON y de pasar los valores correctos como `props` al componente SPA.

   Puede encontrar la definición del componente `Text` de AEM en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Uso de componentes principales de React

[Componentes WCM de AEM - Implementación principal de React](https://github.com/adobe/aem-react-core-wcm-components-base) y [Componentes WCM de AEM - Editor de spa - Implementación principal de React](https://github.com/adobe/aem-react-core-wcm-components-spa). Son un conjunto de componentes de interfaz de usuario reutilizables que se asignan a componentes de AEM predeterminados. La mayoría de los proyectos pueden reutilizar estos componentes como punto de partida para su propia implementación.

1. En el código del proyecto, abra el archivo `import-components.js` en `ui.frontend/src/components`.
Este archivo importa todos los componentes de la SPA que se asignan a componentes de AEM. Dada la naturaleza dinámica de la implementación del Editor de SPA, debemos hacer referencia explícita a cualquier componente de SPA vinculado a componentes con capacidad de creación de AEM. Esto permite a un autor de AEM elegir utilizar un componente en el lugar de la aplicación que desee.
1. Las siguientes instrucciones de importación incluyen componentes de SPA escritos en el proyecto:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Hay varios otros `imports` de `@adobe/aem-core-components-react-spa` y `@adobe/aem-core-components-react-base`. Se están importando los componentes principales de React y se están poniendo a disposición en el proyecto actual. A continuación, se asignan a componentes de AEM específicos del proyecto mediante `MapTo`, como en el ejemplo anterior del componente `Text`.

### Actualizar directivas de AEM

Las directivas son una función de las plantillas de AEM que proporciona a los desarrolladores y a los usuarios avanzados un control granular sobre los componentes que se pueden utilizar. Los componentes principales de React se incluyen en el código de la SPA, pero deben habilitarse mediante una directiva para poder utilizarse en la aplicación.

1. En la pantalla de inicio de AEM, vaya a **Herramientas** > **Plantillas** > **[WKND SPA React](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Seleccione y abra la plantilla **SPA Page** para editarla.

1. Seleccione el **contenedor de diseño** y haga clic en su icono **directiva** para editar la directiva:

   ![directiva de contenedor de diseño](assets/map-components/edit-spa-page-template.png)

1. En **Componentes permitidos** > **WKND SPA React - Contenido** > comprobar **Imagen**, **Teaser** y **Título**.

   ![Componentes actualizados disponibles](assets/map-components/update-components-available.png)

   En **Componentes predeterminados** > **Agregar asignación** y elija el componente **Imagen - WKND SPA React - Contenido**:

   ![Establecer componentes predeterminados](./assets/map-components/default-components.png)

   Escriba un **tipo mime** de `image/*`.

   Haga clic en **Listo** para guardar las actualizaciones de la directiva.

1. En el **contenedor de diseño**, haga clic en el icono **directiva** para el componente **Texto**.

   Cree una nueva directiva denominada **WKND SPA Text**. En **Complementos** > **Formato** > marque todas las casillas para habilitar opciones de formato adicionales:

   ![Habilitar formato RTE](assets/map-components/enable-formatting-rte.png)

   En **Complementos** > **Estilos de párrafo** > marque la casilla para **habilitar estilos de párrafo**:

   ![Activar estilos de párrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Haga clic en **Listo** para guardar la actualización de la directiva.

### Contenido del autor

1. Vaya a **Página principal** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Ahora debería poder usar los componentes adicionales **Imagen**, **Teaser** y **Título** de la página.

   ![Componentes adicionales](assets/map-components/additional-components.png)

1. También debería poder editar el componente `Text` y agregar estilos de párrafo adicionales en el modo **pantalla completa**.

   ![Edición de texto enriquecido en pantalla completa](assets/map-components/full-screen-rte.png)

1. También debería poder arrastrar y soltar una imagen desde **Buscador de recursos**:

   ![Arrastrar y soltar imagen](assets/map-components/drag-drop-image.png)

1. Experiencia con los componentes **Title** y **Teaser**.

1. Agregue sus propias imágenes a través de [AEM Assets](http://localhost:4502/assets.html/content/dam) o instale la base de código terminado para el [sitio de referencia WKND estándar](https://github.com/adobe/aem-guides-wknd/releases/latest). El [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) incluye muchas imágenes que se pueden reutilizar en la SPA de WKND. El paquete se puede instalar usando [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Administrador de paquetes instala wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspeccionar el contenedor de diseño

AEM SPA Editor SDK proporciona automáticamente compatibilidad con el **contenedor de diseño**. El **contenedor de diseño**, tal como indica el nombre, es un componente **contenedor**. Los componentes de contenedor son componentes que aceptan estructuras JSON que representan *otros* componentes y las instancian dinámicamente.

Vamos a inspeccionar más el contenedor de diseño.

1. En un explorador, vaya a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API de modelo JSON: cuadrícula interactiva](./assets/map-components/responsive-grid-modeljson.png)

   El componente **Contenedor de diseño** tiene un `sling:resourceType` de `wcm/foundation/components/responsivegrid` y el Editor de la SPA lo reconoce usando la propiedad `:type`, al igual que los componentes `Text` y `Image`.

   Las mismas capacidades para cambiar el tamaño de un componente mediante [Modo de diseño](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=es#defining-layouts-layout-mode) están disponibles con el Editor de SPA.

2. Volver a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Agregue componentes adicionales de **Image** e intente cambiar su tamaño con la opción **Diseño**:

   ![Cambiar el tamaño de la imagen mediante el modo Diseño](./assets/map-components/responsive-grid-layout-change.gif)

3. Vuelva a abrir el modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) y observe `columnClassNames` como parte del JSON:

   ![Nombres de clase de columna](./assets/map-components/responsive-grid-classnames.png)

   El nombre de clase `aem-GridColumn--default--4` indica que el componente debe tener 4 columnas de ancho basado en una cuadrícula de 12 columnas. Encontrará más detalles sobre la cuadrícula [adaptable aquí](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Vuelva al IDE y en el módulo `ui.apps` hay una biblioteca del lado del cliente definida en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Abra el archivo `less/grid.less`.

   Este archivo determina los puntos de interrupción (`default`, `tablet` y `phone`) utilizados por el **contenedor de diseño**. El propósito de este archivo es personalizarlo según las especificaciones del proyecto. Actualmente los puntos de interrupción están establecidos en `1200px` y `768px`.

5. Debe poder utilizar las capacidades adaptables y las directivas de texto enriquecido actualizadas del componente `Text` para crear una vista como la siguiente:

   ![Creación final de ejemplo de capítulo](assets/map-components/final-page.png)

## Enhorabuena. {#congratulations}

Ha aprendido a asignar componentes de la SPA a componentes de AEM y ha utilizado los componentes principales de React. También tiene la oportunidad de explorar las capacidades adaptables de **Contenedor de diseño**.

### Siguientes pasos {#next-steps}

[Navegación y enrutamiento](navigation-routing.md): descubra cómo se pueden admitir varias vistas en la SPA asignándolas a páginas de AEM con la SDK del Editor de SPA. La navegación dinámica se implementa mediante los componentes principales React y React Router.

## (Bonus) Mantener configuraciones en el control de código fuente {#bonus-configs}

En muchos casos, especialmente al principio de un proyecto de AEM, es importante mantener las configuraciones, como las plantillas y las directivas de contenido relacionadas, para el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.

Los siguientes pasos se llevarán a cabo con el IDE de código de Visual Studio y [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync), pero podrían realizarse con cualquier herramienta y IDE que haya configurado para **extraer** o **importar** contenido de una instancia local de AEM.

1. En el IDE de código de Visual Studio, asegúrese de que tiene **VSCode AEM Sync** instalado mediante la extensión Marketplace:

   ![VSCode AEM Sync](./assets/map-components/vscode-aem-sync.png)

2. Expanda el módulo **ui.content** en el explorador del proyecto y vaya a `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Haga clic con el botón derecho** en la carpeta `templates` y seleccione **Importar desde el servidor de AEM**:

   ![Plantilla de importación de VSCode](./assets/map-components/import-aem-servervscode.png)

4. Repita los pasos para importar contenido, pero seleccione la carpeta **policies** ubicada en `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspeccione el archivo de `filter.xml` ubicado en `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
    <workspaceFilter version="1.0">
        <filter root="/conf/wknd-spa-react" mode="merge"/>
        <filter root="/content/wknd-spa-react" mode="merge"/>
        <filter root="/content/dam/wknd-spa-react" mode="merge"/>
        <filter root="/content/experience-fragments/wknd-spa-react" mode="merge"/>
    </workspaceFilter>
   ```

   El archivo `filter.xml` es responsable de identificar las rutas de acceso de los nodos instalados con el paquete. Observe el `mode="merge"` en cada uno de los filtros que indica que el contenido existente no se modificará, solo se agregará contenido nuevo. Dado que los autores de contenido pueden estar actualizando estas rutas, es importante que una implementación de código sobrescriba el contenido **no**. Consulte la [documentación de FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obtener más información sobre cómo trabajar con elementos de filtro.

   Compare `ui.content/src/main/content/META-INF/vault/filter.xml` y `ui.apps/src/main/content/META-INF/vault/filter.xml` para comprender los diferentes nodos administrados por cada módulo.

## (Bonus) Crear componente de imagen personalizado {#bonus-image}

Los componentes principales de React ya han proporcionado un componente de imagen de SPA. Sin embargo, si quiere práctica adicional, cree su propia implementación de React que se asigne al [componente de imagen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=es) de AEM. El componente `Image` es otro ejemplo de un componente **content**.

### Inspeccione el JSON

Antes de saltar al código SPA, revise el modelo JSON proporcionado por AEM.

1. Vaya a los [ejemplos de imagen de la biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![Componente principal de imagen JSON](./assets/map-components/image-json.png)

   Las propiedades de `src`, `alt` y `title` se usan para rellenar el componente SPA `Image`.

   >[!NOTE]
   >
   > Hay otras propiedades de imagen expuestas (`lazyEnabled`, `widths`) que permiten a un desarrollador crear un componente de carga diferida y adaptable. El componente integrado en este tutorial es sencillo y **no** utiliza estas propiedades avanzadas.

### Implementación del componente de imagen

1. A continuación, cree una nueva carpeta denominada `Image` en `ui.frontend/src/components`.
1. Debajo de la carpeta `Image`, cree un nuevo archivo con el nombre `Image.js`.

   ![Archivo Image.js](./assets/map-components/image-js-file.png)

1. Agregar las siguientes `import` instrucciones a `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. A continuación, agregue `ImageEditConfig` para determinar cuándo se debe mostrar el marcador de posición en AEM:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   El marcador de posición se mostrará si no se ha establecido la propiedad `src`.

1. A continuación, implemente la clase `Image`:

   ```js
    export default class Image extends Component {
   
       get content() {
           return <img     className="Image-src"
                           src={this.props.src}
                           alt={this.props.alt}
                           title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       render() {
           if(ImageEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
                   <div className="Image">
                       {this.content}
                   </div>
           );
       }
   }
   ```

   El código anterior procesará un `<img>` basado en las props `src`, `alt` y `title` pasadas por el modelo JSON.

1. Agregue el código `MapTo` para asignar el componente React al componente AEM:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Observe que la cadena `wknd-spa-react/components/image` corresponde a la ubicación del componente AEM en `ui.apps` en: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Cree un nuevo archivo con el nombre `Image.css` en el mismo directorio y agregue lo siguiente:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. En `Image.js`, agregue una referencia al archivo en la parte superior debajo de las instrucciones `import`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Abra el archivo `ui.frontend/src/components/import-components.js` y agregue una referencia al nuevo componente `Image`:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. En `import-components.js`, comente la imagen del componente principal de React:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Esto garantizará que se utilice el componente de imagen personalizado en su lugar.

1. Desde la raíz del proyecto, implemente el código SPA en AEM mediante Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspeccione el SPA en AEM. Todos los componentes de imagen de la página deben seguir funcionando. Inspeccione la salida procesada y verá el marcado para el componente de imagen personalizado en lugar del componente principal React.

   *Marcado de componente de imagen personalizado*

   ```html
   <div class="Image">
       <img class="Image-src" src="/content/image-src.jpg">
   </div>
   ```

   *Marcado de imagen del componente principal de React*

   ```html
   <div class="cmp-image cq-dd-image">
       <img src="/content/image-src.jpg" class="cmp-image__image">
   </div>
   ```

   Esta es una buena introducción para ampliar e implementar sus propios componentes.

---
title: SPA AEM Asignación de componentes de a componentes de AEM SPA | Introducción al Editor de y React
description: Obtenga información sobre cómo asignar componentes de React a componentes de Adobe Experience Manager AEM AEM SPA () con el SDK de JS de Editor de. SPA AEM SPA AEM La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas en los componentes de la de componentes del Editor de componentes, de forma similar a la creación tradicional de los componentes de la. AEM También aprenderá a utilizar los componentes principales de React de forma predeterminada
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4854
thumbnail: 4854-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 497ce6d7-cd39-4fb3-b5e0-6c60845f7648
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2257'
ht-degree: 1%

---

# SPA AEM Asignación de componentes de a componentes de {#map-components}

Obtenga información sobre cómo asignar componentes de React a componentes de Adobe Experience Manager AEM AEM SPA () con el SDK de JS de Editor de. SPA AEM SPA AEM La asignación de componentes permite a los usuarios realizar actualizaciones dinámicas en los componentes de la de componentes del Editor de componentes, de forma similar a la creación tradicional de los componentes de la.

AEM AEM En este capítulo se profundiza en la API del modelo JSON de y en cómo el contenido JSON expuesto por un componente de la aplicación se puede insertar automáticamente en un componente de React como props.

## Objetivo

1. AEM SPA Obtenga información sobre cómo asignar componentes de la a componentes de la aplicación.
1. Inspect AEM muestra cómo un componente de React utiliza propiedades dinámicas pasadas desde la interfaz de usuario de.
1. Aprenda a utilizar de forma predeterminada [AEM React componentes principales](https://github.com/adobe/aem-react-core-wcm-components-examples).

## Qué va a generar

Este capítulo inspecciona cómo se proporciona el `Text` SPA AEM El componente se asigna a la `Text`componente. Reaccionar a los componentes principales como `Image` SPA SPA AEM El componente de se utiliza en el recurso y se crea en el recurso de. Funciones listas para usar del **Contenedor de diseño** y **Editor de plantillas** las directivas también se utilizan para crear una vista que sea un poco más variada en apariencia.

![Creación final de muestra de capítulo](./assets/map-components/final-page.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment). Este capítulo es una continuación de la [SPA Integración de la](integrate-spa.md) SPA AEM , sin embargo, para seguir todo lo que necesita es un proyecto de habilitado para el uso de la.

## Método de asignación

SPA AEM El concepto básico es asignar un componente de a un componente de. AEM Los componentes de, que ejecutan del lado del servidor, exportan contenido como parte de la API del modelo JSON. SPA El contenido JSON lo consume el usuario, que se ejecuta en el lado del cliente en el explorador. SPA AEM Se crea una asignación 1:1 entre los componentes de la y un componente de la.

![AEM Información general de alto nivel sobre la asignación de un componente de a un componente de React](./assets/map-components/high-level-approach.png)

*AEM Información general de alto nivel sobre la asignación de un componente de a un componente de React*

## Inspect el componente Texto

El [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype) proporciona un `Text` AEM componente asignado al componente de la lista de componentes de la [Componente Texto](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). Este es un ejemplo de **content** componente, ya que se procesa *content* AEM de la.

Veamos cómo funciona el componente.

### Inspect el modelo JSON

1. SPA AEM Antes de saltar al código de la, es importante comprender el modelo JSON que proporciona el usuario de la interfaz de usuario de JSON de la que se proporciona la aplicación. Vaya a [Biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/text.html) y vea la página del componente Texto. AEM La biblioteca de componentes principales proporciona ejemplos de todos los componentes principales de la.
1. Seleccione el **JSON** para ver uno de los ejemplos:

   ![Modelo JSON de texto](./assets/map-components/text-json.png)

   Debe ver tres propiedades: `text`, `richText`, y `:type`.

   `:type` es una propiedad reservada que enumera las `sling:resourceType` AEM (o ruta) del componente de. El valor de `:type` AEM SPA es lo que se utiliza para asignar el componente de la al componente de la.

   `text` y `richText` SPA son propiedades adicionales que se exponen al componente de la.

1. Vea la salida JSON en [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Debería poder encontrar una entrada similar a:

   ```json
   "text": {
       "id": "text-a647cec03a",
       "text": "<p>Hello World! Updated content!</p>\r\n",
       "richText": true,
       ":type": "wknd-spa-react/components/text",
       "dataLayer": {}
      }
   ```

### Inspect SPA, el componente Texto

1. AEM SPA En el IDE de su elección, abra el Proyecto de para la creación de la página de inicio de sesión de la página de la página de. Expanda el `ui.frontend` y abra el archivo. `Text.js` bajo `ui.frontend/src/components/Text/Text.js`.

1. La primera área que vamos a inspeccionar es la `class Text` en ~line 40:

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

   `Text` es un componente React estándar. El componente utiliza `this.props.richText` para determinar si el contenido que se va a procesar va a ser texto enriquecido o texto sin formato. El &quot;contenido&quot; real utilizado proviene de `this.props.text`.

   Para evitar un posible ataque XSS, el texto enriquecido se escapa mediante `DOMPurify` antes de usar [peligrosamenteSetInnerHTML](https://reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) para procesar el contenido. Recupere el `richText` y `text` propiedades del modelo JSON anteriormente en el ejercicio.

1. A continuación, abra `ui.frontend/src/components/import-components.js` eche un vistazo a la `TextEditConfig` en ~line 86:

   ```js
   const TextEditConfig = {
   emptyLabel: 'Text',
   
       isEmpty: function(props) {
           return !props || !props.text || props.text.trim().length < 1;
       }
   };
   ```

   AEM El código anterior es responsable de determinar cuándo procesar el marcador de posición en el entorno de autor de la. Si la variable `isEmpty` método devuelve **true** a continuación, se representa el marcador de posición.

1. Por último, eche un vistazo a la `MapTo` llamada en ~line 94:

   ```js
   export default MapTo('wknd-spa-react/components/text')(LazyTextComponent, TextEditConfig);
   ```

   `MapTo` AEM SPA es proporcionado por el SDK de JS del Editor de (`@adobe/aem-react-editable-components`). La ruta `wknd-spa-react/components/text` representa el `sling:resourceType` AEM del componente de la. Esta ruta coincide con el `:type` expuesto por el modelo JSON observado anteriormente. `MapTo` se encarga de analizar la respuesta del modelo JSON y pasar los valores correctos como `props` SPA al componente de la.

   AEM Puede encontrar la `Text` definición del componente en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/text`.

## Uso de componentes principales de React

[AEM Componentes de WCM: implementación principal de React](https://github.com/adobe/aem-react-core-wcm-components-base) y [AEM Componentes de WCM - Editor de spa - Implementación principal de React](https://github.com/adobe/aem-react-core-wcm-components-spa). AEM Se trata de un conjunto de componentes de la interfaz de usuario reutilizables que se asignan a componentes de la interfaz de usuario listos para usar. La mayoría de los proyectos pueden reutilizar estos componentes como punto de partida para su propia implementación.

1. En el código del proyecto, abra el archivo. `import-components.js` en `ui.frontend/src/components`.
SPA AEM Este archivo importa todos los componentes de la que se asignan a componentes de la. SPA SPA AEM Dada la naturaleza dinámica de la implementación del Editor de, debemos hacer referencia explícita a cualquier componente de la lista de componentes que se pueden crear con la ayuda de componentes que se pueden crear con la capacidad de crear de manera. AEM Esto permite a un autor de la elegir utilizar un componente en el lugar de la aplicación que desee.
1. SPA Las siguientes instrucciones de importación incluyen componentes de escritos en el proyecto:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   ```

1. Hay varios más `imports` de `@adobe/aem-core-components-react-spa` y `@adobe/aem-core-components-react-base`. Se están importando los componentes principales de React y se están poniendo a disposición en el proyecto actual. AEM A continuación, se asignan a componentes de proyecto específicos de la mediante el `MapTo`, igual que con el `Text` ejemplo de componente anterior.

### AEM Actualizar directivas

AEM Las directivas son una función de las plantillas de que proporciona a los desarrolladores y a los usuarios avanzados un control granular sobre los componentes que se pueden utilizar. SPA Los componentes principales de React se incluyen en el código de la, pero deben habilitarse mediante una directiva para poder utilizarse en la aplicación.

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **Plantillas** > **[SPA Reacción de WKND](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd-spa-react)**.

1. Seleccione y abra el **SPA Página de** plantilla para editar.

1. Seleccione el **Contenedor de diseño** y haga clic en su **directiva** para editar la directiva:

   ![directiva de contenedor de diseño](assets/map-components/edit-spa-page-template.png)

1. En **Componentes permitidos** > **SPA Reacción de WKND: contenido** > comprobar **Imagen**, **Teaser**, y **Título**.

   ![Componentes actualizados disponibles](assets/map-components/update-components-available.png)

   En **Componentes predeterminados** > **Añadir asignación** y elija la **SPA Imagen - WKND React - Contenido** componente:

   ![Definición de componentes predeterminados](./assets/map-components/default-components.png)

   Introduzca una **tipo MIME** de `image/*`.

   Clic **Listo** para guardar las actualizaciones de directivas.

1. En el **Contenedor de diseño** haga clic en **directiva** para el **Texto** componente.

   Cree una nueva directiva denominada **SPA Texto de WKND**. En **Complementos** > **Formato** > marque todas las casillas para habilitar opciones de formato adicionales:

   ![Habilitar formato RTE](assets/map-components/enable-formatting-rte.png)

   En **Complementos** > **Estilos de párrafo** > marque la casilla para **Activar estilos de párrafo**:

   ![Activar estilos de párrafo](./assets/map-components/text-policy-enable-paragraphstyles.png)

   Clic **Listo** para guardar la actualización de la directiva.

### Contenido del autor

1. Vaya a **Homepage** [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).

1. Ahora debería poder utilizar los componentes adicionales **Imagen**, **Teaser**, y **Título** en la página.

   ![Componentes adicionales](assets/map-components/additional-components.png)

1. También debería poder editar la variable `Text` y agregar estilos de párrafo adicionales en **pantalla completa** modo.

   ![Edición de texto enriquecido en pantalla completa](assets/map-components/full-screen-rte.png)

1. También debe poder arrastrar y soltar una imagen desde el **Buscador de recursos**:

   ![Arrastrar y soltar imagen](assets/map-components/drag-drop-image.png)

1. Experiencia con el **Título** y **Teaser** componentes.

1. Añada sus propias imágenes mediante [AEM Assets](http://localhost:4502/assets.html/content/dam) o instale el código final base para el estándar [Sitio de referencia de WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). El [Sitio de referencia de WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) SPA incluye muchas imágenes que se pueden reutilizar en la interfaz de usuario de. El paquete se puede instalar utilizando [AEM Administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp).

   ![Administrador de paquetes instalar wknd.all](./assets/map-components/package-manager-wknd-all.png)

## Inspect el contenedor de diseño

Compatibilidad con el **Contenedor de diseño** AEM SPA es proporcionado automáticamente por el SDK de Editor de. El **Contenedor de diseño**, como indica su nombre, es un **contenedor** componente. Los componentes de contenedor son componentes que aceptan estructuras JSON que representan *otro* componentes y crearlos dinámicamente.

Vamos a inspeccionar más el contenedor de diseño.

1. En un explorador, vaya a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   ![API de modelo JSON: cuadrícula interactiva](./assets/map-components/responsive-grid-modeljson.png)

   El **Contenedor de diseño** el componente tiene un `sling:resourceType` de `wcm/foundation/components/responsivegrid` SPA y es reconocido por el Editor de la mediante el `:type` propiedad, al igual que el `Text` y `Image` componentes.

   Las mismas capacidades para cambiar el tamaño de un componente mediante [Modo de diseño](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html#defining-layouts-layout-mode) SPA están disponibles con el Editor de.

2. Volver a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). Añadir más **Imagen** componentes e intente cambiar su tamaño con el **Diseño** opción:

   ![Cambiar el tamaño de la imagen mediante el modo Diseño](./assets/map-components/responsive-grid-layout-change.gif)

3. Vuelva a abrir el modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) y observe la `columnClassNames` como parte del JSON:

   ![Nombres de clase de columna](./assets/map-components/responsive-grid-classnames.png)

   El nombre de la clase `aem-GridColumn--default--4` indica que el componente debe tener 4 columnas de ancho en función de una cuadrícula de 12 columnas. Más detalles acerca de [cuadrícula adaptable se puede encontrar aquí](https://adobe-marketing-cloud.github.io/aem-responsivegrid/).

4. Vuelva al IDE y, en el `ui.apps` : hay una biblioteca del lado del cliente definida en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-grid`. Abra el archivo `less/grid.less`.

   Este archivo determina los puntos de interrupción (`default`, `tablet`, y `phone`) utilizado por el **Contenedor de diseño**. El propósito de este archivo es personalizarlo según las especificaciones del proyecto. Actualmente los puntos de interrupción están configurados en `1200px` y `768px`.

5. Debe poder utilizar las capacidades adaptables y las políticas de texto enriquecido actualizadas del `Text` para crear una vista como la siguiente:

   ![Creación final de muestra de capítulo](assets/map-components/final-page.png)

## Enhorabuena. {#congratulations}

SPA AEM Felicidades, ha aprendido a asignar componentes de a componentes principales y ha utilizado los componentes principales de React. También tiene la oportunidad de explorar las capacidades de respuesta de **Contenedor de diseño**.

### Pasos siguientes {#next-steps}

[Navegación y enrutamiento](navigation-routing.md) SPA AEM SPA : Descubra cómo se pueden admitir varias vistas en la asignando a páginas de la página con el SDK de Editor de la página de la página de la página de la aplicación de la versión de la aplicación. La navegación dinámica se implementa mediante los componentes principales React y React Router.

## (Bonus) Mantener configuraciones en el control de código fuente {#bonus-configs}

AEM En muchos casos, especialmente al principio de un proyecto de, es importante mantener las configuraciones, como las plantillas y las directivas de contenido relacionadas, para el control de código fuente. Esto garantiza que todos los desarrolladores trabajen con el mismo conjunto de contenido y configuraciones y puede garantizar una coherencia adicional entre entornos. Una vez que un proyecto alcanza un cierto nivel de madurez, la práctica de administrar plantillas se puede transferir a un grupo especial de usuarios avanzados.

Los siguientes pasos se realizarán mediante el IDE de código de Visual Studio y [AEM Sincronización de VSCode con la](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) pero podría estar utilizando cualquier herramienta y cualquier IDE que haya configurado para **extraer** o **importar** AEM contenido de una instancia local de.

1. En el IDE de código de Visual Studio, asegúrese de que tiene **AEM Sincronización de VSCode con la** instalado mediante la extensión Marketplace:

   ![AEM Sincronización de VSCode con la](./assets/map-components/vscode-aem-sync.png)

2. Expanda el **ui.content** en el Explorador de proyectos y vaya a `/conf/wknd-spa-react/settings/wcm/templates`.

3. **Haga clic con el botón derecho** el `templates` carpeta y seleccione **AEM Importar desde servidor de**:

   ![Plantilla de importación de VSCode](./assets/map-components/import-aem-servervscode.png)

4. Repita los pasos para importar contenido, pero seleccione **directivas** carpeta ubicada en `/conf/wknd-spa-react/settings/wcm/templates/policies`.

5. Inspect el `filter.xml` archivo ubicado en `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   El `filter.xml` es responsable de identificar las rutas de los nodos que se instalan con el paquete. Observe el `mode="merge"` en cada uno de los filtros que indica que el contenido existente no se modificará, solo se añade contenido nuevo. Dado que los autores de contenido pueden estar actualizando estas rutas, es importante que la implementación de código haga lo siguiente **no** sobrescribir contenido. Consulte la [Documentación de FileVault](https://jackrabbit.apache.org/filevault/filter.html) para obtener más información sobre cómo trabajar con elementos de filtro.

   Comparar `ui.content/src/main/content/META-INF/vault/filter.xml` y `ui.apps/src/main/content/META-INF/vault/filter.xml` para comprender los diferentes nodos administrados por cada módulo.

## (Bonus) Crear componente de imagen personalizado {#bonus-image}

SPA Los componentes principales de React ya han proporcionado un componente de imagen de. AEM Sin embargo, si desea realizar prácticas adicionales, cree su propia implementación de React que se asigne a la aplicación de la aplicación de la aplicación de la aplicación de la forma de [Componente de imagen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html?lang=es). El `Image` Este componente es otro ejemplo de **content** componente.

### Inspect el JSON

SPA AEM Antes de saltar al código de la, revise el modelo JSON proporcionado por el administrador de códigos de.

1. Vaya a [Ejemplos de imágenes en la biblioteca de componentes principales](https://www.aemcomponents.dev/content/core-components-examples/library/core-content/image.html).

   ![Componente principal de imagen JSON](./assets/map-components/image-json.png)

   Propiedades de `src`, `alt`, y `title` SPA se utilizan para rellenar la `Image` componente.

   >[!NOTE]
   >
   > Hay otras propiedades de imagen expuestas (`lazyEnabled`, `widths`) que permiten a un desarrollador crear un componente de carga diferida y adaptable. El componente integrado en este tutorial es sencillo y tiene **no** utilice estas propiedades avanzadas.

### Implementación del componente de imagen

1. A continuación, cree una nueva carpeta llamada `Image` bajo `ui.frontend/src/components`.
1. Debajo del `Image` carpeta crear un nuevo archivo llamado `Image.js`.

   ![Archivo Image.js](./assets/map-components/image-js-file.png)

1. Añada lo siguiente `import` instrucciones para `Image.js`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   ```

1. A continuación, añada el `ImageEditConfig` AEM para determinar cuándo se debe mostrar el marcador de posición en el código de tiempo de:

   ```js
   export const ImageEditConfig = {
   
       emptyLabel: 'Image',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   ```

   El marcador de posición mostrará si la variable `src` no se ha establecido la propiedad.

1. A continuación, implemente la `Image` clase:

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

   El código anterior genera un `<img>` en función de las props `src`, `alt`, y `title` pasado por el modelo JSON.

1. Añada el `MapTo` AEM código para asignar el componente React al componente de:

   ```js
   MapTo('wknd-spa-react/components/image')(Image, ImageEditConfig);
   ```

   Observe la cadena `wknd-spa-react/components/image` AEM corresponde a la ubicación del componente de la en `ui.apps` a las: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image`.

1. Cree un nuevo archivo llamado `Image.css` en el mismo directorio y agregue lo siguiente:

   ```scss
   .Image-src {
       margin: 1rem 0;
       width: 100%;
       border: 0;
   }
   ```

1. Entrada `Image.js` agregue una referencia al archivo en la parte superior debajo de la etiqueta `import` frases:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   require('./Image.css');
   ```

1. Abra el archivo `ui.frontend/src/components/import-components.js` y añada una referencia a la nueva `Image` componente:

   ```js
   import './Page/Page';
   import './Text/Text';
   import './Container/Container';
   import './ExperienceFragment/ExperienceFragment';
   import './Image/Image'; //add reference to Image component
   ```

1. Entrada `import-components.js` comente la imagen del componente principal de React:

   ```js
   //MapTo('wknd-spa-react/components/image')(ImageV2, {isEmpty: ImageV2IsEmptyFn});
   ```

   Esto garantizará que se utilice el componente de imagen personalizado en su lugar.

1. SPA AEM Desde la raíz del proyecto, implemente el código de para que se ejecute mediante Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Inspect SPA AEM el en la. Todos los componentes de imagen de la página deben seguir funcionando. Inspect muestra el resultado procesado y debería ver el marcado del componente de imagen personalizado en lugar del componente principal React.

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

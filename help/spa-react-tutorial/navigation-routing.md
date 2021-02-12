---
title: Añadir navegación y enrutamiento | Introducción al Editor de SPA de AEM y Reacción
description: Obtenga información sobre cómo se pueden admitir varias vistas en el SPA asignando páginas AEM con el SDK del Editor de SPA. La navegación dinámica se implementa mediante el enrutador de reacción y se agrega a un componente de encabezado existente.
sub-product: sitios
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '2112'
ht-degree: 1%

---


# Añadir navegación y enrutamiento {#navigation-routing}

Obtenga información sobre cómo se pueden admitir varias vistas en el SPA asignando páginas AEM con el SDK del Editor de SPA. La navegación dinámica se implementa mediante el enrutador de reacción y se agrega a un componente de encabezado existente.

## Objetivo

1. Comprender las opciones de enrutamiento del modelo de SPA disponibles al utilizar el Editor de SPA.
1. Aprenda a utilizar [Reaccionar router](https://reacttraining.com/react-router/) para navegar entre diferentes vistas del SPA.
1. Implemente una navegación dinámica impulsada por la jerarquía de páginas de AEM.

## Qué va a generar

Este capítulo agregará un menú de navegación a un componente `Header` existente. El menú de navegación estará impulsado por la jerarquía de páginas AEM y utilizará el modelo JSON proporcionado por el [Componente principal de navegación](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html).

![Navegación implementada](assets/navigation-routing/final-navigation-implemented.gif)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtener el código

1. Descargue el punto de partida de este tutorial a través de Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Implemente el código base en una instancia de AEM local mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si utiliza [AEM 6.x](overview.md#compatibility) agregue el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Instale el paquete terminado para el [sitio de referencia WKND tradicional](https://github.com/adobe/aem-guides-wknd/releases/latest). Las imágenes proporcionadas por [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) se reutilizarán en el SPA WKND. El paquete se puede instalar mediante [Administrador de paquetes de AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o extraer el código localmente cambiando a la rama `React/navigation-routing-solution`.

## Actualizaciones de encabezado de Inspect {#inspect-header}

En capítulos anteriores, el componente `Header` se agregó como un componente de Reacción pura incluido mediante `App.js`. En este capítulo, el componente `Header` se ha eliminado y se agregará mediante el [Editor de plantillas](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Esto permitirá a los usuarios configurar el menú de navegación de `Header` desde dentro de AEM.

>[!NOTE]
>
> Ya se han realizado varias actualizaciones de CSS y JavaScript en la base de código para inicio de este capítulo. Para centrarse en los conceptos principales, no **todos** de los cambios de código se analizan. Puede vista de los cambios completos [aquí](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. En el IDE de su elección, abra el proyecto de inicio de SPA para este capítulo.
1. Debajo del módulo `ui.frontend` inspeccione el archivo `Header.js` en: `ui.frontend/src/components/Header/Header.js`.

   Se han realizado varias actualizaciones, incluida la adición de un `HeaderEditConfig` y un `MapTo` para permitir que el componente se asigne a un componente de AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. En el módulo `ui.apps` inspeccione la definición de componente del componente AEM `Header`: `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   El componente AEM `Header` heredará toda la funcionalidad del [Componente principal de navegación](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) mediante la propiedad `sling:resourceSuperType`.

## Añadir el encabezado a la plantilla {#add-header-template}

1. Abra un explorador e inicie sesión en AEM, [http://localhost:4502/](http://localhost:4502/). La base de código de inicio ya debe implementarse.
1. Vaya a la **Plantilla de página SPA**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Seleccione el **Contenedor de diseño raíz** más externo y haga clic en su icono **Política**. Tenga cuidado **no** para seleccionar el **Contenedor de diseño** desbloqueado para la creación.

   ![Seleccione el icono de directiva de contenedor de diseño raíz](assets/navigation-routing/root-layout-container-policy.png)

1. Cree una nueva directiva denominada **Estructura SPA**:

   ![Política de estructura SPA](assets/navigation-routing/spa-policy-update.png)

   En **Componentes permitidos** > **General** > seleccione el componente **Contenedor de diseño**.

   En **Componentes permitidos** > **WKND SPA REACT - STRUCTURE** > seleccione el componente **Encabezado**:

   ![Seleccionar componente de encabezado](assets/navigation-routing/select-header-component.png)

   En **Componentes permitidos** > **WKND SPA REACT - Contenido** > seleccione los componentes **Imagen** y **Texto**. Debe tener 4 componentes en total seleccionados.

   Haga clic en **Listo** para guardar los cambios.

1. Actualice la página y agregue el componente **Header** sobre el **Contenedor de diseño** no bloqueado:

   ![agregar componente Encabezado a plantilla](./assets/navigation-routing/add-header-component.gif)

1. Seleccione el componente **Header** y haga clic en su icono **Policy** para editar la directiva.
1. Cree una nueva directiva con un **Título de directiva** de **Encabezado de SPA WKND**.

   En **Propiedades**:

   * Establezca la **Raíz de navegación** en `/content/wknd-spa-react/us/en`.
   * Establezca **Excluir niveles raíz** en **1**.
   * Desmarque **Recopilar todas las páginas secundarias**.
   * Establezca la **Profundidad de la estructura de navegación** en **3**.

   ![Configurar directiva de encabezado](assets/navigation-routing/header-policy.png)

   Esto recopilará los dos niveles de navegación más profundos debajo de `/content/wknd-spa-react/us/en`.

1. Después de guardar los cambios, debe ver el `Header` relleno como parte de la plantilla:

   ![Componente de encabezado rellenado](assets/navigation-routing/populated-header.png)

## Crear páginas secundarias

A continuación, cree páginas adicionales en AEM que sirvan como las distintas vistas del SPA. También inspeccionaremos la estructura jerárquica del modelo JSON proporcionado por AEM.

1. Vaya a la consola **Sites**: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Seleccione la Página de inicio **WKND SPA React** y haga clic en **Crear** > **Página**:

   ![Crear nueva página](assets/navigation-routing/create-new-page.png)

1. En **Plantilla** seleccione **Página SPA**. En **Propiedades** escriba **Página 1** para **Título** y **página-1** como nombre.

   ![Introduzca las propiedades iniciales de la página](assets/navigation-routing/initial-page-properties.png)

   Haga clic en **Crear** y, en la ventana emergente de cuadro de diálogo, haga clic en **Abrir** para abrir la página en el Editor de SPA de AEM.

1. Añada un nuevo componente **Texto** en el **Contenedor de diseño principal**. Edite el componente e introduzca el texto: **Página 1** con el elemento RTE y el elemento **H1** (tendrá que entrar al modo de pantalla completa para cambiar los elementos de párrafo)

   ![Página de contenido de muestra 1](assets/navigation-routing/page-1-sample-content.png)

   No dude en añadir contenido adicional, como una imagen.

1. Vuelva a la consola de AEM Sites y repita los pasos anteriores, creando una segunda página denominada **Página 2** como un elemento secundario de **Página 1**.
1. Por último, cree una tercera página, **Página 3** pero como **secundario** de **Página 2**. Una vez completada la jerarquía del sitio, debe tener el siguiente aspecto:

   ![Jerarquía del sitio de muestra](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. En una nueva ficha, abra la API de modelo JSON proporcionada por AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Este contenido JSON se solicita cuando se carga por primera vez el SPA. La estructura exterior es similar a la siguiente:

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {},
       "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
       }
   }
   ```

   Debajo de `:children` debe ver una entrada para cada una de las páginas creadas. El contenido de todas las páginas se encuentra en esta solicitud JSON inicial. Una vez implementado el enrutamiento de navegación, las vistas posteriores del SPA se cargarán rápidamente, ya que el contenido ya está disponible en el cliente.

   No es aconsejable cargar **ALL** del contenido de un SPA en la solicitud JSON inicial, ya que esto ralentizaría la carga inicial de la página. A continuación, veamos cómo se recopila la profundidad de jerarquía de las páginas.

1. Vaya a la plantilla **SPA raíz** en: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Haga clic en el **menú de propiedades de página** > **Política de página**:

   ![Abra la directiva de página para SPA raíz](assets/navigation-routing/open-page-policy.png)

1. La plantilla **SPA raíz** tiene una ficha **Estructura jerárquica** adicional para controlar el contenido JSON recopilado. La **Profundidad de estructura** determina la profundidad en la jerarquía del sitio para recopilar páginas secundarias debajo de la **raíz**. También puede utilizar el campo **Patrones de estructura** para filtrar páginas adicionales basadas en una expresión normal.

   Actualice la **Profundidad de estructura** a **2**:

   ![Actualizar profundidad de estructura](assets/navigation-routing/update-structure-depth.png)

   Haga clic en **Listo** para guardar los cambios en la directiva.

1. Vuelva a abrir el modelo JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
       "/content/wknd-spa-react/us/en/home": {},
       "/content/wknd-spa-react/us/en/home/page-1": {},
       "/content/wknd-spa-react/us/en/home/page-2": {}
       }
   }
   ```

   Observe que la ruta **Página 3** se ha eliminado: `/content/wknd-spa-react/us/en/home/page-2/page-3` del modelo JSON inicial.

   Posteriormente, observaremos cómo el SDK del Editor SPA de AEM puede cargar dinámicamente contenido adicional.

## Implementar la navegación

A continuación, implemente el menú de navegación como parte del `Header`. Podríamos agregar el código directamente en `Header.js` pero una mejor práctica con es evitar los componentes grandes. En su lugar, implementaremos un componente de SPA `Navigation` que podría reutilizarse posteriormente.

1. Revise el JSON expuesto por el componente AEM `Header` en [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json):

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   La naturaleza jerárquica de las páginas AEM se modela en el JSON y se puede utilizar para rellenar un menú de navegación. Recuerde que el componente `Header` hereda toda la funcionalidad del [Componente principal de navegación](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) y que el contenido expuesto a través del JSON se asignará automáticamente a las propiedades de React.

1. Abra una nueva ventana de terminal y vaya a la carpeta `ui.frontend` del proyecto de SPA. Inicio el **webpack-dev-server** con el comando `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Abra una nueva ficha del explorador y vaya a [http://localhost:3000/](http://localhost:3000/).

   El **webpack-dev-server** debe configurarse para representar el modelo JSON desde una instancia local de AEM (`ui.frontend/.env.development`). Esto nos permitirá codificar directamente con el contenido creado en AEM en el ejercicio anterior. Asegúrese de que está autenticado en AEM en la misma sesión de navegación.

   ![alternancia de menús](./assets/navigation-routing/nav-toggle-static.gif)

   El `Header` tiene ya implementada la funcionalidad de alternancia de menú. A continuación, implemente el menú de navegación.

1. Vuelva al IDE de su elección y abra el `Header.js` en `ui.frontend/src/components/Header/Header.js`.
1. Actualice el método `homeLink()` para eliminar la cadena codificada y utilizar las props dinámicas transferidas por el componente AEM:

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   El código anterior rellenará una dirección URL basada en el elemento de navegación raíz configurado por el componente. `homeLink()` se utiliza para rellenar el logotipo en el  `logo()` método y se utiliza para determinar si el botón Atrás debe mostrarse en  `backButton()`.

   Guarde los cambios en `Header.js`.

1. Añada una línea en la parte superior de `Header.js` para importar el componente `Navigation` debajo de las otras importaciones:

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. A continuación, actualice el método `get navigation()` para crear una instancia del componente `Navigation`:

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Como se mencionó anteriormente, en lugar de implementar la navegación dentro del `Header` implementaremos la mayoría de la lógica en el componente `Navigation`.  Las props del `Header` incluyen la estructura JSON necesaria para crear el menú, pasamos todas las props.
1. Abra el archivo `Navigation.js` en `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implementar el método `renderGroupNav(children)`:

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   Este método toma una matriz de elementos de navegación, `children`, y crea una lista sin ordenar. A continuación, repite la matriz y pasa el elemento a `renderNavItem`, que se implementará a continuación.

1. Implementar el `renderNavItem`:

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   Este método procesa un elemento de lista, con clases CSS basadas en las propiedades `level` y `active`. A continuación, el método llama a `renderLink` para crear la etiqueta de anclaje. Dado que el contenido `Navigation` es jerárquico, se utiliza una estrategia recursiva para llamar a `renderGroupNav` para los elementos secundarios del elemento actual.

1. Implementar el método `renderLink`:

   Añada un método de importación para el componente [Link](https://reacttraining.com/react-router/web/api/Link), parte del enrutador de React, en la parte superior del archivo con las demás importaciones:

   ```js
   import {Link} from "react-router-dom";
   ```

   A continuación, finalice la implementación del método `renderLink`:

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Observe que en lugar de utilizar una etiqueta de anclaje normal, `<a>`, se utiliza el componente [Link](https://reacttraining.com/react-router/web/api/Link). Esto garantiza que no se active una actualización completa de la página y, en su lugar, aprovecha el enrutador de React proporcionado por el SDK de JS del Editor SPA de AEM.

1. Guarde los cambios en `Navigation.js` y vuelva a **webpack-dev-server**: [http://localhost:3000](http://localhost:3000)

   ![Navegación de encabezado completada](assets/navigation-routing/completed-header.png)

   Para abrir la navegación, haga clic en el botón de alternancia del menú y verá los vínculos de navegación rellenados. Debería poder navegar a diferentes vistas del SPA.

## Inspect el Enrutamiento SPA

Ahora que la navegación se ha implementado, inspeccione el enrutamiento en AEM.

1. En el IDE, abra el archivo `index.js` en `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   Observe que el `App` está envuelto en el componente `Router` de [Enrutador de reacción](https://reacttraining.com/react-router/). El `ModelManager`, proporcionado por el SDK de JS AEM Editor SPA, agrega las rutas dinámicas a AEM páginas basadas en la API de modelo JSON.

1. Abra un terminal, desplácese hasta la raíz del proyecto e implemente el proyecto para AEM con sus conocimientos Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Vaya a la página principal de SPA en AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) y abra las herramientas de desarrollador del explorador. Las capturas de pantalla siguientes se obtienen del navegador Google Chrome.

   Actualice la página y debe ver una solicitud XHR a `/content/wknd-spa-react/us/en.model.json`, que es la raíz de SPA. Tenga en cuenta que solo se incluyen tres páginas secundarias en función de la configuración de profundidad de jerarquía de la plantilla raíz de SPA realizada anteriormente en el tutorial. Esto no incluye **Página 3**.

   ![Solicitud JSON inicial: raíz SPA](assets/navigation-routing/initial-json-request.png)

1. Con las herramientas de desarrollador abiertas, utilice la navegación `Header` para navegar a **Página 3**:

   ![Navegación por la página 3](assets/navigation-routing/page-three-navigation.png)

   Observe que se realiza una nueva solicitud XHR a: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Página tres Solicitud XHR](assets/navigation-routing/page-3-xhr-request.png)

   El Administrador de modelos de AEM entiende que el contenido **Página 3** JSON no está disponible y automáticamente déclencheur la solicitud XHR adicional.

1. Continúe navegando por la SPA mediante los distintos vínculos de navegación del componente `Header`. Observe que no se realizan solicitudes XHR adicionales y que no se actualiza la página completa. Esto hace que el SPA sea más rápido para el usuario final y reduce las solicitudes innecesarias a AEM.

   ![Navegación implementada](assets/navigation-routing/final-navigation-implemented.gif)

1. Experimente con vínculos profundos navegando directamente a: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Observe que el botón Atrás del explorador sigue funcionando.

## Felicitaciones! {#congratulations}

Enhorabuena, ha aprendido cómo se pueden admitir varias vistas en el SPA asignando a AEM páginas con el SDK del Editor de SPA. La navegación dinámica se ha implementado mediante React Router y se ha agregado al componente `Header`.

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) o extraer el código localmente cambiando a la rama `React/navigation-routing-solution`.

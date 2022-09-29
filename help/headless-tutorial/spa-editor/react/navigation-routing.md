---
title: Adición de la navegación y el enrutamiento | Introducción al AEM SPA Editor y React
description: Aprenda cómo se pueden admitir varias vistas de la SPA asignando páginas AEM con el SDK del Editor de SPA. La navegación dinámica se implementa mediante el router React y los componentes principales React.
sub-product: sites
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# Adición de la navegación y el enrutamiento {#navigation-routing}

Aprenda cómo se pueden admitir varias vistas de la SPA asignando páginas AEM con el SDK del Editor de SPA. La navegación dinámica se implementa mediante el router React y los componentes principales React.

## Objetivo

1. Comprender las opciones de enrutamiento del modelo de SPA disponibles al utilizar el SPA Editor.
1. Aprenda a utilizar [Enrutador React](https://reacttraining.com/react-router/) para desplazarse entre las distintas vistas de la SPA.
1. Utilice AEM componentes principales de React para implementar una navegación dinámica controlada por la jerarquía de páginas de AEM.

## Qué va a generar

Este capítulo agregará navegación a un SPA en AEM. El menú de navegación está impulsado por la jerarquía de páginas AEM y utilizará el modelo JSON proporcionado por la variable [Componente principal de navegación](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navegación agregada](assets/navigation-routing/navigation-added.png)

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Este capítulo es una continuación del [Asignar componentes](map-components.md) capítulo, sin embargo, para seguir todo lo que necesita es un proyecto de AEM habilitado para SPA implementado en una instancia de AEM local.

## Agregar la navegación a la plantilla {#add-navigation-template}

1. Abra un explorador e inicie sesión en AEM. [http://localhost:4502/](http://localhost:4502/). La base del código de inicio ya debe implementarse.
1. Vaya a la **Plantilla de página SPA**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Seleccione la parte exterior **Contenedor de diseño raíz** y haga clic en **Política** icono. Tenga cuidado **not** para seleccionar el **Contenedor de diseño** desbloqueado para la creación.

   ![Seleccione el icono de la política del contenedor de diseño raíz](assets/navigation-routing/root-layout-container-policy.png)

1. Cree una nueva directiva con el nombre **Estructura SPA**:

   ![Política de estructura SPA](assets/navigation-routing/spa-policy-update.png)

   En **Componentes permitidos** > **General** > seleccione el **Contenedor de diseño** componente.

   En **Componentes permitidos** > **WKND SPA REACT - ESTRUCTURA** > seleccione el **Navegación** componente:

   ![Seleccionar componente de navegación](assets/navigation-routing/select-navigation-component.png)

   En **Componentes permitidos** > **WKND SPA REACT: contenido** > seleccione el **Imagen** y **Texto** componentes. Debe tener 4 componentes totales seleccionados.

   Haga clic en **Listo** para guardar los cambios.

1. Actualice la página y agregue la variable **Navegación** componente encima del desbloqueado **Contenedor de diseño**:

   ![añadir componente Navegación a plantilla](assets/navigation-routing/add-navigation-component.png)

1. Seleccione el **Navegación** y haga clic en su **Política** para editar la directiva.
1. Cree una nueva directiva con un **Título de la política** de **Navegación SPA**.

   En el **Propiedades**:

   * Configure las variables **Raíz de navegación** a `/content/wknd-spa-react/us/en`.
   * Configure las variables **Excluir niveles raíz** a **1**.
   * Desmarcar **Recopilar todas las páginas secundarias**.
   * Configure las variables **Profundidad de la estructura de navegación** a **3**.

   ![Configurar directiva de navegación](assets/navigation-routing/navigation-policy.png)

   Esto recopilará los 2 niveles de navegación situados debajo `/content/wknd-spa-react/us/en`.

1. Después de guardar los cambios, debería ver la variable `Navigation` como parte de la plantilla:

   ![Componente de navegación rellenado](assets/navigation-routing/populated-navigation.png)

## Crear páginas secundarias

A continuación, cree páginas adicionales en AEM que sirvan como las diferentes vistas de la SPA. También inspeccionaremos la estructura jerárquica del modelo JSON proporcionado por AEM.

1. Vaya a la **Sitios** consola: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Seleccione el **Página principal de WKND SPA React** y haga clic en **Crear** > **Página**:

   ![Crear nueva página](assets/navigation-routing/create-new-page.png)

1. En **Plantilla** select **Página SPA**. En **Propiedades** enter **Página 1** para el **Título** y **page-1** como nombre.

   ![Introduzca las propiedades de la página inicial](assets/navigation-routing/initial-page-properties.png)

   Haga clic en **Crear** y en la ventana emergente de cuadro de diálogo, haga clic en **Apertura** para abrir la página en el AEM SPA Editor.

1. Agregar una nueva **Texto** al componente principal **Contenedor de diseño**. Edite el componente e introduzca el texto: **Página 1** uso de RTE y **H2** elemento.

   ![Página de contenido de muestra 1](assets/navigation-routing/page-1-sample-content.png)

   Siéntase libre de añadir contenido adicional, como una imagen.

1. Vuelva a la consola de AEM Sites y repita los pasos anteriores, creando una segunda página denominada **Página 2** como hermano de **Página 1**.
1. Finalmente cree una tercera página, **Página 3** pero como **secundario** de **Página 2**. Una vez completada la jerarquía del sitio, debería tener el siguiente aspecto:

   ![Jerarquía del sitio de muestra](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. El componente Navegación ahora se puede utilizar para desplazarse a diferentes áreas de la SPA.

   ![Navegación y enrutamiento](assets/navigation-routing/navigation-working.gif)

1. Abra la página fuera del Editor de AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). Utilice la variable **Navegación** para navegar a diferentes vistas de la aplicación.

1. Utilice las herramientas para desarrolladores del explorador para inspeccionar las solicitudes de red a medida que navega. Las capturas de pantalla siguientes se obtienen del explorador Google Chrome.

   ![Observar solicitudes de red](assets/navigation-routing/inspect-network-requests.png)

   Observe que después de la carga inicial de la página, la navegación posterior no provoca una actualización completa de la página y que el tráfico de red se minimiza al regresar a las páginas visitadas anteriormente.

## Modelo JSON de página de jerarquía {#hierarchy-page-json-model}

A continuación, revise el modelo JSON que impulsa la experiencia de visualización múltiple del SPA.

1. En una pestaña nueva, abra la API del modelo JSON proporcionada por AEM: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Puede resultar útil utilizar una extensión del explorador para [formatear el JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   Este contenido JSON se solicita cuando el SPA se carga por primera vez. La estructura exterior es similar a la siguiente:

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

   En `:children` debería ver una entrada para cada una de las páginas creadas. El contenido de todas las páginas se encuentra en esta solicitud JSON inicial. Con el enrutamiento de navegación, las vistas posteriores del SPA se cargan rápidamente, ya que el contenido ya está disponible en el lado del cliente.

   No es aconsejable cargar **ALL** del contenido de una SPA en la solicitud inicial JSON, ya que esto ralentizaría la carga inicial de la página. A continuación, veamos cómo se recopila la profundidad de jerarquía de las páginas.

1. Vaya a la **Raíz SPA** plantilla en: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Haga clic en el **Menú Propiedades de página** > **Política de página**:

   ![Abra la directiva de página de SPA raíz](assets/navigation-routing/open-page-policy.png)

1. La variable **Raíz SPA** la plantilla tiene un **Estructura jerárquica** para controlar el contenido JSON recopilado. La variable **Profundidad de la estructura** determina la profundidad en la jerarquía del sitio para recopilar las páginas secundarias debajo de la variable **root**. También puede usar la variable **Patrones de estructura** para filtrar páginas adicionales basadas en una expresión regular.

   Actualice el **Profundidad de la estructura** a **2**:

   ![Actualización de la profundidad de la estructura](assets/navigation-routing/update-structure-depth.png)

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

   Observe que la variable **Página 3** se ha eliminado la ruta de acceso: `/content/wknd-spa-react/us/en/home/page-2/page-3` del modelo JSON inicial. Esto se debe a que **Página 3** se encuentra en un nivel 3 de la jerarquía y hemos actualizado la directiva para incluir solo contenido con una profundidad máxima del nivel 2.

1. Vuelva a abrir la página principal de SPA: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) y abra las herramientas para desarrolladores del explorador.

   Actualice la página y debería ver la solicitud XHR para `/content/wknd-spa-react/us/en.model.json`, que es la raíz SPA. Tenga en cuenta que solo se incluyen tres páginas secundarias en función de la configuración de profundidad de jerarquía de la plantilla raíz de SPA realizada anteriormente en el tutorial. Esto no incluye **Página 3**.

   ![Solicitud inicial de JSON: SPA raíz](assets/navigation-routing/initial-json-request.png)

1. Con las herramientas para desarrolladores abiertas, utilice el `Navigation` para navegar directamente a **Página 3**:

   Tenga en cuenta que se realiza una nueva solicitud XHR a: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Página tres Solicitud XHR](assets/navigation-routing/page-3-xhr-request.png)

   El Administrador de modelos de AEM entiende que la variable **Página 3** El contenido de JSON no está disponible y déclencheur automáticamente la solicitud XHR adicional.

1. Experimente con vínculos profundos navegando directamente a: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). También observe que el botón de retroceso del navegador sigue funcionando.

## Enrutamiento de Inspect React  {#react-routing}

La navegación y el enrutamiento se implementan con [Enrutador React](https://reactrouter.com/). React Router es una colección de componentes de navegación para aplicaciones React. [Componentes principales de AEM React](https://github.com/adobe/aem-react-core-wcm-components-base) utiliza características de React Router para implementar el **Navegación** componente utilizado en los pasos anteriores.

A continuación, inspeccione cómo React Router está integrado con el SPA y experimente usando React Router [Vínculo](https://reactrouter.com/web/api/Link) componente.

1. En el IDE, abra el archivo `index.js` at `ui.frontend/src/index.js`.

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

   Observe que la variable `App` está dentro de la variable `Router` componente desde [Enrutador React](https://reacttraining.com/react-router/). La variable `ModelManager`, proporcionado por el SDK de JS AEM Editor SPA, agrega las rutas dinámicas a AEM páginas en función de la API del modelo JSON.

1. Abra el archivo . `Page.js` at `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   La variable `Page` SPA componente utiliza la variable `MapTo` función a asignar **Páginas** en AEM a un componente SPA correspondiente. La variable `withRoute` ayuda a enrutar dinámicamente el SPA a la página secundaria AEM adecuada en función del `cqPath` propiedad.

1. Abra el `Header.js` componente en `ui.frontend/src/components/Header/Header.js`.
1. Actualice el `Header` para ajustar el `<h1>` en un [Vínculo](https://reactrouter.com/web/api/Link) a la página principal:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   En lugar de usar un valor predeterminado `<a>` etiqueta de anclaje que usamos `<Link>` proporcionado por React Router. Mientras la cookie `to=` señala a una ruta válida, el SPA cambiará a esa ruta y **not** actualice la página completa. Aquí simplemente codificamos el vínculo a la página principal para ilustrar el uso de `Link`.

1. Actualice la prueba en `App.test.js` at `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   Dado que utilizamos características de React Router dentro de un componente estático al que se hace referencia en `App.js` necesitamos actualizar la prueba unitaria para contabilizarla.

1. Abra un terminal, vaya a la raíz del proyecto e implemente el proyecto para AEM con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Vaya a una de las páginas de la SPA en AEM: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   En lugar de usar la variable `Navigation` para navegar, utilice el vínculo de la sección `Header`.

   ![Vínculo de encabezado](assets/navigation-routing/header-link.png)

   Observe que se actualiza la página completa **not** y que el enrutamiento de SPA funciona.

1. Opcionalmente, experimente con el `Header.js` usar un `<a>` etiqueta de anclaje:

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   Esto puede ayudar a ilustrar la diferencia entre SPA enrutamiento y los vínculos de páginas web normales.

## Felicitaciones! {#congratulations}

Felicidades, ha aprendido cómo se pueden admitir varias vistas en el SPA asignando a AEM páginas con el SDK del Editor de SPA. La navegación dinámica se ha implementado utilizando el enrutador React y se ha agregado a la variable `Header` componente.

---
title: Agregar navegación y enrutamiento AEM SPA | Introducción al Editor de y React
description: SPA AEM SPA Descubra cómo se pueden admitir varias vistas en la asignando a páginas de la página con el SDK de Editor de la página de la página de la página de la aplicación de la versión de la aplicación. La navegación dinámica se implementa mediante los componentes principales React y React Router.
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
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# Agregar navegación y enrutamiento {#navigation-routing}

SPA AEM SPA Descubra cómo se pueden admitir varias vistas en la asignando a páginas de la página con el SDK de Editor de la página de la página de la página de la aplicación de la versión de la aplicación. La navegación dinámica se implementa mediante los componentes principales React y React Router.

## Objetivo

1. SPA SPA Comprender las opciones de enrutamiento del modelo de disponibles al utilizar el Editor de rutas.
1. Aprenda a utilizar [Enrutador React](https://reacttraining.com/react-router/) SPA para desplazarse entre las distintas vistas de la.
1. AEM AEM Utilice los componentes principales de React para implementar una navegación dinámica que se base en la jerarquía de páginas de la página de la aplicación de la.

## Qué va a generar

SPA AEM Este capítulo añadirá navegación a un recurso de la. AEM El menú de navegación está gobernado por la jerarquía de páginas de la y utilizará el modelo JSON proporcionado por el [Componente principal de navegación](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navegación añadida](assets/navigation-routing/navigation-added.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment). Este capítulo es una continuación de la [Asignar componentes](map-components.md) SPA AEM AEM , sin embargo, para seguir todo lo que necesita es un proyecto de habilitado para el uso de la implementado en una instancia de local de.

## Añadir la navegación a la plantilla {#add-navigation-template}

1. AEM Abra un explorador e inicie sesión para iniciar sesión en [http://localhost:4502/](http://localhost:4502/). La base del código de inicio ya debería implementarse.
1. Vaya a **SPA Plantilla de página**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Seleccione el más exterior **Contenedor de diseño raíz** y haga clic en **Política** icono. Ten cuidado **no** para seleccionar **Contenedor de diseño** desbloqueado para la creación.

   ![Seleccione el icono de directiva del contenedor de diseño raíz](assets/navigation-routing/root-layout-container-policy.png)

1. Cree una nueva directiva denominada **SPA Estructura de**:

   ![SPA Política de estructura](assets/navigation-routing/spa-policy-update.png)

   En **Componentes permitidos** > **General** > seleccione el **Contenedor de diseño** componente.

   En **Componentes permitidos** > **SPA REACCIÓN DE WKND: ESTRUCTURA** > seleccione el **Navegación** componente:

   ![Seleccionar componente de navegación](assets/navigation-routing/select-navigation-component.png)

   En **Componentes permitidos** > **SPA REACCIÓN DE WKND: contenido** > seleccione el **Imagen** y **Texto** componentes. Debe tener un total de 4 componentes seleccionados.

   Haga clic en **Listo** para guardar los cambios.

1. Actualice la página y añada el **Navegación** componente sobre el desbloqueado **Contenedor de diseño**:

   ![añadir componente de navegación a la plantilla](assets/navigation-routing/add-navigation-component.png)

1. Seleccione el **Navegación** y haga clic en su **Política** para editar la directiva.
1. Crear una nueva directiva con un **Título de política** de **SPA Navegación de**.

   En el **Propiedades**:

   * Configure las variables **Raíz de navegación** hasta `/content/wknd-spa-react/us/en`.
   * Configure las variables **Excluir niveles de raíz** hasta **1**.
   * Desmarcar **Recopilar todas las páginas secundarias**.
   * Configure las variables **Profundidad de estructura de navegación** hasta **3**.

   ![Configurar directiva de navegación](assets/navigation-routing/navigation-policy.png)

   Se recopilará la navegación a 2 niveles de profundidad por debajo `/content/wknd-spa-react/us/en`.

1. Después de guardar los cambios, debería ver los campos rellenados `Navigation` como parte de la plantilla:

   ![Componente de navegación rellenado](assets/navigation-routing/populated-navigation.png)

## Crear páginas secundarias

AEM SPA A continuación, cree páginas adicionales en las que se puedan usar las vistas diferentes de las vistas de la. AEM También inspeccionaremos la estructura jerárquica del modelo JSON proporcionado por los usuarios de la plataforma de datos de la plataforma de datos de.

1. Vaya a **Sites** consola: [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Seleccione el **SPA Página de inicio de WKND React** y haga clic en **Crear** > **Página**:

   ![Crear nueva página](assets/navigation-routing/create-new-page.png)

1. En **Plantilla** select **SPA Página de**. En **Propiedades** introduzcan **Página 1** para el **Título** y **page-1** como el nombre.

   ![Introduzca las propiedades de la página inicial](assets/navigation-routing/initial-page-properties.png)

   Clic **Crear** y en el cuadro de diálogo emergente, haga clic en **Abrir** AEM SPA para abrir la página en el Editor de.

1. Añadir un nuevo **Texto** al componente principal **Contenedor de diseño**. Edite el componente e introduzca el texto: **Página 1** uso del RTE y el **H2** Elemento.

   ![Página de contenido de muestra 1](assets/navigation-routing/page-1-sample-content.png)

   No dude en añadir contenido adicional, como una imagen.

1. Vuelva a la consola de AEM Sites y repita los pasos anteriores, creando una segunda página denominada **Página 2** como hermano de **Página 1**.
1. Por último, cree una tercera página, **Página 3** pero como **niño** de **Página 2**. Una vez completada, la jerarquía del sitio debe tener el siguiente aspecto:

   ![Jerarquía del sitio de muestra](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. SPA Ahora, el componente Navegación se puede utilizar para desplazarse a diferentes áreas de la.

   ![Navegación y enrutamiento](assets/navigation-routing/navigation-working.gif)

1. AEM Abra la página fuera del Editor de: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). Utilice el **Navegación** para desplazarse a diferentes vistas de la aplicación.

1. Utilice las herramientas para desarrolladores del explorador para inspeccionar las solicitudes de red a medida que navega. Las capturas de pantalla a continuación se capturan desde el navegador Google Chrome.

   ![Observar solicitudes de red](assets/navigation-routing/inspect-network-requests.png)

   Observe que después de la carga inicial de la página, la navegación posterior no provoca una actualización de la página completa y que el tráfico de red se minimiza al volver a páginas visitadas anteriormente.

## Modelo JSON de página de jerarquía {#hierarchy-page-json-model}

SPA A continuación, inspeccione el modelo JSON que impulsa la experiencia de vista múltiple de la.

1. AEM En una pestaña nueva, abra la API del modelo JSON proporcionada por: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Puede resultar útil utilizar una extensión de explorador para lo siguiente [formatear el JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   SPA Este contenido JSON se solicita cuando se carga la por primera vez. La estructura exterior tiene el siguiente aspecto:

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

   En `:children` debería ver una entrada para cada una de las páginas creadas. El contenido de todas las páginas se encuentra en esta solicitud JSON inicial. SPA Con el enrutamiento de navegación, las vistas subsiguientes de la se cargan rápidamente, ya que el contenido ya está disponible en el lado del cliente.

   No es aconsejable cargar **TODO** SPA del contenido de un elemento de la solicitud de JSON inicial, ya que esto ralentizaría la carga inicial de la página. A continuación, veamos cómo se recopila la profundidad de jerarquía de las páginas.

1. Vaya a **SPA Raíz** plantilla en: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Haga clic en **Menú Propiedades de página** > **Política de página**:

   ![SPA Abrir la directiva de página para la raíz de la](assets/navigation-routing/open-page-policy.png)

1. El **SPA Raíz** la plantilla tiene un **Estructura jerárquica** para controlar el contenido JSON recopilado. El **Profundidad de estructura** determina la profundidad en la jerarquía del sitio para recopilar páginas secundarias debajo del **raíz**. También puede utilizar la variable **Patrones de estructura** para filtrar páginas adicionales basadas en una expresión regular.

   Actualice el **Profundidad de estructura** hasta **2**:

   ![Actualizar profundidad de estructura](assets/navigation-routing/update-structure-depth.png)

   Clic **Listo** para guardar los cambios en la directiva.

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

   Observe que la variable **Página 3** se ha eliminado la ruta: `/content/wknd-spa-react/us/en/home/page-2/page-3` del modelo JSON inicial. Esto se debe a **Página 3** se encuentra en un nivel 3 en la jerarquía y hemos actualizado la directiva para incluir solo contenido con una profundidad máxima de nivel 2.

1. SPA Vuelva a abrir la página principal de la: [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) y abra las herramientas para desarrolladores del explorador.

   Actualice la página y debería ver la solicitud XHR de `/content/wknd-spa-react/us/en.model.json`SPA , que es la Raíz de la. SPA Tenga en cuenta que solo se incluyen tres páginas secundarias en función de la configuración de profundidad de jerarquía a la plantilla Raíz de realizada anteriormente en el tutorial. Esto no incluye **Página 3**.

   ![SPA Solicitud JSON inicial: raíz de la](assets/navigation-routing/initial-json-request.png)

1. Con las herramientas para desarrolladores abiertas, utilice el `Navigation` componente para navegar directamente a **Página 3**:

   Observe que se realiza una nueva solicitud XHR para: `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Página tres Solicitud XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM El Administrador de modelos de datos entiende que la variable **Página 3** El contenido JSON no está disponible y almacena en déclencheur automáticamente la solicitud XHR adicional.

1. Experimente con vínculos profundos navegando directamente a: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Observe también que el botón Atrás del explorador sigue funcionando.

## Enrutamiento React de Inspect  {#react-routing}

La navegación y el enrutamiento se implementan con [Enrutador React](https://reactrouter.com/). React Router es una colección de componentes de navegación para aplicaciones React. [AEM Componentes principales de React](https://github.com/adobe/aem-react-core-wcm-components-base) utiliza funciones de React Router para implementar el **Navegación** componente utilizado en los pasos anteriores.

SPA A continuación, inspeccione cómo React Router está integrado con la aplicación y experimente con la aplicación de React Routers en la interfaz de usuario de React Router. [Vínculo](https://reactrouter.com/web/api/Link) componente.

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

   Observe que la variable `App` está envuelto en la `Router` componente de [Enrutador React](https://reacttraining.com/react-router/). El `ModelManager`AEM SPA AEM , proporcionado por el SDK de JS del Editor de, agrega las rutas dinámicas a las páginas de la página de datos de la base de datos de la API del modelo JSON, que se basa en la API de modelo JSON.

1. Abra el archivo `Page.js` en `ui.frontend/src/components/Page/Page.js`

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

   El `Page` SPA El componente utiliza la variable `MapTo` función para asignar **Páginas** AEM SPA en el caso de un componente de la correspondiente. El `withRoute` SPA AEM ayuda a enrutar dinámicamente el recurso a la página secundaria de la aplicación correspondiente en función de la variable de la página de la aplicación de la que se ha obtenido el resultado `cqPath` propiedad.

1. Abra el `Header.js` componente en `ui.frontend/src/components/Header/Header.js`.
1. Actualice el `Header` para ajustar el `<h1>` etiqueta en una [Vínculo](https://reactrouter.com/web/api/Link) a la página principal:

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

   En lugar de usar un valor predeterminado `<a>` etiqueta de anclaje que utilizamos `<Link>` proporcionado por React Router. Siempre que la variable `to=` SPA apunta a una ruta válida, el cambiará a esa ruta y **no** realizar una actualización de página completa. Aquí simplemente codificamos el vínculo a la página principal para ilustrar el uso de `Link`.

1. Actualice la prueba en `App.test.js` en `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   Dado que estamos utilizando funciones de React Router dentro de un componente estático al que se hace referencia en `App.js` necesitamos actualizar la prueba unitaria para tener en cuenta esto.

1. AEM Abra un terminal, vaya a la raíz del proyecto e implemente el proyecto para que utilice sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. SPA AEM Navegue hasta una de las páginas de la en la que se encuentra: [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   En lugar de usar la variable `Navigation` para navegar por el componente, utilice el vínculo en la `Header`.

   ![Vínculo de encabezado](assets/navigation-routing/header-link.png)

   Observe que se actualiza la página completa **no** SPA se activa y que el enrutamiento de la funciona.

1. De forma opcional, experimente con `Header.js` archivo con un estándar `<a>` etiqueta de anclaje:

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   SPA Esto puede ayudar a ilustrar la diferencia entre el enrutamiento de la y los vínculos de página web normales.

## Enhorabuena. {#congratulations}

SPA AEM SPA ¡Enhorabuena! Ha aprendido cómo se pueden admitir varias vistas en la asignando a páginas de la página con el SDK del editor de páginas (SDK) de la aplicación de la aplicación de la aplicación de la manera de. La navegación dinámica se ha implementado utilizando React Router y se ha añadido a `Header` componente.

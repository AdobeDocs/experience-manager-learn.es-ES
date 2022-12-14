---
title: Registro de la extensión de la consola de fragmentos de contenido de AEM
description: Obtenga información sobre cómo registrar extensiones de consola de fragmentos de contenido.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---


# Registro de extensiones

AEM las extensiones de la consola de fragmentos de contenido son aplicaciones especializadas de App Builder, basadas en React, y utilizan la función [Espectro React](https://react-spectrum.adobe.com/react-spectrum/) Marco de IU.

Para definir dónde y cómo aparece la consola de fragmento de contenido de AEM de la extensión, se requieren dos configuraciones específicas en la aplicación de App Builder de la extensión: enrutamiento de aplicación y registro de extensión.

## Rutas de la aplicación{#app-routes}

La extensión de `App.js` declara que la variable [React router](https://reactrouter.com/en/main) que incluye una ruta de índice que registra la extensión en la consola de fragmentos de contenido de AEM.

La ruta de índice se invoca cuando, AEM consola de fragmento de contenido se carga inicialmente y el destino de esta ruta define cómo se expone la extensión en la consola.

+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

```javascript
import ExtensionRegistration from "./ExtensionRegistration"
...            
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          {/* The index route maps to the extension registration */}
          <Route index element={<ExtensionRegistration />} />
          ...                                   
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registro de extensiones

`ExtensionRegistration.js` debe cargarse inmediatamente a través de la ruta de índice de la extensión y actúa como punto de registro de la extensión, definiendo:

1. El tipo de extensión; a [menú encabezado](./header-menu.md) o [barra de acciones](./action-bar.md) botón.
   + [Menú Encabezado](./header-menu.md#extension-registration) las extensiones se identifican mediante la variable `headerMenu` propiedad under `methods`.
   + [Barra de acciones](./action-bar.md#extension-registration) las extensiones se identifican mediante la variable `actionBar` propiedad under `methods`.
1. La definición del botón de extensión, en `getButton()` función. Esta función devuelve un objeto con campos:
   + `id` es un ID único para el botón
   + `label` es la etiqueta del botón de extensión en la consola Fragmento de contenido de AEM
   + `icon` es el icono del botón de extensión en la consola Fragmento de contenido de AEM. El icono es un [Espectro React](https://spectrum.adobe.com/page/icons/) nombre del icono, con espacios eliminados.
1. El controlador de clic del botón, en se define en una `onClick()` función.
   + [Menú Encabezado](./header-menu.md#extension-registration) las extensiones no pasan parámetros al controlador de clic.
   + [Barra de acciones](./action-bar.md#extension-registration) las extensiones proporcionan una lista de las rutas de fragmento de contenido seleccionadas en la `selections` parámetro.

### Extensión del menú Encabezado

![Extensión del menú Encabezado](./assets/extension-registration/header-menu.png)

Los botones de extensión del menú de encabezado se muestran cuando no se selecciona ningún fragmento de contenido. Debido a que las extensiones del menú de encabezado no actúan sobre una selección de fragmentos de contenido, no se proporcionan fragmentos de contenido a su `onClick()` controlador.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension', // Unique ID for the button
              'label': 'My header menu extension',      // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Header Menu extensions do not pass parameters to the click handler
          onClick() { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Omitir para crear una extensión de menú de encabezado</p>
      <p class="has-text-blackest">Obtenga información sobre cómo registrar y definir una extensión de menú de encabezado en la consola Fragmentos de contenido de AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./header-menu.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Aprenda a crear una extensión de menú de encabezado">Aprenda a crear una extensión de menú de encabezado</span>
        </a>
      </div>
    </div>
  </div>
</div>

### Extensión de la barra de acciones

![Extensión de la barra de acciones](./assets/extension-registration/action-bar.png)

Los botones de extensión de la barra de acciones se muestran cuando se seleccionan uno o más fragmentos de contenido. Las rutas del fragmento de contenido seleccionado están disponibles para la extensión mediante la variable `selections` , en el botón `onClick(..)` controlador.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-extension',  // Unique ID for the button
              'label': 'My action bar extension',       // Button label 
              'icon': 'Edit'                            // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          // Only Action Bar buttons populate the selections parameter
          onClick(selections) { ... }
        }
      }
    })
  }
  init().catch(console.error)
}
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Omitir para crear una extensión de barra de acciones</p>
      <p class="has-text-blackest">Obtenga información sobre cómo registrar y definir una extensión de barra de acciones en la consola Fragmentos de contenido de AEM.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./action-bar.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Aprenda a crear una extensión de barra de acciones">Aprenda a crear una extensión de barra de acciones</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Incluir extensiones condicionalmente

AEM extensiones de la consola Fragmento de contenido pueden ejecutar lógica personalizada para limitar el momento en que la extensión aparece en la consola Fragmento de contenido de AEM. Esta comprobación se realiza antes de que `register` en la variable `ExtensionRegistration` y devuelve inmediatamente si no se debe mostrar la extensión.

Esta comprobación tiene contexto limitado disponible:

+ El host de AEM en el que se carga la extensión.
+ El token de acceso AEM del usuario actual.

Las comprobaciones más comunes para cargar una extensión son:

+ Uso del host de AEM (`new URLSearchParams(window.location.search).get('repo')`) para determinar si la extensión debe cargarse.
   + Mostrar solo la extensión en entornos AEM que formen parte de un programa específico (como se muestra en el ejemplo siguiente).
   + Mostrar solo la extensión en un entorno de AEM específico (es decir, AEM host).
+ Uso de un [Acción de Adobe I/O Runtime](./runtime-action.md) para realizar una llamada HTTP a AEM para determinar si el usuario actual debe ver la extensión.

El ejemplo siguiente ilustra la limitación de la extensión a todos los entornos del programa `p12345`.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const PROGRAM_ID = 'p12345';

  // Get the current AEM Host (author-pXXX-eYYY.adobeaemcloud.com) the extension is loading on
  const aemHost = new URLSearchParams(window.location.search).get('repo');

  // Create a check to determine if the current AEM host matches the AEM program that uses this extension 
  const aemHostRegex = new RegExp(`^author-${PROGRAM_ID}-e[\\d]+\\.adobeaemcloud\\.com$`)

  // Disable the extension if the Cloud Manager Program Id doesn't match the regex.
  if (!aemHostRegex.test(aemHost)) {
    return; // Skip extension registration if the environment is not in program p12345.
  }

  // Else, continue initializing the extension
  const init = async () => { .. };
  
  init().catch(console.error);
}
```

---
title: AEM Registro de extensión de IU
description: AEM Obtenga información sobre cómo registrar una extensión de IU de.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: ef2290d9-ba40-429b-b10d-e82d6c1c20f6
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Registro de extensiones

AEM Las extensiones de interfaz de usuario de son aplicaciones especializadas de App Builder, basadas en React y que utilizan el marco de interfaz de usuario [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/).

AEM Para definir dónde y cómo aparece la extensión de la interfaz de usuario de, se requieren dos configuraciones en la aplicación de App Builder de la extensión: enrutamiento de aplicaciones y registro de la extensión.

## Rutas de aplicaciones{#app-routes}

AEM `App.js` de la extensión declara el [enrutador React](https://reactrouter.com/en/main) que incluye una ruta de índice que registra la extensión en la interfaz de usuario de la interfaz de usuario de la.

AEM La ruta de índice se invoca cuando se carga inicialmente la interfaz de usuario de y el destino de esta ruta define cómo se expone la extensión en la consola.

+ `./src/aem-ui-extension/web-src/src/components/App.js`

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

`ExtensionRegistration.js` debe cargarse inmediatamente a través de la ruta de índice de la extensión y actúa como punto de registro de la extensión.

AEM En función de la plantilla de extensión de la interfaz de usuario de la aplicación seleccionada al [inicializar la extensión de la aplicación de App Builder](./app-initialization.md), se admiten diferentes puntos de extensión.

+ [Puntos de extensión de IU de fragmentos de contenido](./content-fragments/overview.md#extension-points)

## Incluir extensiones condicionalmente

AEM AEM Las extensiones de interfaz de usuario de pueden ejecutar lógica personalizada para limitar los entornos de en los que aparece la extensión. Esta comprobación se realiza antes de la llamada `register` en el componente `ExtensionRegistration` y devuelve inmediatamente si no se debe mostrar la extensión.

Esta comprobación tiene un contexto limitado disponible:

+ AEM Host en el que se está cargando la extensión.
+ AEM El token de acceso de la cuenta de acceso de la cuenta de usuario actual.

Las comprobaciones más comunes para cargar una extensión son:

+ AEM Usando el host de la (`new URLSearchParams(window.location.search).get('repo')`) para determinar si la extensión debe cargarse.
   + AEM Muestre solo la extensión en entornos de que formen parte de un programa específico (como se muestra en el ejemplo siguiente).
   + AEM AEM Mostrar solo la extensión en un entorno de específico (host de).
+ Se está usando una [acción de Adobe I/O Runtime AEM](./runtime-action.md) para realizar una llamada HTTP a la red para determinar si el usuario actual debe ver la extensión.

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

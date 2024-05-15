---
title: AEM Modo de extensión de IU
description: AEM Obtenga información sobre cómo crear una extensión de interfaz de usuario de modal.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Modo de extensión

![AEM Modo de extensión de IU](./assets/modal/modal.png){align="center"}

AEM AEM El modal de extensión de la interfaz de usuario de proporciona una forma de adjuntar una interfaz de usuario personalizada a las extensiones de la interfaz de usuario.

Los modelos son aplicaciones de React basadas en [Espectro de reacción](https://react-spectrum.adobe.com/react-spectrum/)y pueden crear cualquier interfaz de usuario personalizada que requiera la extensión, incluidas, entre otras:

+ Cuadros de diálogo de confirmación
+ [Formularios de entrada](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Indicadores de progreso](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Resumen de resultados](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Mensajes de error
+ ... o incluso una aplicación React de múltiples vistas!

## Rutas modales

La experiencia modal se define mediante la extensión de la aplicación React del Generador de aplicaciones definida en `web-src` carpeta. Al igual que con cualquier aplicación de React, la experiencia completa se organiza mediante [Reaccionar rutas](https://reactrouter.com/en/main/components/routes) que procesan [Reaccionar componentes](https://reactjs.org/docs/components-and-props.html).

Se requiere al menos una ruta para generar la vista modal inicial. Esta ruta inicial se invoca en la variable [registro de extensión](#extension-registration)de `onClick(..)` función, como se muestra a continuación.


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registro de extensiones

Para abrir un modal, llame a `guestConnection.host.modal.showUrl(..)` se realiza desde el `onClick(..)` función. `showUrl(..)` se pasa a un objeto JavaScript con clave/valores:

+ `title` proporciona el nombre del título del modal mostrado al usuario
+ `url` es la dirección URL que invoca el [Ruta de reacción](#modal-routes) responsable de la vista inicial del modal.

Es imperativo que la `url` pasado a `guestConnection.host.modal.showUrl(..)` resuelve en la ruta en la extensión; de lo contrario, no aparece nada en el modal.

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## Componente modal

Cada ruta de la extensión, [ese no es el `index` ruta](./extension-registration.md#app-routes), se asigna a un componente de React que se puede procesar en el modal de la extensión.

Un modal puede estar compuesto por cualquier número de rutas de React, desde un modal simple de una ruta a un modal complejo de varias rutas.

A continuación se ilustra un modal simple de una ruta, pero esta vista modal podría contener vínculos de React que invoquen otras rutas o comportamientos.

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## Cerrar el modal

![AEM Botón Cerrar modal de la extensión de IU](./assets/modal/close.png){align="center"}

Los modelos deben proporcionar su propio control estricto. Esto se realiza invocando `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```

---
title: Modo de extensión de la consola de fragmentos de contenido de AEM
description: Obtenga información sobre cómo crear un modal de extensión de la consola de fragmentos de contenido AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# Modo de extensión

![AEM modal de extensión de fragmento de contenido](./assets/modal/modal.png){align="center"}

AEM modal de extensión de fragmento de contenido proporciona una forma de adjuntar una interfaz de usuario personalizada a las extensiones de fragmento de contenido de AEM, ya sea [Barra de acciones](./action-bar.md) o [Menú Encabezado](./header-menu.md) botones.

Los modelos son aplicaciones React, basadas en [Espectro React](https://react-spectrum.adobe.com/react-spectrum/), y pueden crear cualquier IU personalizada requerida por la extensión, que incluya, entre otras cosas:

+ Diálogo de confirmación
+ [Formularios de entrada](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Indicadores de progreso](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Resumen de resultados](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Mensajes de error
+ ... o incluso una aplicación de React completamente volada y multivista!

## Rutas móviles

La experiencia modal se define mediante la extensión de la aplicación App Builder React definida en la sección `web-src` carpeta. Al igual que con cualquier aplicación React, la experiencia completa se organiza mediante [Reacción de rutas](https://reactrouter.com/en/main/components/routes) que procesa [Reacción de componentes](https://reactjs.org/docs/components-and-props.html).

Se requiere al menos una ruta para generar la vista modal inicial. Esta ruta inicial se invoca en la variable [registro de extensión](#extension-registration)&#39;s `onClick(..)` , como se muestra a continuación.


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
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

Para abrir un modal, llame a `guestConnection.host.modal.showUrl(..)` se realiza desde la extensión de `onClick(..)` función. `showUrl(..)` se pasa un objeto JavaScript con claves o valores:

+ `title` proporciona al usuario el nombre del título del modal
+ `url` es la dirección URL que invoca la variable [React route](#modal-routes) responsable de la vista inicial del modal.

Es imperativo que `url` pasado a `guestConnection.host.modal.showUrl(..)` resuelve enrutar en la extensión de , de lo contrario, no se muestra nada en el modal.

Consulte la [menú encabezado](./header-menu.md#modal) y [barra de acciones](./action-bar.md#modal) documentación sobre cómo crear direcciones URL modales.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

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

Cada ruta de la extensión, [que no es el `index` route](./extension-registration.md#app-routes), se asigna a un componente React que se puede procesar en el modal de la extensión.

Un modal puede estar compuesto por cualquier número de rutas React, desde un modo simple de una ruta hasta un modal complejo de varias rutas.

A continuación se muestra un modo sencillo de una ruta, aunque esta vista modal podría contener vínculos React que invoquen otras rutas o comportamientos.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

## Cierre el modal

![AEM botón de cierre modal de la extensión del fragmento de contenido](./assets/modal/close.png){align="center"}

Los modelos deben proporcionar su propio control. Esto se hace invocando `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```

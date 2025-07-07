---
title: 'Consola de fragmento de contenido: campos personalizados'
description: Obtenga información sobre cómo crear un campo personalizado en el Editor de fragmentos de contenido de AEM.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 315
last-substantial-update: 2024-02-27T00:00:00Z
jira: KT-14903
thumbnail: KT-14903.jpeg
exl-id: 563bab0e-21e3-487c-9bf3-de15c3a81aba
source-git-commit: be710750cd6f2c71fa6b5eb8a6309d5965d67c82
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 1%

---

# Campos personalizados

Aprenda a crear campos personalizados en el Editor de fragmentos de contenido de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3427585?learn=on)

Las extensiones de la interfaz de usuario de AEM deben desarrollarse con el marco de trabajo [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html), ya que mantiene una apariencia coherente con el resto de AEM y también tiene una amplia biblioteca de funcionalidades prediseñadas, lo que reduce el tiempo de desarrollo.

## Punto de extensión

Este ejemplo reemplaza un campo existente en el Editor de fragmentos de contenido con una implementación personalizada.

| IU de AEM extendida | Punto de extensión |
| ------------------------ | --------------------- | 
| [Editor de fragmentos de contenido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Procesamiento de elementos de formulario personalizado](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/) |

## Extensión de ejemplo

En este ejemplo se muestra la restricción de los valores de campo en el Editor de fragmentos de contenido a un conjunto predeterminado mediante la sustitución del campo estándar por un menú desplegable personalizado de SKU predefinidas. Los autores pueden seleccionar de esta lista de SKU específica. Aunque los SKU suelen proceder de un sistema de gestión de la información del producto (PIM), este ejemplo simplifica la lista estática de los SKU.

El código fuente de este ejemplo es [disponible para descargar](./assets/editor-custom-field/content-fragment-editor-custom-field-src.zip).

### Definición del modelo de fragmento de contenido

Este ejemplo se enlaza a cualquier campo de fragmento de contenido cuyo nombre sea `sku` (a través de una [coincidencia de expresión regular](#extension-registration) de `^sku$`) y lo reemplaza por un campo personalizado. El ejemplo utiliza el modelo de fragmento de contenido de WKND Adventure que se ha actualizado y la definición es la siguiente:

![Definición del modelo de fragmento de contenido](./assets/editor-custom-field/content-fragment-editor.png)

A pesar de que el campo SKU personalizado se muestra como una lista desplegable, su modelo subyacente se configura como un campo de texto. La implementación de campos personalizados solo necesita alinearse con el nombre y el tipo de propiedad adecuados, lo que facilita la sustitución del campo estándar por la versión desplegable personalizada.


### Rutas de aplicaciones

En el componente React principal `App.js`, incluya la ruta `/sku-field` para procesar el componente React `SkuField`.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
import React from "react";
import ErrorBoundary from "react-error-boundary";
import { HashRouter as Router, Routes, Route } from "react-router-dom";
import ExtensionRegistration from "./ExtensionRegistration";
import SkuField from "./SkuField"; // Custom component implemented below

function App() {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          <Route index element={<ExtensionRegistration />} />
          <Route
            exact path="index.html"
            element={<ExtensionRegistration />}
          />
          {/* This is the React route that maps to the custom field */}
          <Route
            exact path="/sku-field"
            element={<SkuField />}/>
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
...
```

Esta ruta personalizada de `/sku-field` se asigna al componente `SkuField` y se usa más abajo en [Registro de extensiones](#extension-registration).

### Registro de extensiones

`ExtensionRegistration.js`, asignado a la ruta index.html, es el punto de entrada para la extensión de AEM y define:

+ La definición del widget en la función `getDefinitions()` con los atributos `fieldNameExp` y `url`. La lista completa de atributos disponibles está disponible en la [Referencia de la API de procesamiento de elementos de formulario personalizados](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference).
+ Valor de atributo `url`, una ruta de acceso URL relativa (`/index.html#/skuField`) para cargar la interfaz de usuario del campo.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        field: {
          getDefinitions() {
            return [
              // Multiple fields can be registered here.
              {
                fieldNameExp: '^sku$',  // This is a regular expression that matches the field name in the Content Fragment Model to replace with React component specified in the `url` key.
                url: '/#/sku-field',    // The URL, which is mapped vai the Route in App.js, to the React component that will be used to render the field.
              },
              // Other bindings besides fieldNameExp, other bindings can be used as well as defined here:
              // https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference
            ];
          },
        },
      }
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Campo personalizado

El componente React `SkuField` actualiza el Editor de fragmentos de contenido con una interfaz de usuario personalizada, usando React Spectrum de Adobe para su formulario de selector. Los aspectos destacados incluyen:

+ Se está utilizando `useEffect` para la inicialización y conexión con el Editor de fragmentos de contenido de AEM, con un estado de carga que se muestra hasta que se completa la instalación.
+ Al procesarse dentro de un iFrame, ajusta dinámicamente el alto del iFrame mediante la función `onOpenChange` para dar cabida a la lista desplegable del Selector de espectro de React de Adobe.
+ Comunique las selecciones de campo de nuevo al host mediante `connection.host.field.onChange(value)` en la función `onSelectionChange`, asegurándose de que el valor seleccionado se valide y se guarde automáticamente según las directrices del modelo de fragmento de contenido.

Los campos personalizados se representan dentro de un iFrame insertado en el Editor de fragmentos de contenido. La comunicación entre el código de campo personalizado y el editor de fragmentos de contenido se establece exclusivamente a través del objeto `connection`, establecido por la función `attach` desde el paquete `@adobe/uix-guest`.

`src/aem-cf-editor-1/web-src/src/components/SkuField.js`

```javascript
import React, { useEffect, useState } from "react";
import { extensionId } from "./Constants";
import { attach } from "@adobe/uix-guest";
import { Provider, View, lightTheme } from "@adobe/react-spectrum";
import { Picker, Item } from "@adobe/react-spectrum";

const SkuField = (props) => {
  const [connection, setConnection] = useState(null);
  const [validationState, setValidationState] = useState(null);
  const [value, setValue] = useState(null);
  const [model, setModel] = useState(null);
  const [items, setItems] = useState(null);

  /**
   * Mock function that gets a list of Adventure SKUs to display.
   * The data could come anywhere, AEM's HTTP APIs, a PIM, or other system.
   * @returns a list of items for the picker
   */
  async function getItems() {
    // Data to drive input field validation can come from anywhere.
    // Fo example this commented code shows how it could be fetched from an HTTP API.
    // fetch(MY_API_URL).then((response) => response.json()).then((data) => { return data; }

    // In this example, for simplicity, we generate a list of 25 SKUs.
    return Array.from({ length: 25 }, (_, i) => ({ 
        name: `WKND-${String(i + 1).padStart(3, '0')}`, 
        id: `WKND-${String(i + 1).padStart(3, '0')}` 
    }));
  }

  /**
   * When the fields changes, update the value in the Content Fragment Editor
   * @param {*} value the selected value in the picker
   */
  const onSelectionChange = async (value) => {
    // This sets the value in the React state of the custom field
    setValue(value);
    // This calls the setValue method on the host (AEM's Content Fragment Editor)
    connection.host.field.onChange(value);
  };

  /**
   * Some widgets, like the Picker, have a variable height.
   * In these cases adjust the Content Fragment Editor's iframe's height so the field doesn't get cut off.     *
   * @param {*} isOpen true if the picker is open, false if it's closed
   */
  const onOpenChange = (isOpen) => {
    if (isOpen) {
      // Calculate the height of the picker box and its label, and surrounding padding.
      const pickerHeight = Number(document.body.clientHeight.toFixed(0));
      // Calculate the height of the picker options dropdown, and surrounding padding.
      // We do this  by multiplying the number of items by the height of each item (32px) and adding 12px for padding.
      const optionsHeight = items.length * 32 + 12;

      // Set the height of the iframe to the height of the picker + the height of the options, or 400px, whichever is smaller.
      // The options will scroll if they they cannot fit into 400px
      const height = Math.min(pickerHeight + optionsHeight, 400);

      // Set the height of the iframe in the Content Fragment Editor
      connection.host.field.setStyles({
        current: { height, },
        parent: { height, },
      })
    } else {
      connection.host.field.setStyles({
        current: { height: 74 },
        parent: { height: 74 },
      })
    }
  };

  useEffect(() => {
    const init = async () => {
      // Connect to the host (AEM's Content Fragment Editor)
      const conn = await attach({ id: extensionId });
      setConnection(conn);

      // get the Content Fragment Model
      setModel(await conn.host.field.getModel());

      // Share the validation state back to the client.
      // When conn.host.field.setValue(value) is called, the
      await conn.host.field.onValidationStateChange((state) => {
        // state can be `valid` or `invalid`.
        setValidationState(state);
      });
      // Get default value from the Content Fragment Editor
      // (either the default value set in the model, or a perviously set value)
      setValue(await conn.host.field.getDefaultValue());

      // Get the list of items for the picker; in this case its a list of adventure SKUs 
      // This could come from elsewhere in AEM or from an external system.
      setItems(await getItems());
    };

    init().catch(console.error);
  }, []);

  // If the component is not yet initialized, return a loading state.
  if (!connection || !model || !items) {
    // Put whatever loader you like here...
    return <Provider theme={lightTheme}>Loading custom field...</Provider>;
  }

  // Wrap the Spectrum UI component in a Provider theme, such that it is styled appropriately.
  // Render the picker, and bind to the data and custom event handlers.

  // Set as much of the model as we can, to allow maximum authoring flexibility without developer support.
  return (
    <Provider theme={lightTheme}>
      <View width="100%">
        <Picker
          label={model.fieldLabel}
          isRequired={model.required}
          placeholder={model.emptyText}
          errorMessage={model.customErrorMsg}
          selectedKey={value}
          necessityIndicator="icon"
          shouldFlip={false}
          width={"90%"}
          items={items}
          isInvalid={validationState === "invalid"}
          onSelectionChange={onSelectionChange}
          onOpenChange={onOpenChange}
        >
          {(item) => <Item key={item.value}>{item.name}</Item>}
        </Picker>
      </View>
    </Provider>
  );
};

export default SkuField;
```

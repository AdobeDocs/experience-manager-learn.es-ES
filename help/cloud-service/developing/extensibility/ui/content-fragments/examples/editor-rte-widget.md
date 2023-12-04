---
title: Añadir widgets al editor de texto enriquecido (RTE)
description: AEM Aprenda a añadir widgets al editor de texto enriquecido (RTE) en el editor de fragmentos de contenido de la
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 572
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Añadir widgets al editor de texto enriquecido (RTE)

AEM Aprenda a añadir widgets al editor de texto enriquecido (RTE) en el editor de fragmentos de contenido de la aplicación de la aplicación de contenido de la.

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

Para añadir el contenido dinámico en el Editor de texto enriquecido (RTE), la variable **widgets** se puede utilizar. Los widgets ayudan a integrar la IU simple o compleja en RTE y la IU se puede crear utilizando el marco de trabajo JS de su elección. Se pueden pensar como diálogos que se abren presionando `{` clave especial en el RTE.

Normalmente, los widgets se utilizan para insertar el contenido dinámico que tiene una dependencia externa del sistema o que podría cambiar según el contexto actual.

El **widgets** se añaden a **RTE** en el Editor de fragmentos de contenido mediante `rte` punto de extensión. Uso de `rte` punto de extensión `getWidgets()` método se añaden uno o varios widgets. Se activan pulsando la tecla `{` clave especial para abrir la opción menú contextual y, a continuación, seleccione el widget deseado para cargar la interfaz de usuario del cuadro de diálogo personalizado.

Este ejemplo muestra cómo añadir un widget llamado _Lista de códigos de descuento_ para buscar, seleccionar y añadir el código de descuento específico de la aventura WKND dentro de un contenido RTE. Estos códigos de descuento se pueden administrar en sistemas externos como Order Management System (OMS), Product Information Management (PIM), una aplicación propia o una acción de Adobe de AppBuilder.

Para que las cosas sean sencillas, este ejemplo utiliza el [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) marco de trabajo para desarrollar la interfaz de usuario del widget o cuadro de diálogo y el nombre de la aventura WKND predefinido, datos del código de descuento.

## Punto de extensión

Este ejemplo se extiende hasta el punto de extensión `rte` para agregar un widget al RTE en el Editor de fragmentos de contenido.

| AEM Interfaz de usuario extendida | Punto de extensión |
| ------------------------ | --------------------- | 
| [Editor de fragmentos de contenido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Widgets del editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Extensión de ejemplo

En el siguiente ejemplo se crea un _Lista de códigos de descuento_ widget. Pulsando la tecla `{` clave especial dentro de RTE, se abre el menú contextual y, a continuación, seleccionando la _Lista de códigos de descuento_ en el menú contextual se abre la interfaz de usuario del cuadro de diálogo.

Los autores de contenido de WKND pueden buscar, seleccionar y agregar el código de descuento actual específico de Adventure, si está disponible.

### Registro de extensiones

`ExtensionRegistration.js`AEM , asignado a la ruta index.html, es el punto de entrada para la extensión de la y define:

+ La definición del widget en `getWidgets()` función con `id, label and url` atributos.
+ El `url` valor de atributo, una ruta URL relativa (`/index.html#/discountCodes`) para cargar la IU de diálogo.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {

          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget",       // Provide a unique ID for the widget
              label: "Discount Code List",          // Provide a label for the widget
              url: "/index.html#/discountCodes",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Añadir `discountCodes` ruta en `App.js`{#add-widgets-route}

En el componente React principal `App.js`, añada el `discountCodes` para procesar la interfaz de usuario de la ruta URL relativa anterior.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### Crear `DiscountCodes` Componente React{#create-widget-react-component}

La interfaz de usuario del widget o cuadro de diálogo se crea con la variable [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) marco. El `DiscountCodes` el código de componente es el siguiente, y estos son los aspectos destacados:

+ La interfaz de usuario se procesa mediante componentes del espectro de React, como [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Botón](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ El `adventureDiscountCodes` La matriz tiene una asignación codificada del nombre de la aventura y el código de descuento. En situaciones reales, estos datos se pueden recuperar de la acción del AppBuilder de Adobe o de sistemas externos como PIM, OMS o la puerta de enlace de API casera o basada en el proveedor de la nube.
+ El `guestConnection` se inicializa usando el `useEffect` [Reaccionar gancho](https://react.dev/reference/react/useEffect) y se administra como estado de componente. AEM Se utiliza para comunicarse con el host de la.
+ El `handleDiscountCodeChange` función obtiene el código de descuento para el nombre de aventura seleccionado y actualiza la variable de estado.
+ El `addDiscountCode` función que utiliza `guestConnection` El objeto proporciona la instrucción RTE que se debe ejecutar. En este caso `insertContent` instrucción y fragmento de código de HTML del código de descuento real que se va a insertar en el RTE.

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```

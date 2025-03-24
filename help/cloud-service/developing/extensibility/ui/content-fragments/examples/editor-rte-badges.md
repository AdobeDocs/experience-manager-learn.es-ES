---
title: Añadir insignias al editor de texto enriquecido (RTE)
description: Aprenda a añadir insignias al Editor de texto enriquecido (RTE) en el Editor de fragmentos de contenido de AEM
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# Añadir insignias al editor de texto enriquecido (RTE)

Aprenda a añadir insignias al Editor de texto enriquecido (RTE) en el Editor de fragmentos de contenido de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[El distintivo del Editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) son extensiones que hacen que el texto del Editor de texto enriquecido (RTE) no se pueda editar. Esto significa que un distintivo declarado como tal solo se puede eliminar completamente y no se puede editar parcialmente. Estos distintivos también admiten colores especiales dentro de RTE, lo que indica claramente a los autores de contenido que el texto es un distintivo y, por lo tanto, no editable. Además, proporcionan indicaciones visuales sobre el significado del texto del distintivo.

El caso de uso más común de los distintivos RTE es usarlos junto con [widgets RTE](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). Esto permite que el contenido insertado en RTE por el widget RTE no se pueda editar.

Normalmente, las insignias asociadas con los widgets se utilizan para agregar el contenido dinámico que tiene una dependencia externa del sistema, pero _los autores de contenido no pueden modificar_ el contenido dinámico insertado para mantener la integridad. Solo se pueden eliminar como un elemento completo.

Las **insignias** se han agregado a **RTE** en el editor de fragmentos de contenido usando el punto de extensión `rte`. Con el método `getBadges()` del punto de extensión `rte` se agregan uno o varios distintivos.

Este ejemplo muestra cómo agregar un widget llamado _Servicio al cliente de reservas de grupos grandes_ para buscar, seleccionar y agregar los detalles del servicio al cliente específicos de la aventura de WKND como **Nombre del representante** y **Número de teléfono** dentro de un contenido RTE. Al usar la funcionalidad de distintivos, el **número de teléfono** pasa a ser **no editable**, pero los autores de contenido de WKND pueden editar el nombre del representante.

Además, el **número de teléfono** tiene un estilo diferente (azul), lo cual es un caso de uso adicional de la funcionalidad de distintivos.

Para simplificar las cosas, este ejemplo usa el módulo [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) para desarrollar la interfaz de usuario del widget o cuadro de diálogo y los números de teléfono del servicio de atención al cliente de WKND predefinidos. Para controlar la no edición y el aspecto de estilo diferente del contenido, se utiliza el carácter `#` en el atributo `prefix` y `suffix` de la definición de distintivos.

## Puntos de extensión

Este ejemplo se extiende al punto de extensión `rte` para agregar un distintivo al RTE en el editor de fragmentos de contenido.

| IU de AEM extendida | Puntos de extensión |
| ------------------------ | --------------------- | 
| [Editor de fragmentos de contenido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Insignias del editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) y [Widgets del editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Extensión de ejemplo

En el siguiente ejemplo se crea un widget de _Servicio al cliente de reservas de grupos grandes_. Al pulsar la tecla `{` dentro de RTE, se abre el menú contextual de widgets RTE. Al seleccionar la opción _Servicio al cliente de reservas de grupos grandes_ en el menú contextual, se abre el modal personalizado.

Una vez que el número de servicio de atención al cliente deseado se agrega desde el modal, los distintivos hacen que el _número de teléfono no se pueda editar_ y lo personalizan en color azul.

### Registro de extensiones

`ExtensionRegistration.js`, asignado a la ruta `index.html`, es el punto de entrada para la extensión de AEM y define:

+ La definición del distintivo se define en `getBadges()` mediante los atributos de configuración `id`, `prefix`, `suffix`, `backgroundColor` y `textColor`.
+ En este ejemplo, el carácter `#` se usa para definir los límites de este distintivo, lo que significa que cualquier cadena del RTE que esté rodeada por `#` se trata como una instancia de este distintivo.

Consulte también los detalles clave del widget RTE:

+ La definición del widget en la función `getWidgets()` con los atributos `id`, `label` y `url`.
+ El valor de atributo `url`, una ruta de acceso URL relativa (`/index.html#/largeBookingsCustomerService`) para cargar el modal.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Agregar ruta `largeBookingsCustomerService` en `App.js`{#add-widgets-route}

En el componente React principal `App.js`, agregue la ruta `largeBookingsCustomerService` para procesar la interfaz de usuario para la ruta de URL relativa anterior.

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### Crear componente de React `LargeBookingsCustomerService`{#create-widget-react-component}

La interfaz de usuario del widget o cuadro de diálogo se crea con el marco de trabajo [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html).

El código del componente React al agregar los detalles del servicio de atención al cliente, rodee la variable del número de teléfono con el carácter de distintivos registrados `#` para convertirla en distintivos, como `#${phoneNumber}#`, de modo que no se pueda editar.

Estos son los aspectos destacados del código `LargeBookingsCustomerService`:

+ La interfaz de usuario se representa mediante componentes del espectro de React, como [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ La matriz `largeGroupCustomerServiceList` tiene una asignación codificada del nombre del representante y el número de teléfono. En situaciones reales, estos datos se pueden recuperar de la acción del Adobe AppBuilder, de sistemas externos o de puertas de enlace de API caseras o basadas en proveedores en la nube.
+ El `guestConnection` se inicializó usando el `useEffect` [vínculo de React](https://react.dev/reference/react/useEffect) y se administró como estado de componente. Se utiliza para comunicarse con el host de AEM.
+ La función `handleCustomerServiceChange` obtiene un nombre representativo y un número de teléfono, y actualiza las variables de estado del componente.
+ La función `addCustomerServiceDetails` que utiliza el objeto `guestConnection` proporciona una instrucción RTE para ejecutar. En este caso, instrucción `insertContent` y fragmento de código HTML.
+ Para que el número de teléfono **no se pueda editar** con distintivos, se agrega el carácter especial `#` antes y después de la variable `phoneNumber`, como `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```

---
title: Añadir insignias al editor de texto enriquecido (RTE)
description: AEM Aprenda a añadir insignias al Editor de texto enriquecido (RTE) en el Editor de fragmentos de contenido de la
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: c965d5ff3f49f4859779e657674dab8602fb831b
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 0%

---


# Añadir insignias al editor de texto enriquecido (RTE)

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[Distintivo del editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)  son extensiones que hacen que el texto del Editor de texto enriquecido (RTE) no se pueda editar. Esto significa que un distintivo declarado como tal solo se puede eliminar completamente y no se puede editar parcialmente. Estos distintivos también admiten colores especiales dentro de RTE, lo que indica claramente a los autores de contenido que el texto es un distintivo y, por lo tanto, no editable. Además, proporcionan indicaciones visuales sobre el significado del texto del distintivo.

El caso de uso más común de las insignias RTE es utilizarlas junto con [Widgets RTE](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). Esto permite que el contenido insertado en RTE por el widget RTE no se pueda editar.

Normalmente, los distintivos asociados a los widgets se utilizan para añadir contenido dinámico que tiene una dependencia externa del sistema, pero _los autores de contenido no pueden modificar_ el contenido dinámico insertado para mantener la integridad. Solo se pueden eliminar como un elemento completo.

El **distintivos** se añaden a **RTE** en el Editor de fragmentos de contenido mediante `rte` punto de extensión. Uso de `rte` punto de extensión `getBadges()` se añaden uno o varios distintivos al método.

Este ejemplo muestra cómo añadir un widget llamado _Servicio al cliente de reservas de grupos grandes_ para buscar, seleccionar y agregar los detalles de servicio al cliente específicos de WKND Adventure, como **Nombre del representante** y **Número de teléfono** con contenido RTE. Uso de la funcionalidad de distintivos en **Número de teléfono** está hecho **no editable** pero los autores de contenido de WKND pueden editar el Nombre del representante.

Además, la variable **Número de teléfono** tiene un estilo diferente (azul), que es un caso de uso adicional de la funcionalidad de distintivos.

Para que las cosas sean sencillas, este ejemplo utiliza el [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) para desarrollar la interfaz de usuario del widget o cuadro de diálogo y los números de teléfono del servicio de atención al cliente de WKND predefinidos. Para controlar la no edición y el aspecto de estilo diferente del contenido, la variable `#` se utiliza en la variable `prefix` y `suffix` atributo de la definición de distintivos.

## Puntos de extensión

Este ejemplo se extiende hasta el punto de extensión `rte` para añadir un distintivo al RTE en el editor de fragmentos de contenido.

| AEM Interfaz de usuario extendida | Puntos de extensión |
| ------------------------ | --------------------- | 
| [Editor de fragmentos de contenido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Distintivos del editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) y [Widgets del editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Extensión de ejemplo

En el siguiente ejemplo se crea un _Servicio al cliente de reservas de grupos grandes_ widget. Pulsando la tecla `{` dentro del RTE, se abre el menú contextual de los widgets RTE. Al seleccionar la variable _Servicio al cliente de reservas de grupos grandes_ en el menú contextual se abre el modal personalizado.

Una vez que el número de servicio al cliente deseado se añade desde el modal, los distintivos hacen que la variable _Número de teléfono no editable_ y lo diseña en color azul.

### Registro de extensiones

`ExtensionRegistration.js`, asignado al `index.html` AEM route, es el punto de entrada para la extensión de la y define:

+ La definición del distintivo se define en `getBadges()` uso de los atributos de configuración `id`, `prefix`, `suffix`, `backgroundColor` y `textColor`.
+ En este ejemplo, la variable `#` se utiliza para definir los límites de este distintivo, es decir, cualquier cadena del RTE rodeada por `#` se trata como una instancia de esta insignia.

Consulte también los detalles clave del widget RTE:

+ La definición del widget en `getWidgets()` función con `id`, `label` y `url` atributos.
+ El `url` valor de atributo, una ruta URL relativa (`/index.html#/largeBookingsCustomerService`) para cargar el modal.


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

          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",                    // Provide a unique ID for the badge
              prefix: "#",                          // Provide a Badge starting character
              suffix: "#",                          // Provide a Badge ending character
              backgroundColor: "",                  // Provide HEX or text CSS color code for the background
              textColor: "#071DF8"                  // Provide HEX or text CSS color code for the text
            }
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",       // Provide a unique ID for the widget
              label: "Large Group Bookings Customer Service",          // Provide a label for the widget
              url: "/index.html#/largeBookingsCustomerService",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/largeBookingsCustomerService`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Añadir `largeBookingsCustomerService` ruta en `App.js`{#add-widgets-route}

En el componente React principal `App.js`, añada el `largeBookingsCustomerService` para procesar la interfaz de usuario de la ruta URL relativa anterior.

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

### Crear `LargeBookingsCustomerService` Componente React{#create-widget-react-component}

La interfaz de usuario del widget o cuadro de diálogo se crea con la variable [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) marco.

El código del componente React al añadir los detalles del servicio de atención al cliente, rodee la variable del número de teléfono con el `#` carácter de insignias registradas para convertirlo en insignias, como `#${phoneNumber}#`, por lo tanto, no puede editarse.

Estos son los aspectos destacados de `LargeBookingsCustomerService` código:

+ La interfaz de usuario se procesa mediante componentes del espectro de React, como [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Botón](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ El `largeGroupCustomerServiceList` La matriz tiene una asignación codificada del nombre representativo y el número de teléfono. En situaciones reales, estos datos se pueden recuperar de la acción del AppBuilder de Adobe, de sistemas externos o de puertas de enlace de API caseras o basadas en proveedores de la nube.
+ El `guestConnection` se inicializa usando el `useEffect` [Reaccionar gancho](https://react.dev/reference/react/useEffect) y se administra como estado de componente. AEM Se utiliza para comunicarse con el host de la.
+ El `handleCustomerServiceChange` obtiene el nombre representativo y el número de teléfono, y actualiza las variables de estado del componente.
+ El `addCustomerServiceDetails` función que utiliza `guestConnection` El objeto proporciona la instrucción RTE que se debe ejecutar. En este caso `insertContent` instrucción y fragmento de código de HTML.
+ Para realizar la **número de teléfono no editable** con distintivos, la variable `#` se añade un carácter especial antes y después de la variable `phoneNumber` variable, like `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

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

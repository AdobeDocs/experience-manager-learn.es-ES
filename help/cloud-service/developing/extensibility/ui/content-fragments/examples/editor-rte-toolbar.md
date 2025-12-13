---
title: Agregar un botón personalizado a la barra de herramientas del Editor de texto enriquecido (RTE)
description: Aprenda a añadir un botón personalizado a la barra de herramientas del Editor de texto enriquecido (RTE) en el Editor de fragmentos de contenido de AEM
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Agregar un botón personalizado a la barra de herramientas del Editor de texto enriquecido (RTE)

Aprenda a añadir un botón personalizado a la barra de herramientas del Editor de texto enriquecido (RTE) en el Editor de fragmentos de contenido de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

Se pueden agregar botones personalizados a la **barra de herramientas RTE** en el Editor de fragmentos de contenido usando el punto de extensión `rte`. Este ejemplo muestra cómo agregar un botón personalizado denominado _Agregar sugerencia_ a la barra de herramientas de RTE y modificar el contenido dentro de RTE.

Utilizando el método `rte` del punto de extensión `getCustomButtons()`, se pueden agregar uno o varios botones personalizados a la **barra de herramientas RTE**. También es posible agregar o quitar botones RTE estándar como _Copiar, Pegar, Negrita y Cursiva_ utilizando los métodos `getCoreButtons()` y `removeButtons)` respectivamente.

Este ejemplo muestra cómo insertar una nota o sugerencia resaltada mediante el botón personalizado _Agregar sugerencia_ de la barra de herramientas. El contenido de la nota o sugerencia resaltado tiene un formato especial aplicado mediante elementos HTML y las clases CSS asociadas. El contenido del marcador de posición y el código HTML se insertan mediante el método de devolución de llamada `onClick()` de `getCustomButtons()`.

## Punto de extensión

Este ejemplo se extiende al punto de extensión `rte` para agregar un botón personalizado a la barra de herramientas RTE del Editor de fragmentos de contenido.

| IU de AEM extendida | Punto de extensión |
| ------------------------ | --------------------- |
| [Editor de fragmentos de contenido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Barra de herramientas del editor de texto enriquecido](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Extensión de ejemplo

En el siguiente ejemplo se crea un botón personalizado _Agregar sugerencia_ en la barra de herramientas RTE. La acción de hacer clic inserta el texto del marcador de posición en la posición del símbolo de intercalación actual en RTE.

El código muestra cómo añadir el botón personalizado con un icono y registrar la función del controlador de clics.

### Registro de extensiones

`ExtensionRegistration.js`, asignado a la ruta index.html, es el punto de entrada para la extensión de AEM y define:

+ La definición del botón de barra de herramientas RTE en la función `getCustomButtons()` con atributos `id, tooltip and icon`.
+ Controlador de clic para el botón, en la función `onClick()`.
+ La función del controlador de clic recibe el objeto `state` como argumento para obtener el contenido del RTE en formato HTML o de texto. Sin embargo, en este ejemplo no se utiliza.
+ La función del controlador de clics devuelve una matriz de instrucciones. Esta matriz tiene un objeto con `type` y `value` atributos. Para insertar el contenido, los atributos `value` y el fragmento de código HTML, el atributo `type` utiliza `insertContent`. Si hay un caso de uso para reemplazar el contenido, use el tipo de instrucción `replaceContent`.

El valor `insertContent` es una cadena de HTML, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. Las clases CSS `cmp-contentfragment__element-tip` utilizadas para mostrar el valor no están definidas en el widget, sino implementadas en la experiencia web en la que se muestra este campo de fragmento de contenido.


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
          // RTE Toolbar custom button
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
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

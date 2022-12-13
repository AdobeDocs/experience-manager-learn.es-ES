---
title: Extensiones del menú de encabezado de la consola de fragmentos de contenido de AEM
description: Obtenga información sobre cómo crear extensiones de menú de encabezado de la consola de fragmentos de contenido AEM.
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
source-wordcount: '366'
ht-degree: 0%

---


# Extensión de menú de encabezado

![Extensión de menú de encabezado](./assets/header-menu/header-menu.png){align="center"}

Extensiones que incluyen un menú de encabezado, introduzca un botón en el encabezado de la Consola de fragmentos de contenido de AEM que se muestra cuando __no__ Los fragmentos de contenido están seleccionados. Debido a que los botones de extensión del menú de encabezado solo se muestran cuando no se selecciona ningún fragmento de contenido, normalmente no actúan sobre los fragmentos de contenido existentes. En su lugar, las extensiones de los menús de encabezado suelen:

+ Cree nuevos fragmentos de contenido mediante lógica personalizada, como crear un conjunto de fragmentos de contenido, vinculados mediante referencias de contenido.
+ Actuar sobre un conjunto seleccionado mediante programación de fragmentos de contenido, como exportar todos los fragmentos de contenido creados en la última semana.

## Registro de extensiones

`ExtensionRegistration.js` es el punto de entrada de la extensión de AEM y define:

1. El tipo de extensión; en el caso de un botón de menú de encabezado.
1. La definición del botón de extensión, en `getButton()` función.
1. El controlador de clic del botón, en la sección `onClick()` función.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Must be unique
      methods: {
        // Configure your Header Menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-button',    // Unique ID for the button
              'label': 'My header menu button',         // Button label 
              'icon': 'Bookmark'                        // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the Header Menu extension button
          onClick() {
            // Header Menu buttons are not associated with selected Content Fragment, and thus are not provided a selection parameter.        
            // Do work like importing data from a well known location, or exporting a welll known set of data
            doWork();            
          },
        }
      }
    }
  }
  init().catch(console.error);
}
```

## Modal

![Modal](./assets/modal/modal.png)

AEM las extensiones de menú de encabezado de la consola de fragmentos de contenido pueden requerir:

+ Entrada adicional del usuario para realizar la acción deseada.
+ La capacidad de proporcionar al usuario información detallada sobre el resultado de la acción.

Para satisfacer estos requisitos, la extensión de la consola de fragmentos de contenido de AEM permite un modal personalizado que se procesa como una aplicación React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-extension";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pasar a la creación de un modal</p>
      <p class="has-text-blackest">Obtenga información sobre cómo crear un modal que se muestre al hacer clic en el botón de extensión del menú de encabezado.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Aprenda a crear un modal">Aprenda a crear un modal</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Sin modal

En ocasiones, AEM extensiones de menú de encabezado de la consola Fragmento de contenido no requieren una interacción más profunda con el usuario, por ejemplo:

+ Invocación de un proceso back-end que no requiera la entrada del usuario, como importar o exportar.
+ Apertura de una nueva página web, como a la documentación interna sobre las directrices de contenido.

En estos casos, la extensión de la consola de fragmentos de contenido de AEM no requiere una [modal](#modal)y puede realizar el trabajo directamente en el botón de menú del encabezado `onClick` controlador.

La extensión de la consola de fragmentos de contenido de AEM permite que un indicador de progreso superponga la consola de fragmentos de contenido de AEM mientras se realiza el trabajo, lo que impide que el usuario realice más acciones. El uso del indicador de progreso es opcional, pero útil para comunicar al usuario el progreso del trabajo sincrónico.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      headerMenu: { ...
        onClick() {
          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork();
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```

---
title: AEM Extensiones de menú de encabezado de la consola Fragmento de contenido
description: AEM Obtenga información sobre cómo crear extensiones de menú de encabezado de la consola Fragmento de contenido de la consola de.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 92d6e98e-24d0-4229-9d30-850f6b72ab43
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Extensión de menú de encabezado

![Extensión de menú de encabezado](./assets/header-menu/header-menu.png){align="center"}

AEM Las extensiones que incluyen un menú de encabezado introducen un botón en el encabezado de la consola de fragmentos de contenido de que se muestra al __no__ Los fragmentos de contenido están seleccionados. Dado que los botones de extensión de menú de encabezado solo se muestran cuando no se selecciona ningún fragmento de contenido, no suelen actuar sobre los fragmentos de contenido existentes. En su lugar, las extensiones de menús de encabezado suelen:

+ Cree nuevos fragmentos de contenido mediante lógica personalizada, como un conjunto de fragmentos de contenido, vinculados mediante referencias de contenido.
+ Actuar en un conjunto de fragmentos de contenido seleccionado mediante programación, como exportar todos los fragmentos de contenido creados en la última semana.

## Registro de extensiones

`ExtensionRegistration.js` AEM es el punto de entrada para la extensión de la y define lo siguiente:

1. El tipo de extensión; en el caso de un botón de menú de encabezado.
1. Definición del botón de extensión, en `getButton()` función.
1. El controlador de clics para el botón, en el `onClick()` función.

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

AEM Las extensiones de menú de encabezado de la consola Fragmento de contenido pueden requerir lo siguiente:

+ Entrada adicional del usuario para realizar la acción deseada.
+ La capacidad de proporcionar al usuario información detallada sobre el resultado de la acción.

AEM Para admitir estos requisitos, la extensión de la consola Fragmento de contenido de la aplicación permite un modal personalizado que se procesa como una aplicación de React.

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
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pasar a creación de un modal</p>
      <p class="has-text-blackest">Obtenga información sobre cómo crear un modal que se muestra al hacer clic en el botón de extensión de menú de encabezado.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Aprenda a crear un modal">Aprenda a crear un modal</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Sin modal

AEM En ocasiones, las extensiones de menú de encabezado de la consola Fragmento de contenido no requieren ninguna interacción adicional con el usuario, por ejemplo:

+ Invocar un proceso back-end que no requiera la entrada del usuario, como la importación o exportación.
+ Abrir una nueva página web, como para acceder a documentación interna sobre las directrices de contenido.

AEM En estos casos, la extensión de la consola Fragmento de contenido de la aplicación no requiere un [modal](#modal), y puede realizar el trabajo directamente en los botones del menú del encabezado `onClick` controlador.

AEM AEM La extensión de la consola Fragmento de contenido permite que un indicador de progreso se superponga a la consola Fragmento de contenido de la aplicación mientras se realiza el trabajo, lo que impide que el usuario realice más acciones. El uso del indicador de progreso es opcional, pero resulta útil para comunicar el progreso del trabajo sincrónico al usuario.

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

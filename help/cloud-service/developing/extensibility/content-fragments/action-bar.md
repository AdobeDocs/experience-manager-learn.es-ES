---
title: Extensiones de la barra de acciones de la consola Fragmento de contenido de AEM
description: Obtenga información sobre cómo crear extensiones de barra de acciones de la consola Fragmento de contenido AEM.
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
source-wordcount: '333'
ht-degree: 0%

---


# Extensión de la barra de acciones

![Extensión de la barra de acciones](./assets/action-bar/action-bar.png){align="center"}

Extensiones que incluyen una barra de acciones, introduzca un botón en la acción de la Consola de fragmentos de contenido de AEM que se muestra al __1 o más__ Los fragmentos de contenido están seleccionados. Debido a que los botones de extensión de la barra de acciones solo se muestran cuando se selecciona al menos un fragmento de contenido, normalmente actúan sobre los fragmentos de contenido seleccionados. Algunos ejemplos son:

+ Invocación de un proceso empresarial o flujo de trabajo en los fragmentos de contenido seleccionados.
+ Actualizar o cambiar los datos de los fragmentos de contenido seleccionados.

## Registro de extensiones

`ExtensionRegistration.js` es el punto de entrada de la extensión de AEM y define:

1. El tipo de extensión; en el caso de un botón de barra de acciones.
1. La definición del botón de extensión, en `getButton()` función.
1. El controlador de clic del botón, en la sección `onClick()` función.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## Modal

![Modal](./assets/modal/modal.png)

AEM las extensiones de la barra de acciones de la consola de fragmentos de contenido pueden requerir:

+ Entrada adicional del usuario para realizar la acción deseada.
+ La capacidad de proporcionar al usuario información detallada sobre el resultado de la acción.

Para satisfacer estos requisitos, la extensión de la consola de fragmentos de contenido de AEM permite un modal personalizado que se procesa como una aplicación React.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Pasar a la creación de un modal</p>
      <p class="has-text-blackest">Aprenda a crear un modal que se muestre al hacer clic en el botón de extensión de la barra de acciones.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Aprenda a crear un modal">Aprenda a crear un modal</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Sin modal

En ocasiones, AEM extensiones de la barra de acciones de la consola de fragmentos de contenido no requieren más interacción con el usuario, por ejemplo:

+ Invocación de un proceso back-end que no requiera la entrada del usuario, como importar o exportar.

En estos casos, la extensión de la consola Fragmento de contenido de AEM no requiere una [modal](#modal)y realice el trabajo directamente en el botón de barra de acciones `onClick` controlador.

La extensión de la consola Fragmento de contenido de AEM permite que un indicador de progreso superponga la consola Fragmento de contenido de AEM mientras se realiza el trabajo, lo que impide que el usuario realice más acciones. El uso del indicador de progreso es opcional, pero útil para comunicar al usuario el progreso del trabajo sincrónico.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```

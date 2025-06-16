---
title: Cómo utilizar AEM React Editable Components v2
description: Aprenda a utilizar AEM React Editable Components v2 para activar una aplicación de React.
version: Experience Manager as a Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 190
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 1%

---

# Cómo utilizar AEM React Editable Components v2

{{spa-editor-deprecation}}

AEM proporciona [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), un SDK basado en Node.js que permite crear componentes React, que admiten la edición de componentes en contexto mediante AEM SPA Editor.

* [módulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
* [Proyecto de Github](https://github.com/adobe/aem-react-editable-components)
* [Documentación de Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Para obtener más información y ejemplos de código para la versión 2.0 de los componentes editables de AEM React, consulte la documentación técnica:

* [Integración con la documentación de AEM](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
* [Documentación de componente editable](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
* [Documentación de ayudantes](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## Páginas de AEM

Los componentes editables de AEM React funcionan tanto con aplicaciones de Editor de SPA como con aplicaciones de React de SPA remotas. El contenido que rellena los componentes editables de React debe exponerse a través de páginas de AEM que extienden el [componente de página SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). Los componentes de AEM, que se asignan a componentes editables de React, deben implementar el [marco del exportador de componentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) de AEM, como los [componentes principales de WCM de AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es).


## Dependencias

Asegúrese de que la aplicación React se esté ejecutando en Node.js 14+.

El conjunto mínimo de dependencias para que la aplicación React utilice AEM React Editable Components v2 es: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping` y `@adobe/aem-spa-page-model-manager`.

* `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [La base de componentes de AEM React Core WCM](https://github.com/adobe/aem-react-core-wcm-components-base) y [AEM React Core WCM Components SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) no son compatibles con AEM React Editable Components v2.

## Editor de SPA 

Al utilizar los componentes editables de AEM React con una aplicación React basada en el Editor de SPA, AEM `ModelManager` SDK, como SDK:

1. Recupera contenido de AEM
1. Rellena los componentes comestibles de React con contenido de AEM

Ajuste la aplicación React con un ModelManager inicializado y procese la aplicación React. La aplicación React debe contener una instancia del componente `<Page>` exportado desde `@adobe/aem-react-editable-components`. El componente `<Page>` tiene lógica para crear dinámicamente componentes de React basados en `.model.json` proporcionados por AEM.

* `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

`<Page>` se pasa como representación de la página de AEM como JSON, a través de `pageModel` proporcionado por `ModelManager`. El componente `<Page>` crea dinámicamente componentes React para los objetos de `pageModel` al hacer coincidir `resourceType` con un componente React que se registra al tipo de recurso mediante `MapTo(..)`.

## Componentes editables

Se pasó `<Page>` a la representación de la página de AEM como JSON, a través de `ModelManager`. A continuación, el componente `<Page>` crea dinámicamente componentes de React para cada objeto en el JSON haciendo coincidir el valor `resourceType` del objeto JS con un componente de React que se registra al tipo de recurso a través de la invocación `MapTo(..)` del componente. Por ejemplo, se utilizaría lo siguiente para crear una instancia

* `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

El JSON anterior proporcionado por AEM se puede usar para crear instancias de forma dinámica y rellenar un componente React editable.

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## Incrustar componentes

Los componentes editables se pueden reutilizar e incrustar entre sí. Existen dos consideraciones clave al incrustar un componente editable en otro:

1. El contenido JSON de AEM para el componente de incrustación debe contener el contenido para satisfacer los componentes incrustados. Esto se hace creando un cuadro de diálogo para el componente AEM que recopila los datos necesarios.
1. La instancia &quot;no editable&quot; del componente React debe estar incrustada, en lugar de la instancia &quot;editable&quot; que está ajustada con `<EditableComponent>`. El motivo es que, si el componente incrustado tiene el envoltorio `<EditableComponent>`, el Editor de SPA intenta vestir el componente interno con el cuadro de edición de Chrome (cuadro de desplazamiento azul), en lugar del componente de incrustación exterior.

* `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

El JSON anterior proporcionado por AEM se puede usar para crear instancias de forma dinámica y rellenar un componente React editable que incruste otro componente React.


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```

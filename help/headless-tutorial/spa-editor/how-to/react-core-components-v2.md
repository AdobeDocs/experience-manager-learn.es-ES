---
title: Cómo utilizar AEM componentes editables React v2
description: Aprenda a utilizar AEM React Editable Components v2 para activar una aplicación React.
version: Cloud Service
feature: SPA Editor
topic: Headless
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 1%

---


# Cómo utilizar AEM componentes editables React v2

AEM proporciona [Componentes editables de AEM React v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), un SDK basado en Node.js que permite la creación de componentes React, que admiten la edición de componentes en contexto mediante AEM editor de SPA.

+ [módulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Proyecto Github](https://github.com/adobe/aem-react-editable-components)
+ [documentación de Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Para obtener más información y ejemplos de código para AEM React Editable Components v2, consulte la documentación técnica:

+ [Integración con AEM documentación](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentación de componentes editable](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentación de los ayudantes](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM páginas

AEM los componentes editables React funcionan tanto con SPA Editor como con aplicaciones React de SPA remoto. El contenido que rellena los componentes editables de React debe exponerse a través de páginas AEM que amplíen el [Componente SPA página](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html). AEM componentes, que se asignan a componentes de React editables, deben implementar AEM [Marco de exportación de componentes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) - como [Componentes principales de AEM WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es).


## Dependencias

Asegúrese de que la aplicación React se esté ejecutando en Node.js 14+.

El conjunto mínimo de dependencias para que la aplicación React use AEM React Editable Components v2 son: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`y  `@adobe/aem-spa-page-model-manager`.


+ `package.json`

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
> [Base de componentes WCM principales de React AEM](https://github.com/adobe/aem-react-core-wcm-components-base) y [AEM Componentes principales de WCM SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) no son compatibles con AEM React Editable Components v2.

## Editor SPA

Al utilizar los componentes editables de React AEM con una aplicación React basada en SPA Editor, el AEM `ModelManager` SDK, como SDK:

1. Recupera contenido de AEM
1. Rellena los componentes ReactComble con contenido AEM

Ajuste la aplicación React con un ModelManager inicializado y represente la aplicación React. La aplicación React debe contener una instancia de `<Page>` componente exportado de `@adobe/aem-react-editable-components`. La variable `<Page>` tiene lógica para crear componentes de React de forma dinámica en función del `.model.json` proporcionado por AEM.

+ `src/index.js`

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

La variable `<Page>` se pasa como la representación de la página de AEM como JSON, a través de la variable `pageModel` proporcionado por el `ModelManager`. La variable `<Page>` crea de forma dinámica componentes React para los objetos del `pageModel` coincidiendo con la variable `resourceType` con un componente React que se registra a sí mismo en el tipo de recurso mediante `MapTo(..)`.

## Componentes editables

La variable `<Page>` se pasa a la representación de la página AEM como JSON, a través de la variable `ModelManager`. La variable `<Page>` a continuación, crea de forma dinámica componentes de React para cada objeto del JSON al hacer coincidir el objeto de JS `resourceType` con un componente React que se registra al tipo de recurso mediante el `MapTo(..)` invocación. Por ejemplo, se utilizaría lo siguiente para crear una instancia

+ `HTTP GET /content/.../home.model.json`

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

El JSON anterior proporcionado por AEM podría usarse para crear una instancia dinámica de un componente React editable y rellenarlo.

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

## Incrustación de componentes

Los componentes editables se pueden reutilizar e incrustar entre sí. Existen dos consideraciones clave al incrustar un componente editable en otro:

1. El contenido JSON de AEM para el componente de incrustación debe contener el contenido para satisfacer los componentes incrustados. Esto se hace creando un cuadro de diálogo para el componente AEM que recopila los datos necesarios.
1. La instancia &quot;no editable&quot; del componente React debe estar incrustada, en lugar de la instancia &quot;editable&quot;, que está empaquetada con `<EditableComponent>`. El motivo es, si el componente incrustado tiene la variable `<EditableComponent>` para ajustar, el SPA Editor intenta vestir el componente interior con el cromo de edición (cuadro azul de pase), en lugar del componente incrustado exterior.

+ `HTTP GET /content/.../home.model.json`

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

El JSON anterior proporcionado por AEM podría usarse para crear instancias de forma dinámica y rellenar un componente React editable que incruste otro componente React.


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




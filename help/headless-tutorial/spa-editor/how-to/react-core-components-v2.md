---
title: AEM Uso de componentes editables de React v2 de
description: AEM Aprenda a utilizar componentes editables de React v2 de para alimentar una aplicación de React.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 190
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 0%

---

# AEM Uso de componentes editables de React v2 de

{{edge-delivery-services}}

AEM AEM AEM SPA proporciona [Componentes editables de React v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), un SDK basado en Node.js que permite la creación de componentes de React, que admiten la edición de componentes en contexto mediante el Editor de componentes de.

+ [módulo npm](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Proyecto de Github](https://github.com/adobe/aem-react-editable-components)
+ [Documentación de Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


AEM Para obtener más información y ejemplos de código para la versión 2.0 de los componentes editables de React, consulte la documentación técnica:

+ AEM [Integración con documentación de](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Documentación de componente editable](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Documentación de ayudantes](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM Páginas de

AEM SPA SPA Los componentes editables de React funcionan tanto con las aplicaciones de Editor de como con las de React de remoto. AEM SPA El contenido que rellena los componentes editables de React debe exponerse a través de páginas de que amplíen [el componente de página](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). AEM AEM AEM Los componentes de React, que se asignan a componentes editables, deben implementar el marco de trabajo del exportador de componentes [, como [componentes principales de WCMs](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es), que se pueden implementar de forma predeterminada, como por ejemplo, los componentes de ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html).


## Dependencias

Asegúrese de que la aplicación React se esté ejecutando en Node.js 14+.

AEM El conjunto mínimo de dependencias para que la aplicación React utilice componentes editables de React v2 es: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping` y `@adobe/aem-spa-page-model-manager`.


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
> AEM AEM SPA AEM [React Core WCM Components Base](https://github.com/adobe/aem-react-core-wcm-components-base) y [React Core WCM Components](https://github.com/adobe/aem-react-core-wcm-components-spa) no son compatibles con los componentes editables de React v2 de.

## SPA Editor de

AEM SPA AEM Al utilizar los componentes editables de React de la aplicación de la aplicación React basada en el Editor de la, el SDK de `ModelManager` de la aplicación se utiliza como SDK:

1. AEM Recupera contenido de la
1. AEM Rellena los componentes comestibles de React con contenido que se muestra a la vista de la

Ajuste la aplicación React con un ModelManager inicializado y procese la aplicación React. La aplicación React debe contener una instancia del componente `<Page>` exportado desde `@adobe/aem-react-editable-components`. AEM El componente `<Page>` tiene lógica para crear de forma dinámica componentes de React basados en `.model.json` proporcionados por los usuarios de la aplicación de tipo de datos de la aplicación de tipo de datos.

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

AEM `<Page>` se pasa como la representación de la página de la página de como JSON, a través de `pageModel` proporcionado por `ModelManager`. El componente `<Page>` crea dinámicamente componentes React para los objetos de `pageModel` al hacer coincidir `resourceType` con un componente React que se registra al tipo de recurso mediante `MapTo(..)`.

## Componentes editables

AEM Se pasó `<Page>` a la representación de la página de la como JSON, a través de `ModelManager`. A continuación, el componente `<Page>` crea dinámicamente componentes de React para cada objeto en el JSON haciendo coincidir el valor `resourceType` del objeto JS con un componente de React que se registra al tipo de recurso a través de la invocación `MapTo(..)` del componente. Por ejemplo, se utilizaría lo siguiente para crear una instancia

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

AEM El JSON anterior proporcionado por el usuario de la aplicación de datos podría utilizarse para crear instancias de forma dinámica y rellenar un componente React editable.

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

1. AEM El contenido JSON de la para el componente de incrustación debe contener el contenido para satisfacer los componentes incrustados. AEM Esto se realiza creando un cuadro de diálogo para el componente de la que recopila los datos necesarios.
1. La instancia &quot;no editable&quot; del componente React debe estar incrustada, en lugar de la instancia &quot;editable&quot; que está ajustada con `<EditableComponent>`. SPA El motivo es que, si el componente incrustado tiene el envoltorio `<EditableComponent>`, el Editor intenta vestir el componente interno con el cuadro de edición de cromo (cuadro de desplazamiento azul), en lugar del componente de incrustación exterior.

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

AEM El JSON anterior proporcionado por el usuario de la aplicación de datos podría utilizarse para crear instancias de forma dinámica y rellenar un componente de React editable que incruste otro componente de React.


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

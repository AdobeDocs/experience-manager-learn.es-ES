---
title: Uso de texto enriquecido con AEM sin encabezado
description: Aprenda a crear contenido e incruste contenido referenciado mediante un editor de texto enriquecido multilínea con fragmentos de contenido de Adobe Experience Manager, y cómo las API de GraphQL AEM texto enriquecido como JSON lo consumen aplicaciones sin encabezado.
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 0%

---

# Texto enriquecido con AEM sin encabezado

El campo Texto multilínea es un tipo de datos de fragmentos de contenido que permite a los autores crear contenido de texto enriquecido. Las referencias a otro contenido, como imágenes u otros fragmentos de contenido, se pueden insertar dinámicamente en línea dentro del flujo del texto. AEM API de GraphQL ofrece una sólida capacidad para devolver texto enriquecido como HTML, texto sin formato o como JSON puro. La representación JSON es potente, ya que proporciona a la aplicación cliente control total sobre cómo procesar el contenido.

## Editor multilínea

>[!VIDEO](https://video.tv.adobe.com/v/342104/?quality=12&learn=on)

En el Editor de fragmentos de contenido, la barra de menús del campo de texto de varias líneas proporciona a los autores capacidades estándar de formato de texto enriquecido, como **bold**, *cursiva* y subrayado. Al abrir el campo Multi line en el modo de pantalla completa, se habilita [herramientas de formato adicionales, como el tipo de párrafo, buscar y reemplazar, revisión ortográfica, etc.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

## Tipo de datos de varias líneas de texto {#multi-line-data-type}

Utilice la variable **Texto de varias líneas** tipo de datos al definir el modelo de fragmento de contenido para permitir la creación de texto enriquecido.

![Tipo de datos de texto enriquecido de varias líneas](assets/rich-text/multi-line-rich-text.png)

Al utilizar el tipo de datos Multi-line text , puede establecer la variable **Tipo predeterminado** a:

* Texto enriquecido
* Markdown
* Texto sin formato

La variable **Tipo predeterminado** influye directamente en la experiencia de edición y determina si están presentes las herramientas de texto enriquecido.

También puede [habilitar referencias en línea](#insert-fragment-references) a otros fragmentos de contenido comprobando la variable **Permitir referencia de fragmento** y configurar la variable **Modelos de fragmento de contenido permitidos**.

## Respuesta de texto enriquecido con la API de GraphQL

Al crear una consulta de GraphQL, los desarrolladores pueden elegir diferentes tipos de respuesta de `html`, `plaintext`, `markdown`y `json` desde un campo Multi line .

Los desarrolladores pueden utilizar el [Vista previa de JSON](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) en el editor de fragmentos de contenido para mostrar todos los valores del fragmento de contenido actual que se pueden devolver mediante la API de GraphQL.

### Ejemplo de JSON

La variable `json` response ofrece la mayor flexibilidad para los desarrolladores de front-end cuando trabajan con contenido de texto enriquecido. El contenido de texto enriquecido se entrega como una matriz de tipos de nodos JSON que se pueden procesar de forma única en función de la plataforma del cliente.

A continuación se muestra un tipo de respuesta JSON de un campo multilínea denominado `main` que contiene un párrafo: &quot;*Este es un párrafo que incluye **important**contenido.*&quot; donde &quot;importante&quot; está marcado como **bold**.

**Consulta de GraphQL:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

**Respuesta de GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### Otros ejemplos

A continuación se muestran varios ejemplos de tipos de respuesta de un campo multilínea denominado `main` que contiene un párrafo: &quot;Este es un párrafo que incluye **important** contenido&quot;. donde &quot;importante&quot; está marcado como **bold**.

++ejemplo de HTML

**Consulta de GraphQL:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**Respuesta de GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

++Ejemplo de Markdown

**Consulta de GraphQL:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**Respuesta de GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++Ejemplo de texto sin formato

**Consulta de GraphQL:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**Respuesta de GraphQL:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

La variable `plaintext` procesar elimina cualquier formato.

+++


## Representación de una respuesta JSON de texto enriquecido {#render-multiline-json-richtext}

La respuesta JSON de texto enriquecido del campo Multi line se estructura como un árbol jerárquico. Cada objeto o nodo representa un bloque de HTML diferente del texto enriquecido.

A continuación, se muestra una respuesta JSON de ejemplo de un campo de texto Multi line . Observe que cada objeto, o nodo, incluye un `nodeType` que representa el bloque de HTML del texto enriquecido como `paragraph`, `link`y `text`. Cada nodo contiene opcionalmente `content` que es una submatriz que contiene cualquier elemento secundario del nodo actual.

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

La forma más sencilla de procesar las líneas múltiples `json` La respuesta es procesar cada objeto o nodo en la respuesta y, a continuación, procesar los elementos secundarios del nodo actual. Se puede utilizar una función recursiva para recorrer el árbol JSON.

A continuación se muestra un código de ejemplo que ilustra un enfoque recursivo transversal. Los ejemplos están basados en JavaScript y utilizan React&#39;s [JSX](https://reactjs.org/docs/introducing-jsx.html), sin embargo, los conceptos de programación pueden aplicarse a cualquier idioma.

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

La variable `renderNodeList` es el punto de entrada en el algoritmo recursivo. La variable `renderNodeList` espera una matriz de `childNodes`. A continuación, cada nodo de la matriz se pasa a una función `renderNode`.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

La variable `renderNode` espera un único objeto llamado `node`. Un nodo puede tener elementos secundarios que se procesan recursivamente mediante la variable `renderNodeList` función descrita anteriormente. Por último, una `nodeMap` se utiliza para procesar el contenido del nodo en función de su `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

La variable `nodeMap` es un literal de objeto JavaScript que se utiliza como mapa. Cada una de las &quot;claves&quot; representa una `nodeType`. Parámetros de `node` y `children` se puede pasar a las funciones resultantes que renderizan el nodo . El tipo de retorno utilizado en este ejemplo es JSX, sin embargo, el método se puede adaptar para crear un literal de cadena que represente el contenido del HTML.

### Ejemplo de código completo

Se puede encontrar una utilidad de renderización de texto enriquecido reutilizable en el [Ejemplo de WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app/src/utils/renderRichText.js) - utilidad reutilizable que expone una función `mapJsonRichText`. Esta utilidad la pueden utilizar los componentes que desean renderizar una respuesta JSON de texto enriquecido como React JSX.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) : componente de ejemplo que realiza una solicitud de GraphQL que incluye texto enriquecido. El componente utiliza la variable `mapJsonRichText` para procesar el texto enriquecido y cualquier referencia.


## Agregar referencias en línea a texto enriquecido {#insert-fragment-references}

El campo Línea múltiple permite a los autores insertar imágenes u otros recursos digitales desde AEM Assets en el flujo del texto enriquecido.

![insertar imagen](assets/rich-text/insert-image.png)

La captura de pantalla anterior muestra una imagen insertada en el campo Línea múltiple con el **Insertar recurso** botón.

Las referencias a otros fragmentos de contenido también se pueden vincular o insertar en el campo Multilínea usando la variable **Insertar fragmento de contenido** botón.

![Insertar referencia de fragmento de contenido](assets/rich-text/insert-contentfragment.png)

La captura de pantalla de arriba representa otro fragmento de contenido, la Guía Ultimate para los Parques de Skate LA, insertándolo en el campo multilínea. Los tipos de fragmentos de contenido que se pueden insertar en el campo están controlados por la variable **Modelos de fragmento de contenido permitidos** en el [Tipo de datos de varias líneas](#multi-line-data-type) en el Modelo de fragmento de contenido.

## Consultar referencias en línea con GraphQL

La API de GraphQL permite a los desarrolladores crear una consulta que incluya propiedades adicionales sobre cualquier referencia insertada en un campo de varias líneas. La respuesta JSON incluye una `_references` objeto que enumera estas propiedades adicionales. La respuesta JSON proporciona a los desarrolladores control total sobre cómo procesar las referencias o los vínculos en lugar de tener que lidiar con un HTML de opinión.

Por ejemplo, puede que desee:

* Incluya lógica de enrutamiento personalizada para administrar vínculos a otros fragmentos de contenido al implementar una aplicación de una sola página, como usar el enrutador React o Next.js
* Representar una imagen en línea utilizando la ruta absoluta a un entorno de AEM Publish como `src` valor.
* Determine cómo procesar una referencia incrustada a otro fragmento de contenido con propiedades personalizadas adicionales.

Utilice la variable `json` tipo de devolución e incluya la variable `_references` al crear una consulta de GraphQL:

**Consulta de GraphQL:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _path
        _publishUrl
        width
      }
      ...on ArticleModel {
        _path
        author
      }
      
    }
  }
}
```

En la consulta anterior, la variable `main` se devuelve como JSON. La variable `_references` el objeto incluye fragmentos para gestionar cualquier referencia de tipo `ImageRef` o de tipo `ArticleModel`.

**Respuesta JSON:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "_publishUrl": "http://localhost:4503/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "width": 1920
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
        }
      ]
    }
  }
}
```

La respuesta JSON incluye dónde se insertó la referencia en el texto enriquecido con la variable `"nodeType": "reference"`. La variable `_references` a continuación, incluye cada referencia con las propiedades adicionales solicitadas. Por ejemplo, la variable `ImageRef` devuelve la variable `width` de la imagen a la que se hace referencia en el artículo.

## Representación de referencias en línea en texto enriquecido

Para presentar referencias en línea, el enfoque recursivo explicado en [Representación de una respuesta JSON de varias líneas](#render-multiline-json-richtext) se puede expandir.

Donde `nodeMap` es el mapa que procesa los nodos JSON.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if(node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if(node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

El enfoque de alto nivel es inspeccionar cada vez que `nodeType` es igual que `reference` en la respuesta JSON de línea múltiple. A continuación, se puede llamar a una función de renderización personalizada que incluya la variable `_references` objeto devuelto en la respuesta de GraphQL.

La ruta de referencia en línea se puede comparar con la entrada correspondiente en la variable `_references` objeto y otro mapa personalizado `renderReference` se puede llamar a .

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._path} alt={'in-line reference'} /> 
    },
    'AdventureModel': (node) => {
        // when __typename === AdventureModel
        return <Link to={`/adventure:${node._path}`}>{`${node.adventureTitle}: ${node.adventurePrice}`}</Link>;
    }
    ...
}
```

La variable `__typename` del `_references` puede utilizarse para asignar distintos tipos de referencia a diferentes funciones de renderización.

### Ejemplo de código completo

Puede encontrar un ejemplo completo de cómo escribir un procesador de referencias personalizadas en [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) como parte del [Ejemplo de WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Ejemplo completo

>[!VIDEO](https://video.tv.adobe.com/v/342105/?quality=12&learn=on)

El vídeo anterior muestra un ejemplo completo:

1. Actualización del campo de texto Multi line de un Modelo de fragmento de contenido para permitir referencias de fragmento
1. Uso del editor de fragmentos de contenido para incluir una imagen y hacer referencia a otro fragmento en un campo de texto de varias líneas.
1. Creación de una consulta de GraphQL que incluya la respuesta de texto multilínea como JSON y cualquier `_references` se utiliza.
1. Escribir una SPA React que muestre las referencias en línea de la respuesta de texto enriquecido.

---
title: AEM Uso de texto enriquecido con sin encabezado
description: Aprenda a crear contenido e incrustar contenido referenciado mediante un editor de texto enriquecido multilínea con fragmentos de contenido de Adobe Experience Manager AEM, y cómo se entrega el texto enriquecido mediante el uso de API de GraphQL como JSON para que las aplicaciones sin encabezado lo consuman.
version: Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 785
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# AEM Texto enriquecido con sin encabezado de

El campo de texto multilínea es un tipo de datos de fragmentos de contenido que permite a los autores crear contenido de texto enriquecido. Las referencias a otro contenido, como imágenes u otros fragmentos de contenido, se pueden insertar dinámicamente en línea dentro del flujo del texto. El campo Texto de una sola línea es otro tipo de datos de fragmentos de contenido que deben utilizarse para elementos de texto simples.

AEM API de GraphQL ofrece una sólida capacidad para devolver texto enriquecido como HTML, texto sin formato o como JSON puro. La representación JSON es potente, ya que proporciona a la aplicación cliente control total sobre cómo procesar el contenido.

## Editor de varias líneas

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

En el Editor de fragmentos de contenido, la barra de menús del campo de texto multilínea proporciona a los autores funciones de formato de texto enriquecido estándar, como **negrita**, *cursiva* y subrayado. Al abrir el campo multilínea en modo de pantalla completa, se habilitarán [herramientas de formato adicionales, como tipo de párrafo, buscar y reemplazar, revisión ortográfica y más](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=es).

>[!NOTE]
>
> Los complementos de texto enriquecido del editor de varias líneas no se pueden personalizar.

## Tipo de datos de texto multilínea {#multi-line-data-type}

Utilice el tipo de datos **Texto multilínea** al definir el modelo de fragmento de contenido para habilitar la creación de texto enriquecido.

![Tipo de datos de texto enriquecido multilínea](assets/rich-text/multi-line-rich-text.png)

Se pueden configurar varias propiedades del campo multilínea.

La propiedad **Procesar como** se puede establecer en:

* Área de texto: representa un solo campo multilínea
* Campo múltiple: procesa varios campos de varias líneas


El **tipo predeterminado** se puede establecer en:

* Texto enriquecido
* Markdown
* Texto sin formato

La opción **Tipo predeterminado** influye directamente en la experiencia de edición y determina si están presentes las herramientas de texto enriquecido.

También puede [habilitar referencias en línea](#insert-fragment-references) a otros fragmentos de contenido comprobando **Permitir referencia a fragmento** y configurando los **modelos de fragmentos de contenido permitidos**.

Marque la casilla **Traducible** si el contenido se va a localizar. Solo se puede localizar texto enriquecido y texto sin formato. Consulte [trabajar con contenido localizado para obtener más información](./localized-content.md).

## Respuesta de texto enriquecido con la API de GraphQL

Al crear una consulta de GraphQL, los desarrolladores pueden elegir diferentes tipos de respuesta de `html`, `plaintext`, `markdown` y `json` de un campo de varias líneas.

Los desarrolladores pueden usar la [Vista previa de JSON](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html?lang=es) en el editor de fragmentos de contenido para mostrar todos los valores del fragmento de contenido actual que se pueden devolver mediante la API de GraphQL.

## Consulta persistente de GraphQL

La selección del formato de respuesta `json` para el campo multilínea ofrece la mayor flexibilidad al trabajar con contenido de texto enriquecido. El contenido de texto enriquecido se entrega como una matriz de tipos de nodos JSON que se pueden procesar de forma exclusiva en función de la plataforma del cliente.

A continuación se muestra un tipo de respuesta JSON de un campo de varias líneas denominado `main` que contiene un párrafo: &quot;*Este es un párrafo que incluye contenido de **important**.*&quot; donde &quot;important&quot; está marcado como **bold**.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

La variable `$path` utilizada en el filtro `_path` requiere la ruta de acceso completa al fragmento de contenido (por ejemplo, `/content/dam/wknd/en/magazine/sample-article`).

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

A continuación se muestran varios ejemplos de tipos de respuesta de un campo multilínea denominado `main` que contiene un párrafo: &quot;Este es un párrafo que incluye contenido **important**&quot;. donde &quot;important&quot; está marcado como **bold**.

Ejemplo del HTML ++

**Consulta persistente de GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

Ejemplo de +++Markdown

**Consulta persistente de GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

**Consulta persistente de GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

La opción de procesamiento `plaintext` elimina cualquier formato.

+++


## Procesar una respuesta JSON de texto enriquecido {#render-multiline-json-richtext}

La respuesta JSON de texto enriquecido del campo de varias líneas está estructurada como un árbol jerárquico. Cada objeto o nodo representa un bloque de HTML diferente del texto enriquecido.

A continuación se muestra una respuesta JSON de ejemplo de un campo de texto multilínea. Observe que cada objeto, o nodo, incluye un `nodeType` que representa el bloque de HTML del texto enriquecido como `paragraph`, `link` y `text`. Cada nodo contiene opcionalmente `content`, que es una submatriz que contiene los elementos secundarios del nodo actual.

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

La forma más sencilla de procesar la respuesta de varias líneas `json` es procesar cada objeto o nodo en la respuesta y, a continuación, procesar los elementos secundarios del nodo actual. Se puede utilizar una función recursiva para recorrer el árbol JSON.

A continuación se muestra un ejemplo de código que ilustra un enfoque de recorrido recursivo. Los ejemplos están basados en JavaScript y utilizan [JSX](https://reactjs.org/docs/introducing-jsx.html) de React; sin embargo, los conceptos de programación se pueden aplicar a cualquier lenguaje.

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

`renderNodeList` es una función recursiva que toma una matriz de `childNodes`. Cada nodo de la matriz se pasa a continuación a una función `renderNode`, que a su vez llama a `renderNodeList` si el nodo tiene tareas secundarias.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

La función `renderNode` espera un solo objeto denominado `node`. Un nodo puede tener elementos secundarios que se procesan recursivamente mediante la función `renderNodeList` descrita anteriormente. Finalmente, se usa un `nodeMap` para procesar el contenido del nodo en función de su `nodeType`.

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

`nodeMap` es un literal de objeto JavaScript que se usa como mapa. Cada una de las &quot;claves&quot; representa un(a) `nodeType` diferente. Los parámetros de `node` y `children` se pueden pasar a las funciones resultantes que representan el nodo. El tipo de valor devuelto utilizado en este ejemplo es JSX, pero el método se podría adaptar para crear un literal de cadena que represente el contenido del HTML.

### Ejemplo de código completo

Se puede encontrar una utilidad de representación de texto enriquecido reutilizable en el [ejemplo de WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js): utilidad reutilizable que expone una función `mapJsonRichText`. Esta utilidad la pueden utilizar los componentes que desean procesar una respuesta JSON de texto enriquecido como React JSX.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js): componente de ejemplo que realiza una petición GraphQL que incluye texto enriquecido. El componente utiliza la utilidad `mapJsonRichText` para representar el texto enriquecido y las referencias.


## Agregar referencias en línea a texto enriquecido {#insert-fragment-references}

El campo Multiline permite a los autores insertar imágenes u otros recursos digitales de AEM Assets en el flujo del texto enriquecido.

![insertar imagen](assets/rich-text/insert-image.png)

La captura de pantalla anterior muestra una imagen insertada en el campo multilínea con el botón **Insertar recurso**.

Las referencias a otros fragmentos de contenido también se pueden vincular o insertar en el campo de varias líneas con el botón **Insertar fragmento de contenido**.

![Insertar referencia de fragmento de contenido](assets/rich-text/insert-contentfragment.png)

La captura de pantalla anterior muestra otro fragmento de contenido, la guía definitiva para los parques de patinaje de Los Ángeles, insertándose en el campo de varias líneas. Los tipos de fragmentos de contenido que se pueden insertar en el campo están controlados por la configuración **Modelos de fragmentos de contenido permitidos** en el [tipo de datos multilínea](#multi-line-data-type) del modelo de fragmentos de contenido.

## Consulta de referencias en línea con GraphQL

La API de GraphQL permite a los desarrolladores crear una consulta que incluye propiedades adicionales sobre cualquier referencia insertada en un campo de varias líneas. La respuesta JSON incluye un objeto `_references` independiente que enumera estas propiedades adicionales. La respuesta JSON proporciona a los desarrolladores control total sobre cómo procesar las referencias o los vínculos en lugar de tener que lidiar con un HTML opinado.

Por ejemplo, es posible que desee:

* Incluya lógica de enrutamiento personalizada para administrar vínculos a otros fragmentos de contenido al implementar una aplicación de una sola página, como React Router o Next.js
* AEM Procesar una imagen en línea utilizando la ruta absoluta a un entorno de Publish de la como el valor `src`.
* Determine cómo procesar una referencia incrustada a otro fragmento de contenido con propiedades personalizadas adicionales.

Utilice el tipo de valor devuelto `json` e incluya el objeto `_references` al construir una consulta GraphQL:

**Consulta persistente de GraphQL:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

En la consulta anterior, el campo `main` se devuelve como JSON. El objeto `_references` incluye fragmentos para controlar cualquier referencia de tipo `ImageRef` o de tipo `ArticleModel`.

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
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

La respuesta JSON incluye dónde se insertó la referencia en el texto enriquecido con `"nodeType": "reference"`. A continuación, el objeto `_references` incluye cada referencia.

## Representar referencias en línea en texto enriquecido

Para procesar referencias en línea, se puede expandir el enfoque recursivo explicado en [Procesar una respuesta JSON de varias líneas](#render-multiline-json-richtext).

Donde `nodeMap` es el mapa que procesa los nodos JSON.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

El enfoque de alto nivel es inspeccionar siempre que un(a) `nodeType` sea igual a `reference` en la respuesta JSON de varias líneas. Se puede llamar a una función de procesamiento personalizada que incluya el objeto `_references` devuelto en la respuesta de GraphQL.

La ruta de referencia en línea se puede comparar con la entrada correspondiente del objeto `_references` y se puede llamar a otro mapa personalizado `renderReference`.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

El `__typename` del objeto `_references` se puede usar para asignar diferentes tipos de referencia a diferentes funciones de procesamiento.

### Ejemplo de código completo

Se puede encontrar un ejemplo completo de cómo escribir un procesador de referencias personalizado en [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) como parte del [ejemplo de WKND GraphQL React](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Ejemplo de extremo a extremo

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> El vídeo anterior utiliza `_publishUrl` para procesar la referencia de imagen. En su lugar, prefiera `_dynamicUrl`, tal como se explica en el [procedimiento para imágenes optimizadas para la web](./images.md);


El vídeo anterior muestra un ejemplo completo:

1. Actualización del campo de texto multilínea de un modelo de fragmento de contenido para permitir referencias a fragmento
2. Uso del Editor de fragmentos de contenido para incluir una imagen y hacer referencia a otro fragmento en un campo de texto multilínea.
3. Creando una consulta de GraphQL que incluya la respuesta de texto multilínea como JSON y cualquier `_references` utilizado.
4. SPA Escribir una respuesta de React que procese las referencias en línea de la respuesta de texto enriquecido.

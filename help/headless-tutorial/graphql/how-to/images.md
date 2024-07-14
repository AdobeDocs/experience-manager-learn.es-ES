---
title: AEM Uso de imágenes optimizadas con sin encabezado
description: AEM Obtenga información sobre cómo solicitar direcciones URL de imagen optimizada con sin encabezado de la.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 300
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 4%

---

# Imágenes optimizadas con AEM Headless {#images-with-aem-headless}

AEM Las imágenes son un aspecto crítico de [desarrollar experiencias enriquecidas, convincentes y sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es). AEM Sin encabezado admite la administración de recursos de imagen y su entrega optimizada.

AEM Los fragmentos de contenido utilizados en el modelado de contenido sin encabezado, a menudo hacen referencia a recursos de imagen destinados a su visualización en la experiencia sin encabezado. AEM Se pueden escribir consultas de GraphQL de para proporcionar direcciones URL a imágenes en función de desde dónde se hace referencia a la imagen.

El tipo `ImageRef` tiene cuatro opciones de URL para las referencias de contenido:

+ AEM AEM `_path` es la ruta de acceso a la que se hace referencia en el código de acceso, y no incluye un origen de tipo de acceso (nombre de host) de la dirección de correo electrónico ()
+ `_dynamicUrl` es la dirección URL para la entrega optimizada para la web del recurso de imagen.
   + AEM AEM AEM Publish El `_dynamicUrl` no incluye un origen de, por lo que la aplicación cliente debe proporcionar el dominio (servicio de autor o de).
+ AEM `_authorUrl` es la dirección URL completa del recurso de imagen en el autor de la
   + AEM Se puede usar [Author](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) para proporcionar una experiencia de vista previa de la aplicación sin encabezado.
+ AEM `_publishUrl` es la dirección URL completa del recurso de imagen en Publish de la
   + AEM [Publish](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) es normalmente el lugar desde el que la implementación de producción de la aplicación sin encabezado muestra las imágenes.

`_dynamicUrl` es la dirección URL recomendada que se debe usar para la entrega de recursos de imagen y debe reemplazar el uso de `_path`, `_authorUrl` y `_publishUrl` siempre que sea posible.

|                                | AEM as a Cloud Service | AEM AS A CLOUD SERVICE RDE | AEM SDK de | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| ¿Admite imágenes optimizadas para la web? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Imágenes con AEM Headless"
>abstract="Descubra cómo AEM Headless admite la administración de recursos de imagen y su envío optimizado."

## Modelo de fragmento de contenido

Asegúrese de que el campo Fragmento de contenido que contiene la referencia de imagen sea del tipo de datos __referencia de contenido__.

Los tipos de campo se revisan en el [Modelo de fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), seleccionando el campo e inspeccionando la pestaña __Propiedades__ a la derecha.

![Modelo de fragmento de contenido con referencia de contenido a una imagen](./assets/images/content-fragment-model.jpeg)

## Consulta persistente de GraphQL

En la consulta de GraphQL, devuelva el campo como tipo `ImageRef` y solicite el campo `_dynamicUrl`. Por ejemplo, consultar una aventura en el [proyecto del sitio WKND](https://github.com/adobe/aem-guides-wknd) e incluir la URL de imagen para las referencias del recurso de imagen en su campo `primaryImage`, se puede hacer con una nueva consulta persistente `wknd-shared/adventure-image-by-path` definida como:

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### Variables de consulta

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

La variable `$path` utilizada en el filtro `_path` requiere la ruta de acceso completa al fragmento de contenido (por ejemplo, `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

El `_assetTransform` define cómo se construye el `_dynamicUrl` para optimizar la representación de la imagen servida. Las direcciones URL de imágenes optimizadas para la web también se pueden ajustar en el cliente cambiando los parámetros de consulta de la dirección URL.

| Parámetro de GraphQL | Descripción | Requerido | Valores de variables GraphQL |
|-------------------|------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------|
| `format` | Formato del recurso de imagen. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`, `WEBP`, `WEBPLL`, `WEBPLY` |
| `seoName` | Nombre del segmento de archivo en la dirección URL. Si no se proporciona, se utiliza el nombre del recurso de imagen. | ✘ | Alfanumérico, `-` o `_` |
| `crop` | El fotograma de recorte extraído de la imagen debe tener el tamaño de la imagen | ✘ | Enteros positivos que definen una región de recorte dentro de los límites de las dimensiones de imagen originales |
| `size` | Tamaño de la imagen de salida (tanto altura como anchura) en píxeles. | ✘ | Enteros positivos |
| `rotation` | Rotación de la imagen en grados. | ✘ | `R90`, `R180`, `R270` |
| `flip` | Voltee la imagen. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` |
| `quality` | Calidad de imagen en porcentaje de la calidad original. | ✘ | 1-100 |
| `width` | Ancho de la imagen de salida en píxeles. Cuando se proporciona `size`, se omite `width`. | ✘ | Entero positivo |
| `preferWebP` | AEM Si `true` y el servidor de la aplicación de datos sirve un WebP, si el explorador lo admite, independientemente de `format`. | ✘ | `true`, `false` |


## Respuesta de GraphQL

La respuesta JSON resultante contiene los campos solicitados que contienen la URL optimizada para la web a los recursos de imagen.

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

Para cargar la imagen optimizada para la web de la imagen a la que se hace referencia en su aplicación, utilizó `_dynamicUrl` de `primaryImage` como dirección URL de origen de la imagen.

AEM En React, la visualización de una imagen optimizada para la web desde la aplicación de Publish tiene el siguiente aspecto:

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

AEM Recuerde que `_dynamicUrl` no incluye el dominio de la, por lo que debe proporcionar el origen deseado para que se resuelva la dirección URL de la imagen.

## URL interactivas

El ejemplo anterior muestra el uso de una imagen de un solo tamaño, pero en las experiencias web, a menudo se requieren conjuntos de imágenes adaptables. Las imágenes interactivas se pueden implementar con [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) o [elementos de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). El siguiente fragmento de código muestra cómo utilizar `_dynamicUrl` como base. `width` es un parámetro de URL que puede anexar a `_dynamicUrl` para activar distintas vistas adaptables.

```javascript
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

## Ejemplo de React

Vamos a crear una aplicación React simple que muestre imágenes optimizadas para la web siguiendo [patrones de imagen adaptables](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Existen dos patrones principales para las imágenes adaptables:

+ [Elemento Img con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) para un mayor rendimiento
+ [Elemento de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) para el control de diseño

### Elemento img con srcset

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

Los elementos [Img con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) se utilizan con el atributo `sizes` para proporcionar recursos de imagen diferentes para tamaños de pantalla diferentes. Los flujos de imagen son útiles al proporcionar diferentes recursos de imagen para diferentes tamaños de pantalla.

### Elemento de imagen

[Los elementos de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) se utilizan con varios elementos `source` para proporcionar diferentes recursos de imagen para diferentes tamaños de pantalla. Los elementos de imagen son útiles al proporcionar diferentes representaciones de imagen para diferentes tamaños de pantalla.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Código de ejemplo

AEM AEM Esta sencilla aplicación React usa el [SDK sin encabezado](./aem-headless-sdk.md) para consultar las API sin encabezado de para el contenido de Aventura, y muestra la imagen optimizada para la web usando el [elemento img con srcset](#img-element-with-srcset) y [elemento picture](#picture-element). `srcset` y `sources` utilizan una función `setParams` personalizada para anexar el parámetro de consulta de entrega optimizado para la web a `_dynamicUrl` de la imagen, de modo que se debe cambiar la representación de la imagen entregada según las necesidades del cliente web.

AEM AEM La consulta de la información sobre el acceso se realiza en el vínculo personalizado de React [useAdventureByPath que usa el SDK sin encabezado de la](./aem-headless-sdk.md#graphql-persisted-queries).

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```

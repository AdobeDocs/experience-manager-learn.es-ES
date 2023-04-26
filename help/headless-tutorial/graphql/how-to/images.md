---
title: Uso de imágenes optimizadas con AEM sin encabezado
description: Aprenda a solicitar URL de imagen optimizadas con AEM sin encabezado.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: 97a311e043d3903070cd249d993036b5d88a21dd
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 6%

---

# Imágenes optimizadas con AEM sin encabezado {#images-with-aem-headless}

Las imágenes son un aspecto crítico de [desarrollo de experiencias ricas, atractivas AEM sin objetivos](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es). AEM sin encabezado admite la administración de recursos de imagen y su entrega optimizado.

Los fragmentos de contenido utilizados en AEM modelado de contenido sin encabezado suelen hacer referencia a recursos de imagen que se van a mostrar en la experiencia sin encabezado. AEM las consultas de GraphQL se pueden escribir para proporcionar direcciones URL a las imágenes en función de desde dónde se hace referencia a la imagen.

La variable `ImageRef` El tipo tiene cuatro opciones de URL para las referencias de contenido:

+ `_path` es la ruta a la que se hace referencia en AEM y no incluye un origen AEM (nombre de host)
+ `_dynamicUrl` es la dirección URL completa del recurso de imagen preferido optimizado para la web.
   + La variable `_dynamicUrl` no incluye un origen AEM, por lo que la aplicación cliente debe proporcionar el dominio (AEM Author o AEM Publish service).
+ `_authorUrl` es la dirección URL completa del recurso de imagen en AEM Author
   + [Autor de AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) se puede utilizar para obtener una vista previa de la aplicación sin encabezado.
+ `_publishUrl` es la dirección URL completa del recurso de imagen en AEM Publish
   + [AEM Publish](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) es normalmente desde donde la implementación de producción de la aplicación sin periféricos muestra imágenes.

La variable `_dynamicUrl` es la URL preferida para usar con recursos de imagen y debe reemplazar el uso de `_path`, `_authorUrl`y `_publishUrl` siempre que sea posible.

|  | AEM as a Cloud Service | AEM RDE as a Cloud Service | SDK AEM | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| ¿Admite imágenes optimizadas para la web? | š | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Imágenes con AEM Headless"
>abstract="Descubra cómo AEM Headless admite la administración de recursos de imagen y su envío optimizado."

## Modelo de fragmento de contenido

Asegúrese de que el campo Fragmento de contenido que contiene la referencia de imagen sea del __referencia de contenido__ tipo de datos.

Los tipos de campo se revisan en el [Modelo de fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), seleccionando el campo e inspeccionando el __Propiedades__ a la derecha.

![Modelo de fragmento de contenido con referencia de contenido a una imagen](./assets/images/content-fragment-model.jpeg)

## Consulta persistente de GraphQL

En la consulta de GraphQL, devuelva el campo como `ImageRef` y solicite la `_dynamicUrl` campo . Por ejemplo, para consultar una aventura en la [Proyecto del sitio WKND](https://github.com/adobe/aem-guides-wknd) e incluyendo la URL de imagen para las referencias de recurso de imagen en su `primaryImage` , se puede realizar con una nueva consulta persistente `wknd-shared/adventure-image-by-path` definido como:

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

La variable `$path` se usa en la variable `_path` requiere la ruta completa al fragmento de contenido (por ejemplo `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

La variable `_assetTransform` define cómo la variable `_dynamicUrl` está construido para optimizar la representación de imágenes servidas. Las URL de imágenes optimizadas para web también se pueden ajustar en el cliente cambiando los parámetros de consulta de la URL.

| Parámetro GraphQL | Parámetro de dirección URL | Descripción | Requerido | Valores de variables de GraphQL | Valores de parámetro de URL | Ejemplo de parámetro de URL |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | N/D | Formato del recurso de imagen. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/D | N/D |
| `seoName` | N/D | Nombre del segmento de archivo en la dirección URL. Si no se proporciona, se utiliza el nombre del recurso de imagen. | ✘ | Alfanumérico, `-`o `_` | N/D | N/D |
| `crop` | `crop` | El marco de recorte extraído de la imagen debe estar dentro del tamaño de la imagen | ✘ | Enteros positivos que definen una región de recorte dentro de los límites de las dimensiones de la imagen original | Cadena delimitada por comas de coordenadas numéricas `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | Tamaño de la imagen de salida (altura y anchura) en píxeles. | ✘ | Enteros positivos | Enteros positivos delimitados por comas en el orden `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | Rotación de la imagen en grados. | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | Girar la imagen. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | Calidad de imagen en porcentaje de la calidad original. | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | Anchura de la imagen de salida en píxeles. When `size` se proporciona `width` se ignora. | ✘ | Entero positivo | Entero positivo | `?width=1600` |
| `preferWebP` | `preferwebp` | If `true` y AEM proporciona un WebP si el explorador lo admite, independientemente del `format`. | ✘ | `true`, `false` | `true`, `false` | `?preferwebp=true` |

## Respuesta de GraphQL

La respuesta JSON resultante contiene los campos solicitados que contienen la URL optimizada para la web para los recursos de imagen.

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

Para cargar la imagen optimizada para web de la imagen a la que se hace referencia en la aplicación, utilice la variable `_dynamicUrl` del `primaryImage` como URL de origen de la imagen.

En React, mostrar una imagen optimizada para web desde AEM Publish tiene el siguiente aspecto:

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Recuerde: `_dynamicUrl` no incluye el dominio AEM, por lo que debe proporcionar el origen deseado para que la URL de la imagen se resuelva.

## Direcciones URL adaptables

El ejemplo anterior muestra el uso de una imagen de un solo tamaño; sin embargo, en las experiencias web, a menudo se requieren conjuntos de imágenes interactivas. Las imágenes interactivas se pueden implementar mediante [secuencias de comandos img](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) o [elementos de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). El siguiente fragmento de código muestra cómo usar la variable `_dynamicUrl` como base y añada diferentes parámetros de anchura para potenciar diferentes vistas adaptables. No solo puede `width` utilice el parámetro de consulta , pero el cliente puede agregar otros parámetros de consulta para optimizar aún más el recurso de imagen según sus necesidades.

```javascript
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

## Ejemplo de reacción

Vamos a crear una aplicación React sencilla que muestre las imágenes optimizadas para la web siguientes [patrones de imagen interactivos](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Existen dos patrones principales para las imágenes interactivas:

+ [Elemento de imagen con conjunto de secuencias de comandos](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) para mejorar el rendimiento
+ [Elemento Imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) para control de diseño

### Elemento de imagen con conjunto de secuencias de comandos

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Elementos de imagen con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) se usan con la variable `sizes` para proporcionar distintos recursos de imagen para diferentes tamaños de pantalla. Los conjuntos de cadenas de imagen son útiles cuando se proporcionan distintos recursos de imagen para diferentes tamaños de pantalla.

### Elemento Imagen

[Elementos de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) se utilizan con varios `source` para proporcionar distintos recursos de imagen para diferentes tamaños de pantalla. Los elementos de imagen son útiles cuando se proporcionan distintas representaciones de imagen para diferentes tamaños de pantalla.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Código de ejemplo

Esta sencilla aplicación React usa la variable [SDK AEM sin encabezado](./aem-headless-sdk.md) para consultar AEM API sin encabezado sobre un contenido de Aventura y muestra la imagen optimizada para la web utilizando [elemento img con srcset](#img-element-with-srcset) y [elemento de imagen](#picture-element). La variable `srcset` y `sources` usar un `setParams` para añadir el parámetro de consulta de entrega optimizado para la web al `_dynamicUrl` de la imagen, cambie la representación de la imagen entregada según las necesidades del cliente web.

La consulta con AEM se realiza en el vínculo React personalizado [useAdventureByPath que utiliza el SDK AEM sin encabezado](./aem-headless-sdk.md#graphql-persisted-queries).

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

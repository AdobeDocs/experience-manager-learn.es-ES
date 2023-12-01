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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 6%

---

# Imágenes optimizadas con AEM Headless {#images-with-aem-headless}

Las imágenes son un aspecto crítico de [AEM desarrollo de experiencias enriquecidas, convincentes y sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es). AEM Sin encabezado admite la administración de recursos de imagen y su entrega optimizada.

AEM Los fragmentos de contenido utilizados en el modelado de contenido sin encabezado, a menudo hacen referencia a recursos de imagen destinados a su visualización en la experiencia sin encabezado. AEM Las consultas de GraphQL se pueden escribir para proporcionar direcciones URL a imágenes en función de desde dónde se hace referencia a la imagen.

El `ImageRef` El tipo tiene cuatro opciones de URL para las referencias de contenido:

+ `_path` AEM AEM es la ruta de acceso a la que se hace referencia en el código de tiempo de la y no incluye un origen de tipo (nombre de host)
+ `_dynamicUrl` es la dirección URL completa del recurso de imagen preferido y optimizado para la web.
   + El `_dynamicUrl` AEM AEM AEM no incluye un origen de, por lo que la aplicación cliente debe proporcionar el dominio (servicio de autor o publicación de la).
+ `_authorUrl` AEM es la dirección URL completa del recurso de imagen en el autor de la
   + [AEM Autor de](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) se puede utilizar para proporcionar una experiencia de previsualización de la aplicación sin encabezado.
+ `_publishUrl` AEM es la dirección URL completa del recurso de imagen en la publicación de la
   + [AEM Publicación de](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) suele ser desde donde la implementación de producción de la aplicación sin encabezado muestra las imágenes.

El `_dynamicUrl` es la dirección URL preferida para los recursos de imagen y debe reemplazar el uso de `_path`, `_authorUrl`, y `_publishUrl` siempre que sea posible.

|                                | AEM as a Cloud Service | AEM RDE AS A CLOUD SERVICE | AEM SDK de | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| ¿Admite imágenes optimizadas para la web? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Imágenes con AEM Headless"
>abstract="Descubra cómo AEM Headless admite la administración de recursos de imagen y su envío optimizado."

## Modelo de fragmento de contenido

Asegúrese de que el campo Fragmento de contenido que contiene la referencia de imagen forme parte de la etiqueta __referencia de contenido__ tipo de datos.

Los tipos de campo se revisan en la [Modelo de fragmento de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), seleccionando el campo e inspeccionando el __Propiedades__ a la derecha.

![Modelo de fragmento de contenido con referencia de contenido a una imagen](./assets/images/content-fragment-model.jpeg)

## Consulta persistente de GraphQL

En la consulta de GraphQL, devuelva el campo como. `ImageRef` escriba y solicite el `_dynamicUrl` field. Por ejemplo, consultar una aventura en [Proyecto del sitio WKND](https://github.com/adobe/aem-guides-wknd) e incluir la URL de imagen para las referencias de recursos de imagen en su `primaryImage` , se puede hacer con una nueva consulta persistente `wknd-shared/adventure-image-by-path` definido como:

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

El `$path` utilizada en la variable `_path` El filtro requiere la ruta completa al fragmento de contenido (por ejemplo, `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

El `_assetTransform` define cómo se usa la variable `_dynamicUrl` se crea para optimizar la representación de la imagen servida. Las direcciones URL de imágenes optimizadas para la web también se pueden ajustar en el cliente cambiando los parámetros de consulta de la dirección URL.

| Parámetro de GraphQL | Parámetro de dirección URL | Descripción | Requerido | Valores de variables GraphQL | Valores de parámetro de URL | Ejemplo de parámetro de URL |
|:---------|:----------|:-------------------------------|:--:|:--------------------------|:---|:--|
| `format` | N/D | Formato del recurso de imagen. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`,  `WEBP`, `WEBPLL`, `WEBPLY` | N/D | N/D |
| `seoName` | N/D | Nombre del segmento de archivo en la dirección URL. Si no se proporciona, se utiliza el nombre del recurso de imagen. | ✘ | Alfanumérico, `-`, o `_` | N/D | N/D |
| `crop` | `crop` | El fotograma de recorte extraído de la imagen debe tener el tamaño de la imagen | ✘ | Enteros positivos que definen una región de recorte dentro de los límites de las dimensiones de imagen originales | Cadena delimitada por comas de coordenadas numéricas `<X_ORIGIN>,<Y_ORIGIN>,<CROP_WIDTH>,<CROP_HEIGHT>` | `?crop=10,20,300,400` |
| `size` | `size` | Tamaño de la imagen de salida (tanto altura como anchura) en píxeles. | ✘ | Enteros positivos | Enteros positivos delimitados por comas en el orden `<WIDTH>,<HEIGHT>` | `?size=1200,800` |
| `rotation` | `rotate` | Rotación de la imagen en grados. | ✘ | `R90`, `R180`, `R270` | `90`, `180`, `270` | `?rotate=90` |
| `flip` | `flip` | Voltee la imagen. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` | `h`, `v`, `hv` | `?flip=h` |
| `quality` | `quality` | Calidad de imagen en porcentaje de la calidad original. | ✘ | 1-100 | 1-100 | `?quality=80` |
| `width` | `width` | Ancho de la imagen de salida en píxeles. Cuándo `size` se proporciona `width` se ignora. | ✘ | Entero positivo | Entero positivo | `?width=1600` |
| `preferWebP` | `preferwebp` | If `true` AEM y sirve un WebP si el explorador lo admite, independientemente de la variable `format`. | ✘ | `true`, `false` | `true`, `false` | `?preferwebp=true` |

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

Para cargar la imagen optimizada para la web de la imagen a la que se hace referencia en la aplicación, se utiliza el `_dynamicUrl` de la `primaryImage` como URL de origen de la imagen.

AEM En React, la visualización de una imagen optimizada para la web desde la publicación de la página web tiene el siguiente aspecto:

```jsx
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Recuerde, `_dynamicUrl` AEM no incluye el dominio de, por lo que debe proporcionar el origen deseado para que se resuelva la dirección URL de la imagen.

## URL interactivas

El ejemplo anterior muestra el uso de una imagen de un solo tamaño, pero en las experiencias web, a menudo se requieren conjuntos de imágenes adaptables. Las imágenes interactivas se pueden implementar mediante [srcsets de img](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) o [elementos de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). El siguiente fragmento de código muestra cómo utilizar la variable `_dynamicUrl` como base, y anexar diferentes parámetros de anchura, para activar diferentes vistas adaptables. No solo pueden `width` se puede utilizar el parámetro de consulta, pero el cliente puede agregar otros parámetros de consulta para optimizar aún más el recurso de imagen según sus necesidades.

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

## Ejemplo de React

Vamos a crear una aplicación React simple que muestre imágenes optimizadas para la web a continuación [patrones de imagen adaptables](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/). Existen dos patrones principales para las imágenes adaptables:

+ [Elemento img con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) para un rendimiento mejorado
+ [Elemento de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) para el control de diseño

### Elemento img con srcset

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[Elementos img con srcset](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) se utilizan con la variable `sizes` para proporcionar diferentes recursos de imagen para diferentes tamaños de pantalla. Los flujos de imagen son útiles al proporcionar diferentes recursos de imagen para diferentes tamaños de pantalla.

### Elemento de imagen

[Elementos de imagen](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) se utilizan con varios `source` para proporcionar diferentes recursos de imagen para diferentes tamaños de pantalla. Los elementos de imagen son útiles al proporcionar diferentes representaciones de imagen para diferentes tamaños de pantalla.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Código de ejemplo

Esta aplicación React simple usa el [AEM SDK sin encabezado de](./aem-headless-sdk.md) AEM para consultar las API sin encabezado para obtener contenido de Aventura, y muestra la imagen optimizada para la web mediante [elemento img con srcset](#img-element-with-srcset) y [elemento de imagen](#picture-element). El `srcset` y `sources` usar un personalizado `setParams` para anexar el parámetro de consulta de envío optimizado para la web al `_dynamicUrl` de la imagen, por lo que debe cambiar la representación de la imagen entregada según las necesidades del cliente web.

AEM La consulta de la información sobre la se realiza en el vínculo React personalizado [AEM useAdventureByPath que utiliza el SDK sin encabezado de la](./aem-headless-sdk.md#graphql-persisted-queries).

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

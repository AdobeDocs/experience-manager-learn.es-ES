---
title: 'Aplicación de iOS: ejemplo sin encabezado de AEM'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación para iOS muestra cómo consultar contenido mediante las API de GraphQL de AEM utilizando consultas persistentes.
version: Experience Manager as a Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# aplicación de iOS

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación para iOS muestra cómo consultar contenido mediante las API de GraphQL de AEM utilizando consultas persistentes.

![Aplicación SwiftUI de iOS con AEM Headless](./assets/ios-swiftui-app/ios-app.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Xcode](https://developer.apple.com/xcode/) (requiere macOS)
+ [Git](https://git-scm.com/)

## Requisitos de AEM

La aplicación de iOS funciona con las siguientes opciones de implementación de AEM. Todas las implementaciones requieren que esté instalado [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=es)
+ Configuración local mediante [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)

La aplicación de iOS está diseñada para conectarse a un entorno de __AEM Publish__, pero puede obtener contenido de AEM Author si se proporciona autenticación en la configuración de la aplicación de iOS.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra [Xcode](https://developer.apple.com/xcode/) y abra la carpeta `ios-app`
1. Modifique el archivo `Config.xcconfig` y actualice `AEM_SCHEME` y `AEM_HOST` para que coincida con el servicio de publicación de AEM de destino.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Si se conecta a AEM Author, agregue `AEM_AUTH_TYPE` y las propiedades de autenticación de compatibilidad a `Config.xcconfig`.

   __Autenticación básica__

   `AEM_USERNAME` y `AEM_PASSWORD` autentican a un usuario local de AEM con acceso al contenido de WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticación de token__

   `AEM_TOKEN` es un [token de acceso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=es) que se autentica ante un usuario de AEM con acceso al contenido de WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Cree la aplicación mediante Xcode e impleméntelo en el simulador de iOS.
1. Se debe mostrar una lista de las aventuras del sitio WKND en la aplicación. Al seleccionar una aventura, se abren los detalles de la aventura. En la vista de lista de aventuras, extraiga para actualizar los datos de AEM.

## El código

A continuación se muestra un resumen de cómo se crea la aplicación de iOS, cómo se conecta a AEM sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Consultas persistentes

Siguiendo las prácticas recomendadas de AEM sin encabezado, la aplicación iOS utiliza consultas persistentes de AEM GraphQL para consultar datos de aventura. La aplicación utiliza dos consultas persistentes:

+ `wknd/adventures-all` persistió en la consulta, lo que devuelve todas las aventuras en AEM con un conjunto abreviado de propiedades. Esta consulta persistente genera la lista de aventuras de la vista inicial.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` persistió en la consulta, lo que devuelve una sola aventura de `slug` (una propiedad personalizada que identifica de forma exclusiva una aventura) con un conjunto completo de propiedades. Esta consulta persistente activa las vistas de detalles de aventura.

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### Ejecutar consulta persistente de GraphQL

Las consultas persistentes de AEM se ejecutan a través de HTTP GET y, por lo tanto, no se pueden utilizar las bibliotecas comunes de GraphQL que utilizan HTTP POST como Apollo. En su lugar, cree una clase personalizada que ejecute las solicitudes HTTP GET de consulta persistentes a AEM.

`AEM/Aem.swift` crea una instancia de la clase `Aem` utilizada para todas las interacciones con AEM Headless. El patrón es:

1. Cada consulta persistente tiene una función pública correspondiente (por ejemplo, `getAdventures(..)` o `getAdventureBySlug(..)`) las vistas de la aplicación iOS invocan para obtener datos de aventura.
1. La función pública llama a una función privada `makeRequest(..)` que invoca una solicitud HTTP GET asincrónica a AEM Headless y devuelve los datos JSON.
1. A continuación, cada función pública descodifica los datos JSON y realiza las comprobaciones o transformaciones necesarias antes de devolver los datos de Aventura a la vista.

   + Los datos JSON de GraphQL de AEM se descodifican mediante las estructuras y las clases definidas en `AEM/Models.swift`, que se asignan a los objetos JSON devueltos por mi AEM Headless.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }
    
    ...

    /// #makeRequest(..)
    /// Generic method for constructing and executing AEM GraphQL persisted queries
    private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
        // Encode optional parameters as required by AEM
        let persistedQueryParams = params.map { (param) -> String in
            encode(string: ";\(param.key)=\(param.value)")
        }.joined(separator: "")
        
        // Construct the AEM GraphQL persisted query URL, including optional query params
        let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

        var request = URLRequest(url: URL(string: url)!);

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### Modelos de datos de respuesta de GraphQL

iOS prefiere asignar objetos JSON a modelos de datos con tipo.

`src/AEM/Models.swift` define las [estructuras y clases descodificables](https://developer.apple.com/documentation/swift/decodable) de Swift que se asignan a las respuestas JSON de AEM devueltas por las respuestas JSON de AEM.

### Vistas

La interfaz de usuario de Swift se utiliza para las distintas vistas de la aplicación. Apple proporciona un tutorial de introducción para [crear listas y navegar con SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  La entrada de la aplicación e incluye `AdventureListView` cuyo controlador de eventos `.onAppear` se usa para recuperar todos los datos de aventuras a través de `aem.getAdventures()`. El objeto `aem` compartido se inicializó aquí y se expuso a otras vistas como [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Muestra una lista de aventuras (basada en los datos de `aem.getAdventures()`) y muestra un elemento de lista para cada aventura que use `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Muestra todos los elementos de la lista de aventuras (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Muestra los detalles de una aventura, incluido el título, la descripción, el precio, el tipo de actividad y la imagen principal. Esta vista consulta a AEM para obtener detalles completos sobre la aventura mediante `aem.getAdventureBySlug(slug: slug)`, donde se pasa el parámetro `slug` en función de la fila de la lista de selección.

### Imágenes remotas

Las imágenes a las que hacen referencia los fragmentos de contenido de aventura las proporciona AEM. Esta aplicación de iOS usa el campo de ruta de acceso `_dynamicUrl` en la respuesta de GraphQL y prefija `AEM_SCHEME` y `AEM_HOST` para crear una dirección URL completa. Si se desarrolla con AEM SDK, `_dynamicUrl` devuelve nulo, por lo que para el desarrollo se debe volver al campo `_path` de la imagen.

Si se conecta a recursos protegidos en AEM que requieren autorización, también se deben agregar credenciales a las solicitudes de imagen.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) y [SDWebImage](https://github.com/SDWebImage/SDWebImage) se utilizan para cargar las imágenes remotas desde AEM que rellenan la imagen Adventure en las vistas `AdventureListItemView` y `AdventureDetailView`.

La clase `aem` (en `AEM/Aem.swift`) facilita el uso de imágenes de AEM de dos maneras:

1. `aem.imageUrl(path: String)` se usa en las vistas para anteponer el esquema y el host de AEM a la ruta de la imagen y crear una dirección URL completa.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. El `convenience init(..)` de `Aem` estableció encabezados de autorización HTTP en la solicitud HTTP de imagen, según la configuración de las aplicaciones de iOS.

   + Si se ha configurado __autenticación básica__, la autenticación básica se adjuntará a todas las solicitudes de imagen.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + Si se ha configurado la autenticación de __token__, la autenticación de token se adjuntará a todas las solicitudes de imagen.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + Si __no se ha configurado ninguna autenticación__, no se adjuntará ninguna autenticación a las solicitudes de imagen.

Se puede usar un enfoque similar con [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) nativo de SwiftUI. `AsyncImage` es compatible con iOS 15.0+.

## Recursos adicionales

+ [Introducción a AEM Headless: tutorial de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es)
+ [Listas de SwiftUI y tutorial de navegación](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)

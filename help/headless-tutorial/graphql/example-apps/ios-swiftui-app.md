---
title: 'Aplicación de iOS AEM: ejemplo sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). Esta aplicación para iOS AEM muestra cómo consultar contenido mediante las API de GraphQL de uso de consultas persistentes de la aplicación de datos de usuario de.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM sin encabezado as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# aplicación de iOS

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). Esta aplicación para iOS AEM muestra cómo consultar contenido mediante las API de GraphQL de uso de consultas persistentes de la aplicación de datos de usuario de.

![Aplicación SwiftUI de iOS AEM con encabezado sin encabezado de la](./assets/ios-swiftui-app/ios-app.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Xcode](https://developer.apple.com/xcode/) (requiere macOS)
+ [Git](https://git-scm.com/)

## AEM requisitos de

La aplicación de iOS AEM funciona con las siguientes opciones de implementación de. Todas las implementaciones requieren que esté instalado [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)

La aplicación de iOS AEM está diseñada para conectarse a un entorno __Publish AEM__, pero puede obtener contenido de la fuente de Autor de la aplicación o si se proporciona autenticación en la configuración de la aplicación de iOS.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra [Xcode](https://developer.apple.com/xcode/) y abra la carpeta `ios-app`
1. AEM Modifique el archivo `Config.xcconfig` y actualice `AEM_SCHEME` y `AEM_HOST` para que coincida con el destino que tiene el servicio de Publish de la.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   AEM Si se conecta con el autor de la, agregue `AEM_AUTH_TYPE` y las propiedades de autenticación de compatibilidad a `Config.xcconfig`.

   __Autenticación básica__

   AEM `AEM_USERNAME` y `AEM_PASSWORD` autentican a un usuario local de la aplicación con acceso al contenido de WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticación de token__

   AEM `AEM_TOKEN` es un [token de acceso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) que se autentica ante un usuario de con acceso al contenido de WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Cree la aplicación mediante Xcode e impleméntelo en el simulador de iOS.
1. Se debe mostrar una lista de las aventuras del sitio WKND en la aplicación. Al seleccionar una aventura, se abren los detalles de la aventura. AEM En la vista de lista de aventuras, extraiga para actualizar los datos de la vista de.

## El código

A continuación se muestra un resumen de cómo se crea la aplicación de iOS AEM, cómo se conecta a las consultas sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Consultas persistentes

AEM Siguiendo las prácticas recomendadas de las consultas sin encabezado, la aplicación de iOS AEM utiliza consultas persistentes de GraphQL para consultar datos de aventuras, de forma predeterminada, de la manera más sencilla. La aplicación utiliza dos consultas persistentes:

+ AEM `wknd/adventures-all` persistió en la consulta, lo que devuelve todas las aventuras en la que se ha realizado un seguimiento con un conjunto abreviado de propiedades. Esta consulta persistente genera la lista de aventuras de la vista inicial.

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

AEM Las consultas persistentes de los se ejecutan a través de una GET HTTP y, por lo tanto, no se pueden utilizar las bibliotecas comunes de GraphQL que utilizan un POST HTTP como Apollo. En su lugar, cree una clase personalizada que ejecute las solicitudes de GET AEM HTTP de consulta persistentes a la dirección de correo electrónico de la dirección de correo electrónico

AEM `AEM/Aem.swift` crea una instancia de la clase `Aem` utilizada para todas las interacciones con el sin encabezado de la. El patrón es:

1. Cada consulta persistente tiene una función pública correspondiente (por ejemplo, `getAdventures(..)` o `getAdventureBySlug(..)`) las vistas de la aplicación iOS invocan para obtener datos de aventura.
1. La función pública llama a una función privada `makeRequest(..)` que invoca una petición HTTP asincrónica de GET AEM a la red sin encabezado y devuelve los datos JSON.
1. A continuación, cada función pública descodifica los datos JSON y realiza las comprobaciones o transformaciones necesarias antes de devolver los datos de Aventura a la vista.

   + AEM datos JSON de GraphQL AEM se descodifican mediante las estructuras o las clases definidas en `AEM/Models.swift`, que se asignan a los objetos JSON devueltos por mi sistema sin encabezado.

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

AEM AEM El objeto `src/AEM/Models.swift` define las estructuras y clases de Swift [decodificables](https://developer.apple.com/documentation/swift/decodable) que se asignan a las respuestas de JSON de la devueltas por las respuestas de JSON de la aplicación de la aplicación en la que se.

### Vistas

La interfaz de usuario de Swift se utiliza para las distintas vistas de la aplicación. Apple proporciona un tutorial de introducción para [crear listas y navegar con SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  La entrada de la aplicación e incluye `AdventureListView` cuyo controlador de eventos `.onAppear` se usa para recuperar todos los datos de aventuras a través de `aem.getAdventures()`. El objeto `aem` compartido se inicializó aquí y se expuso a otras vistas como [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Muestra una lista de aventuras (basada en los datos de `aem.getAdventures()`) y muestra un elemento de lista para cada aventura que use `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Muestra todos los elementos de la lista de aventuras (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Muestra los detalles de una aventura, incluido el título, la descripción, el precio, el tipo de actividad y la imagen principal. AEM Esta vista consulta los detalles completos de la aventura usando `aem.getAdventureBySlug(slug: slug)`, donde el parámetro `slug` se pasa en función de la fila de la lista de selección.

### Imágenes remotas

AEM Las imágenes a las que hacen referencia los fragmentos de contenido de aventura las proporciona el usuario de forma. Esta aplicación de iOS usa el campo de ruta de acceso `_dynamicUrl` en la respuesta de GraphQL y prefija `AEM_SCHEME` y `AEM_HOST` para crear una dirección URL completa. Si se desarrolla con el SDK de AEM, `_dynamicUrl` devuelve nulo, por lo que para el desarrollo se debe volver al campo `_path` de la imagen.

AEM Si la conexión a recursos protegidos en un entorno que requiere autorización, también se deben agregar credenciales a las solicitudes de imagen.

AEM [SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) y [SDWebImage](https://github.com/SDWebImage/SDWebImage) se utilizan para cargar las imágenes remotas de los grupos de trabajo que rellenan la imagen de Aventura en las vistas `AdventureListItemView` y `AdventureDetailView`.

AEM La clase `aem` (en `AEM/Aem.swift`) facilita el uso de imágenes de dos maneras:

1. AEM `aem.imageUrl(path: String)` se usa en las vistas para anteponer el esquema de la y el host a la ruta de acceso de la imagen, lo que crea una dirección URL completa.

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

+ AEM [Tutorial de introducción a la sin encabezado de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es)
+ [Listas de SwiftUI y tutorial de navegación](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)

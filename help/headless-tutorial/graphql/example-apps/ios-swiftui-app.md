---
title: 'Aplicación iOS: ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación de iOS muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '981'
ht-degree: 3%

---

# aplicación iOS

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación de iOS muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes.

![Aplicación iOS SwiftUI con AEM sin encabezado](./assets/ios-swiftui-app/ios-app.png)

Consulte la [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Xcode 9.3+](https://developer.apple.com/xcode/) (requiere macOS)
+ [Git](https://git-scm.com/)

## AEM requisitos

La aplicación iOS funciona con las siguientes opciones de implementación de AEM. Todas las implementaciones requieren la variable [Sitio WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) para instalar.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)
+ [Inicio rápido de AEM 6.5 SP13+](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es?lang=en#install-local-aem-instances)

La aplicación iOS está diseñada para conectarse a un __AEM Publish__ , sin embargo, puede obtener contenido de AEM Author si la autenticación se proporciona en la configuración de la aplicación de iOS.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) y abra la carpeta `ios-app`
1. Modificación del archivo `Config.xcconfig` archivo y actualizar `AEM_SCHEME` y `AEM_HOST` para que coincida con el servicio de publicación de AEM de Target.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = http
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   ```

   Si se conecta a AEM Author, agregue la variable `AEM_AUTH_TYPE` y las propiedades de autenticación compatibles con `Config.xcconfig`.

   __Autenticación básica__

   La variable `AEM_USERNAME` y `AEM_PASSWORD` autentique a un usuario AEM local con acceso al contenido de WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Autenticación de tokens__

   La variable `AEM_TOKEN` es un [token de acceso](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) que se autentica a un usuario AEM con acceso al contenido de WKND GraphQL.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. Cree la aplicación utilizando Xcode e implemente la aplicación en el simulador de iOS.
1. Se debe mostrar una lista de las aventuras del sitio WKND en la aplicación. Al seleccionar una aventura, se abren los detalles de la aventura. En la vista de lista de aventuras, presione para actualizar los datos de AEM.

## El código

A continuación se muestra un resumen de cómo se crea la aplicación de iOS, cómo se conecta a AEM sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

### Consultas persistentes

Siguiendo AEM prácticas recomendadas sin encabezado, la aplicación de iOS utiliza consultas persistentes AEM GraphQL para consultar datos de aventura. La aplicación utiliza dos consultas persistentes:

+ `wknd/adventures-all` consulta persistente, que devuelve todas las aventuras en AEM con un conjunto abreviado de propiedades. Esta consulta persistente impulsa la lista de aventuras de la vista inicial.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que devuelve una sola aventura de `slug` (una propiedad personalizada que identifica de forma exclusiva una aventura) con un conjunto completo de propiedades. Esta consulta persistente potencia las vistas de detalles de aventura.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

AEM las consultas persistentes se ejecutan a través de GET HTTP y, por lo tanto, no se pueden utilizar las bibliotecas comunes de GraphQL que utilizan POST HTTP como Apollo. En su lugar, cree una clase personalizada que ejecute las solicitudes de GET HTTP de consulta persistentes a AEM.

`AEM/Aem.swift` crea una instancia de `Aem` clase utilizada para todas las interacciones con AEM sin encabezado. El patrón es:

1. Cada consulta persistente tiene una función pública correspondiente (por ejemplo, `getAdventures(..)` o `getAdventureBySlug(..)`) las vistas de la aplicación de iOS se invocan para obtener datos de aventura.
1. El func público llama a un func privado `makeRequest(..)` que invoca una solicitud de GET HTTP asincrónica para AEM sin encabezado y devuelve los datos JSON.
1. A continuación, cada func pública descodifica los datos JSON y realiza las comprobaciones o transformaciones necesarias antes de devolver los datos de Aventura a la vista.

   + AEM datos JSON de GraphQL se descodifican usando las estructuras o clases definidas en `AEM/Models.swift`, que se asignaban a los objetos JSON devolvía mi AEM sin encabezado.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(completion: @escaping ([Adventure]) ->  ()) {
               
        // Create the HTTP request object representing the persisted query to get all adventures
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all")
        
        // Wait fo the HTTP request to return
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            // Error check as needed
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            }
                                    
            if (!data!.isEmpty) {
                // Decode the JSON data into Swift objects
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                
                DispatchQueue.main.async {
                    // Return the array of Adventure objects
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

La variable `src/AEM/Models.swift` define el [descodificable](https://developer.apple.com/documentation/swift/decodable) Estructuras y clases de Swift que se asignan a las AEM respuestas JSON devueltas por AEM respuestas JSON.

### Vistas

SwiftUI se utiliza para las distintas vistas de la aplicación. Apple proporciona un tutorial de introducción para [creación de listas y navegación con SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

   La entrada de la aplicación e incluye `AdventureListView` whose `.onAppear` el controlador de eventos se utiliza para recuperar todos los datos de aventuras mediante `aem.getAdventures()`. El `aem` se inicializa aquí y se expone a otras vistas como un [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

   Muestra una lista de aventuras (según los datos de `aem.getAdventures()`) y muestra un elemento de lista para cada aventura utilizando la variable `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

   Muestra cada elemento de la lista de aventuras (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

   Muestra los detalles de una aventura, incluidos el título, la descripción, el precio, el tipo de actividad y la imagen principal. Esta vista consulta AEM para obtener detalles de aventura completos mediante `aem.getAdventureBySlug(slug: slug)`, donde la variable `slug` se transfiere en función de la fila de lista de selección.

### Imágenes remotas

Las imágenes a las que se hace referencia en los fragmentos de contenido de aventura son proporcionadas por AEM. Esta aplicación de iOS utiliza la ruta `_path` en la respuesta de GraphQL y añade los prefijos a la variable `AEM_SCHEME` y `AEM_HOST` para crear una URL completa.

Si la conexión a recursos protegidos en AEM que requiere autorización, también se deben agregar credenciales a solicitudes de imagen.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) y [SDWebImage](https://github.com/SDWebImage/SDWebImage) se utilizan para cargar las imágenes remotas de AEM que rellenan la imagen de aventura en la variable `AdventureListItemView` y `AdventureDetailView` vistas.

La variable `aem` clase (en `AEM/Aem.swift`) facilita el uso de imágenes AEM de dos maneras:

1. `aem.imageUrl(path: String)` se utiliza en las vistas para anteponer el esquema de AEM y alojar la ruta de la imagen, creando una URL completa.

   ```swift
   // adventure.image() => /content/dam/path/to/an/image.png
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => http://localhost:4503/content/dam/path/to/an/image.png
   ```

2. La variable `convenience init(..)` en `Aem` establezca encabezados de autorización HTTP en la solicitud HTTP de imagen, según la configuración de las aplicaciones iOS.

   + If __autenticación básica__ está configurado, la autenticación básica se adjunta a todas las solicitudes de imagen.

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

   + If __autenticación de token__ está configurado, la autenticación de token se adjunta a todas las solicitudes de imagen.

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

   + If __sin autenticación__ está configurado y, a continuación, no se adjunta ninguna autenticación a las solicitudes de imagen.



Se puede usar un enfoque similar con el nativo de la interfaz de usuario de Swift [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` es compatible con iOS 15.0+.

## Recursos adicionales

+ [Introducción a AEM sin encabezado: Tutorial de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Tutorial sobre listas y navegación de SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)

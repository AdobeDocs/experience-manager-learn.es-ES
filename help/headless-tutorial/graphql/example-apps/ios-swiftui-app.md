---
title: 'Aplicación SwiftUI de iOS: ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Se proporciona una aplicación de iOS que muestra cómo consultar contenido mediante las API de GraphQL de AEM. Apollo Client iOS se utiliza para generar consultas de GraphQL y asignar datos a objetos de Swift para impulsar la aplicación. SwiftUI se utiliza para procesar una lista sencilla y una vista detallada del contenido.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '960'
ht-degree: 2%

---


# Aplicación SwiftUI de iOS

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación de iOS muestra cómo consultar contenido mediante las API de GraphQL de AEM. Apollo Client iOS se utiliza para generar consultas de GraphQL y asignar datos a objetos de Swift para impulsar la aplicación. SwiftUI se utiliza para procesar una lista sencilla y una vista detallada del contenido.

Consulte la [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app)

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

* [Xcode 9.3+](https://developer.apple.com/xcode/)
* [Git](https://git-scm.com/)

## Requisitos AEM

La aplicación está diseñada para conectarse a un AEM **Publicación** con la última versión de [Sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) instalado.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=es#service-pack)

Recomendamos [implementación del sitio de referencia WKND en un entorno de Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Una configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) o [6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) también se puede utilizar.

## Utilización

1. Clonar el `aem-guides-wknd-graphql` repositorio:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Xcode](https://developer.apple.com/xcode/) y abra la carpeta `ios-swiftui-app`
1. Modificación del archivo `Config.xcconfig` archivo y actualizar `AEM_HOST` para que coincida con el entorno de publicación de AEM de destino

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. Cree la aplicación utilizando Xcode e implemente la aplicación en el simulador de iOS.
1. Se debe mostrar en la aplicación una lista de las aventuras del sitio de referencia WKND.

## El código

A continuación se muestra un breve resumen de los archivos y el código importantes utilizados para activar la aplicación. El código completo se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### Apollo iOS

La variable [Apollo iOS](https://www.apollographql.com/docs/ios/) La aplicación utiliza al cliente para ejecutar la consulta de GraphQL con AEM. El oficial [Tutorial de Apollo](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) tiene muchos más detalles sobre cómo instalar y utilizar.

`schema.json` es un archivo que representa el esquema de GraphQL de un entorno AEM con el sitio de referencia WKND instalado. `schema.json` se descargó de AEM y se agregó al proyecto. El cliente Apollo inspecciona cualquier archivo con la extensión `.graphql` como parte de una fase de compilación personalizada. A continuación, el cliente Apollo utiliza la variable `schema.json` y `.graphql` consultas para generar automáticamente el archivo `API.swift`.

Esto proporciona a la aplicación un modelo con establecimiento inflexible de tipos para ejecutar la consulta y los modelos que representan los resultados.

![Fase de compilación personalizada de Xcode](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` contiene la consulta utilizada para consultar las aventuras:

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` construye el `ApolloClient`. La variable `endpointURL` se construye leyendo los valores de la variable `Config.xcconfig` archivo. Si desea conectarse a un AEM **Autor** instancia y necesaria para añadir encabezados adicionales para la autenticación, es posible que desee modificar la variable `ApolloClient` aquí.

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### Datos de aventura

La aplicación está diseñada para mostrar una lista de aventuras y luego una vista detallada de cada aventura.

`AdventuresDataModel.swift` es una clase que incluye una función `fetchAdventures()`. Esta función utiliza la variable `ApolloClient` para ejecutar la consulta. En una consulta correcta, la matriz de resultados será del tipo `AdventureListQuery.Data.AdventureList.Item`, generado automáticamente por el `API.swift` archivo.

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

Es posible utilizar `AdventureListQuery.Data.AdventureList.Item` directamente para activar la aplicación. Sin embargo, es muy posible que algunos de los datos estén incompletos y, por lo tanto, algunas propiedades pueden ser nulas.

`Adventure.swift` es un modelo personalizado introducido actúa como envolvente del modelo generado por Apollo. `Adventure` se inicializa con `AdventureListQuery.Data.AdventureList.Item`. A `typealias` se utiliza para abreviar para que el código sea más legible:

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

La variable `Adventure` struct se inicializa con un `AdventureData` objeto:

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

Esto nos permite proporcionar valores predeterminados y realizar comprobaciones adicionales de datos incompletos. Entonces podemos usar la variable `Adventure` modelo de forma segura para activar varios elementos de IU y no es necesario comprobar constantemente los valores nulos.

En AEM, los fragmentos de contenido se identifican de forma única mediante `_path`. En `Adventure.swift` rellenamos el `id` propiedad con el valor de `_path`. Esto permite que `Adventure` para implementar la variable `Identifiable` y facilita la iteración en una matriz o lista.

### Vistas

SwiftUI se utiliza para las distintas vistas de la aplicación. Un bueno tutorial para [creación de listas y navegación](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) se encuentra en el sitio para desarrolladores de Apple. El código de esta aplicación se deriva de él de forma flexible.

`WKNDAdventuresApp.swift` es la entrada de la aplicación. Incluye `AdventureListView` y `.onAppear` se utiliza para recuperar los datos de aventura.

`AdventureListView.swift` - crea un `NavigationView` y una lista de aventuras completadas por `AdventureRowView`. Navegación a un `AdventureDetailView` está configurado aquí.

`AdventureRowView` - muestra la imagen principal de la aventura y el título de la aventura en una fila.

`AdventureDetailView` - muestra un detalle completo de la aventura individual incluyendo el título, descripción, precio, tipo de actividad e imagen principal.

Cuando se ejecuta y vuelve a generar la CLI de Apollo `API.swift` hace que se detenga la vista previa. Para utilizar la función de vista previa automática, deberá actualizar la variable **CLI de Apollo** Fase de compilación y marque para ejecutar el script **Solo para compilaciones de instalación**.

![Comprobar solo las compilaciones de instalación](assets/ios-swiftui-app/update-build-phases.png)

### Imágenes remotas

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) y [SDWEbImage](https://github.com/SDWebImage/SDWebImage) se utilizan para cargar las imágenes remotas de AEM que rellenan la imagen principal de Aventura en las vistas Fila y Detalle.

La variable [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) es una vista nativa de SwiftUI que también se podría usar. `AsyncImage` solo es compatible con iOS 15.0 o posterior.

## Recursos adicionales

* [Introducción a AEM sin encabezado: Tutorial de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [Tutorial sobre listas y navegación de SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Tutorial del cliente de Apollo iOS](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)


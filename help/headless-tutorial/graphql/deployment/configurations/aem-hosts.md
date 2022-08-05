---
title: Administración de hosts de AEM para AEM GraphQL
description: Obtenga información sobre cómo configurar AEM hosts en AEM aplicación sin encabezado.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# Administración de hosts AEM

La implementación de una aplicación AEM sin encabezado requiere atención sobre cómo se construyen AEM direcciones URL para garantizar el uso correcto AEM host/dominio. Los tipos de solicitud/URL principales que deben tenerse en cuenta son:

+ Solicitudes HTTP a __[API de AEM GraphQL](#aem-graphql-api-requests)__
+ __[Direcciones URL de imágenes](#aem-image-urls)__ a los recursos de imagen a los que se hace referencia en los fragmentos de contenido y que AEM entrega

Normalmente, una aplicación sin encabezado AEM interactúa con un único servicio de AEM tanto para la API de GraphQL como para las solicitudes de imagen. El servicio AEM cambia en función de la implementación de la aplicación sin encabezado de AEM:

| AEM tipo de implementación sin encabezado | Entorno AEM | Servicio AEM |
|-------------------------------|:---------------------:|:----------------:|
| Producción | Producción | Publicación |
| Creación de vista previa | Producción | Vista previa |
| Desarrollo | Desarrollo | Publicación |

Para gestionar las permutaciones de tipo de implementación, cada implementación de aplicación se crea mediante una configuración que especifica el servicio de AEM al que se va a conectar. El host/dominio del servicio de AEM configurado se utiliza para construir las URL de la API de GraphQL de AEM y las URL de imagen. Para determinar el método correcto para administrar las configuraciones dependientes de la compilación, consulte la documentación del marco de la aplicación AEM sin encabezado (por ejemplo, React, iOS, Android™, etc.), ya que el enfoque varía según el marco de trabajo.

| Tipo de cliente | [Aplicación de una sola página (SPA)](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuración de hosts AEM | š | š | š | š |

A continuación se muestran algunos ejemplos de posibles métodos para construir direcciones URL para [API de AEM GraphQL](#aem-graphql-api-requests) y [solicitudes de imagen](#aem-image-requests), para varias plataformas y marcos populares sin encabezado.

## Solicitudes de API de AEM GraphQL

Las solicitudes de GET HTTP de la aplicación sin encabezado a AEM API de GraphQL deben configurarse para interactuar con el servicio de AEM correcto, tal como se describe en la sección [tabla anterior](#managing-aem-hosts).

Al usar [SDK AEM sin encabezado](../../how-to/aem-headless-sdk.md) (disponible para JavaScript basado en explorador, JavaScript basado en servidor y Java™), un host de AEM puede inicializar el objeto cliente sin encabezado de AEM con el servicio de AEM con el que conectarse.

Al desarrollar un cliente sin encabezado AEM personalizado, asegúrese de que el host del servicio de AEM sea parametrizable en función de los parámetros de compilación.

### Ejemplos

A continuación se muestran ejemplos de cómo las solicitudes de API de GraphQL de AEM pueden tener el valor de host AEM configurado para varios marcos de aplicaciones sin encabezado.

+++ Ejemplo de reacción

Este ejemplo, basado en la variable [AEM aplicación React sin encabezado](../../example-apps/react-app.md), ilustra cómo se pueden configurar AEM solicitudes de API de GraphQL para conectarse a diferentes AEM Services en función de variables de entorno.

Las aplicaciones de React deben usar la variable [AEM cliente sin encabezado para JavaScript](../../how-to/aem-headless-sdk.md) para interactuar con AEM API de GraphQL. El cliente AEM sin encabezado, proporcionado por el cliente AEM sin encabezado para JavaScript, debe inicializarse con el host del servicio de AEM al que se conecta.

#### React environment file

React uses [archivos de entorno personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/)o `.env` , almacenados en la raíz del proyecto para definir valores específicos de la compilación. Por ejemplo, la variable `.env.development` contiene los valores utilizados para durante el desarrollo, mientras que `.env.production` contiene valores utilizados para compilaciones de producción.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` archivos para otros usos [se puede especificar](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) por postfijo `.env` y un descriptor semántico, como `.env.stage` o `.env.production`. Different `.env` Los archivos se pueden usar al ejecutar o crear la aplicación React, estableciendo la variable `REACT_APP_ENV` antes de ejecutar un `npm` comando.

Por ejemplo, una aplicación React `package.json` puede contener lo siguiente `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### AEM cliente sin encabezado

La variable [AEM cliente sin encabezado para JavaScript](../../how-to/aem-headless-sdk.md) contiene un cliente AEM sin encabezado que realiza solicitudes HTTP a las API de GraphQL de AEM. El cliente AEM sin encabezado debe inicializarse con el host de AEM con el que interactúa, utilizando el valor del `.env` archivo.

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### React useEffect(..) gancho

Los enlaces useEffect personalizados llaman al cliente sin encabezado AEM, inicializado con el host de AEM, en nombre del componente React que representa la vista.

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### React, componente

el vínculo useEffect personalizado, `useAdventureByPath` se importa y se utiliza para obtener los datos mediante el cliente sin encabezado de AEM y, finalmente, procesar el contenido para el usuario final.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ Ejemplo de iOS™

Este ejemplo, basado en la variable [ejemplo AEM aplicación iOS™ sin encabezado](../../example-apps/ios-swiftui-app.md), ilustra cómo AEM solicitudes de API de GraphQL se pueden configurar para conectarse a diferentes hosts AEM en función de [variables de configuración específicas de la compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Las aplicaciones iOS™ requieren un cliente personalizado AEM sin encabezado para interactuar con las API de GraphQL AEM. El cliente sin encabezado de AEM debe escribirse de modo que el host de servicio de AEM sea configurable.

#### Generar configuración

El archivo de configuración XCode contiene los detalles de configuración predeterminados.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Inicialización del cliente sin encabezado de AEM personalizado

La variable [ejemplo AEM aplicación iOS sin encabezado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) utiliza un cliente sin encabezado AEM personalizado inicializado con los valores de configuración de `AEM_SCHEME` y `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

El cliente personalizado AEM sin encabezado (`api/Aem.swift`) contiene un método `makeRequest(..)` que prefieren AEM solicitudes de API de GraphQL con la AEM configurada `scheme` y `host`.

+ `api/Aem.swift`

```swift
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
    
    return request;
}
```

[Se pueden crear nuevos archivos de configuración de compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conectarse a diferentes servicios de AEM. Los valores específicos de la compilación para la variable `AEM_SCHEME` y `AEM_HOST` se utilizan en función de la compilación seleccionada en XCode, lo que da como resultado que el cliente personalizado AEM sin encabezado se conecte con el servicio de AEM correcto.

+++

+++ Ejemplo de Android™

Este ejemplo, basado en la variable [ejemplo AEM aplicación Android™ sin encabezado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra cómo se pueden configurar AEM solicitudes de API de GraphQL para conectarse a diferentes servicios de AEM en función de variables de configuración específicas de la compilación (o de los sabores).

Las aplicaciones Android™ (cuando se escriben en Java™) deben usar la variable [AEM cliente sin encabezado para Java™](https://github.com/adobe/aem-headless-client-java) para interactuar con AEM API de GraphQL. El cliente AEM sin encabezado, proporcionado por el cliente sin encabezado de AEM para Java™, debe inicializarse con el host de servicio de AEM al que se conecta.

#### Generar archivo de configuración

Las aplicaciones de Android™ definen &quot;productFlavors&quot; que se utilizan para crear artefactos para usos diferentes.
Este ejemplo muestra cómo se pueden definir dos sabores de producto de Android™, proporcionando diferentes hosts de servicio de AEM (`AEM_HOST`) valores para el desarrollo (`dev`) y producción (`prod`).

En el informe `build.gradle` archivo, un nuevo `flavorDimension` named `env` se crea.

En el `env` dimensión, dos `productFlavors` están definidas: `dev` y `prod`. Cada `productFlavor` uses `buildConfigField` para establecer variables específicas de la compilación que definen el servicio de AEM al que se va a conectar.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Cargador Android™

Inicializar el `AEMHeadlessClient` Generador, proporcionado por AEM cliente sin encabezado para Java™ con el `AEM_HOST` del `buildConfigField` campo .

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

Al crear la aplicación Android™ para diferentes usos, especifique la variable `env` y se utiliza el valor de host de AEM correspondiente.

+++

## URL de imagen de AEM

Las solicitudes de imagen de la aplicación sin encabezado a AEM deben configurarse para interactuar con el servicio de AEM correcto, tal como se describe en la sección [tabla superior](#managing-aem-hosts).

Mientras AEM GraphQL `... on ImageRef` proporciona campos `_authorUrl` y `_publishUrl` que contienen direcciones URL absolutas a los respectivos servicios de AEM, normalmente es más directo usar la variable `_path` campo y prefijo del host de servicio de AEM utilizado para consultar las API de GraphQL AEM.

Uso `_path` puede resultar especialmente beneficioso si la aplicación sin encabezado se puede conectar con AEM Author o AEM Publish según el contexto de implementación.

Si la aplicación sin encabezado interactúa exclusivamente con AEM Author o Publish, `_authorUrl` o `_publishUrl` Los campos se pueden utilizar para simplificar la implementación y se pueden ignorar las directrices de los ejemplos siguientes.

### Ejemplos

A continuación se muestran ejemplos de cómo las direcciones URL de imagen pueden prefijar el valor de host de AEM que se puede configurar para varios marcos de aplicaciones sin encabezado. En los ejemplos se presupone el uso de consultas de GraphQL que devuelven referencias de imagen mediante la variable `_path` campo .

Por ejemplo:

#### Consulta persistente de GraphQL

Esta consulta de GraphQL devuelve el valor de `_path`. Como se ve en la variable [Respuesta de GraphQL](#examples-react-graphql-response) que excluye un host.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

#### Respuesta de GraphQL

Esta respuesta de GraphQL devuelve el valor de la referencia de imagen `_path` que excluye un host.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

+++ Ejemplo de reacción

Este ejemplo, basado en la variable [ejemplo AEM aplicación React sin encabezado](../../example-apps/react-app.md), ilustra cómo se pueden configurar las direcciones URL de imagen para conectarse a los servicios de AEM correctos en función de las variables de entorno.

Este ejemplo muestra cómo codificar la referencia de imagen `_path` , con un campo configurable `REACT_APP_AEM_HOST` Reaccione la variable de entorno.

#### React environment file

React uses [archivos de entorno personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/)o `.env` , almacenados en la raíz del proyecto para definir valores específicos de la compilación. Por ejemplo, la variable `.env.development` contiene los valores utilizados para durante el desarrollo, mientras que `.env.production` contiene valores utilizados para compilaciones de producción.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` archivos para otros usos [se puede especificar](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) por postfijo `.env` y un descriptor semántico, como `.env.stage` o `.env.production`. Different `.env` se puede usar al ejecutar o crear la aplicación React, estableciendo la variable `REACT_APP_ENV` antes de ejecutar un `npm` comando.

Por ejemplo, una aplicación React `package.json` puede contener lo siguiente `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### React, componente

El componente React importa el `REACT_APP_AEM_HOST` y prefiera la imagen `_path` para proporcionar una URL de imagen completamente resoluble.

Lo mismo `REACT_APP_AEM_HOST` la variable de entorno se utiliza para inicializar el cliente AEM sin encabezado utilizado por `useAdventureByPath(..)` vínculo useEffect personalizado que se usa para recuperar los datos de GraphQL de AEM. Si utiliza la misma variable para construir la solicitud de API de GraphQL que la URL de la imagen, asegúrese de que la aplicación React interactúe con el mismo servicio de AEM para ambos casos de uso.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._path }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ Ejemplo de iOS™

Este ejemplo, basado en la variable [ejemplo AEM aplicación iOS™ sin encabezado](../../example-apps/ios-swiftui-app.md), ilustra cómo se pueden configurar AEM URL de imagen para conectarse a diferentes hosts AEM en función de [variables de configuración específicas de la compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Generar configuración

El archivo de configuración XCode contiene los detalles de configuración predeterminados.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Generador de URL de imagen

En `Aem.swift`, la implementación personalizada AEM cliente sin encabezado, una función personalizada `imageUrl(..)` toma la ruta de la imagen tal como se proporciona en la variable `_path` en la respuesta de GraphLQ y la precede con AEM host. A continuación, esta función se invoca en las vistas de iOS cada vez que se procesa una imagen.

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image paths wit the AEM scheme/host
    func imageUrl(path: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(path)")!
    }
    ...
}
```

#### Vista de iOS

La vista iOS y los prefijos de la imagen `_path` para proporcionar una URL de imagen completamente resoluble.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image path to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(path: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[Se pueden crear nuevos archivos de configuración de compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conectarse a diferentes servicios de AEM. Los valores específicos de la compilación para la variable `AEM_SCHEME` y `AEM_HOST` se utilizan en función de la compilación seleccionada en XCode, lo que provoca que el cliente personalizado AEM sin encabezado interactúe con el servicio de AEM correcto.

+++

+++ Ejemplo de Android™

Este ejemplo, basado en la variable [ejemplo AEM aplicación Android™ sin encabezado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra cómo se pueden configurar AEM direcciones URL de imagen para conectarse a diferentes servicios de AEM en función de variables de configuración específicas de la compilación (o de los sabores).

#### Generar archivo de configuración

Las aplicaciones de Android™ definen &quot;productFlavors&quot; que se utilizan para crear artefactos para diferentes usos.
Este ejemplo muestra cómo se pueden definir dos sabores de producto de Android™, proporcionando diferentes hosts de servicio de AEM (`AEM_HOST`) valores para el desarrollo (`dev`) y producción (`prod`).

En el informe `build.gradle` archivo, un nuevo `flavorDimension` named `env` se crea.

En el `env` dimensión, dos `productFlavors` están definidas: `dev` y `prod`. Cada `productFlavor` uses `buildConfigField` para establecer variables específicas de la compilación que definen el servicio de AEM al que se va a conectar.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Carga de la imagen AEM

Android™ utiliza un `ImageGetter` para recuperar y almacenar en caché localmente los datos de imagen de AEM. En `prepareDrawableFor(..)` el host de servicio de AEM, definido en la configuración de compilación activa, se utiliza para prefijar la ruta de la imagen y crear una URL a AEM.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String path) {
        // Get the image data from the cache using the path as the key
        Drawable drawable = drawablesByPath.get(path);
        return drawable;
    }
}
```

#### Vista de Android™

La vista Android™ obtiene los datos de imagen mediante la variable `RemoteImagesCache` usando la variable `_path` de la respuesta de GraphQL.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImagePath()));
        ...
    }
...
}
```

Al crear la aplicación Android™ para diferentes usos, especifique la variable `env` y se utiliza el valor de host de AEM correspondiente.

+++
---
title: Administración de hosts de AEM para AEM GraphQL
description: Obtenga información sobre cómo configurar los hosts de AEM en la aplicación sin encabezado de AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# Administración de hosts de AEM

La implementación de una aplicación sin encabezado de AEM requiere atención sobre cómo se construyen las URL de AEM para garantizar que se utilice el host/dominio de AEM correcto. Los tipos de solicitud/URL principales que se deben tener en cuenta son:

+ Solicitudes HTTP a __[API de AEM GraphQL](#aem-graphql-api-requests)__
+ __[URL de imagen](#aem-image-urls)__ a recursos de imagen referenciados en fragmentos de contenido y entregados por AEM

Normalmente, una aplicación sin encabezado de AEM interactúa con un único servicio de AEM tanto para la API de GraphQL como para las solicitudes de imagen. El servicio AEM cambia en función de la implementación de la aplicación sin encabezado de AEM:

| Tipo de implementación sin encabezado de AEM | Entorno de AEM | servicio de AEM |
|-------------------------------|:---------------------:|:----------------:|
| Producción | Producción | Publicación |
| Creación de previsualización | Producción | Vista previa |
| Desarrollo | Desarrollo | Publicación |

Para gestionar las permutaciones de tipo de implementación, cada implementación de aplicación se crea con una configuración que especifica el servicio de AEM al que conectarse. A continuación, el host/dominio del servicio AEM configurado se utiliza para construir las URL de la API de AEM GraphQL y las URL de imagen. Para determinar el método correcto para administrar las configuraciones dependientes de la compilación, consulte la documentación del marco de trabajo de la aplicación de AEM sin encabezado (por ejemplo, React, iOS™ Android, etc.), ya que el método varía según el marco de trabajo.

| Tipo de cliente | [Aplicación de una sola página (SPA)](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuración de hosts de AEM | ✔ | ✔ | ✔ | ✔ |

Los siguientes son ejemplos de posibles enfoques para construir direcciones URL para [la API de AEM GraphQL](#aem-graphql-api-requests) y [solicitudes de imagen](#aem-image-requests), para varios marcos y plataformas sin encabezado populares.

## Solicitudes de API de AEM GraphQL

Las solicitudes HTTP GET de la aplicación sin encabezado a las API de GraphQL de AEM deben configurarse para interactuar con el servicio AEM correcto, tal como se describe en la [tabla anterior](#managing-aem-hosts).

Al utilizar [SDK sin encabezado de AEM](../../how-to/aem-headless-sdk.md) (disponible para JavaScript basado en explorador, JavaScript basado en servidor y Java™), un host de AEM puede inicializar el objeto de cliente sin encabezado de AEM con el servicio de AEM con el que conectarse.

Al desarrollar un cliente sin encabezado de AEM personalizado, asegúrese de que el host del servicio AEM pueda parametrizarse en función de los parámetros de compilación.

### Ejemplos

A continuación se muestran ejemplos de cómo las solicitudes de API de AEM GraphQL pueden tener el valor de host de AEM configurado para varios marcos de aplicaciones sin encabezado.

+++ Ejemplo de React

Este ejemplo, basado de forma flexible en la [aplicación AEM Headless React](../../example-apps/react-app.md), ilustra cómo las solicitudes de la API de AEM GraphQL se pueden configurar para conectarse a diferentes servicios de AEM en función de variables de entorno.

Las aplicaciones de React deben usar [AEM Headless Client for JavaScript](../../how-to/aem-headless-sdk.md) para interactuar con las API de GraphQL de AEM. El cliente sin encabezado de AEM, proporcionado por el cliente sin encabezado de AEM para JavaScript, debe inicializarse con el host del servicio de AEM al que se conecta.

#### Archivo de entorno de React

React usa [archivos de entorno personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/) o `.env` archivos almacenados en la raíz del proyecto para definir valores específicos de la compilación. Por ejemplo, el archivo `.env.development` contiene valores utilizados para durante el desarrollo, mientras que `.env.production` contiene valores utilizados para las compilaciones de producción.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` archivos para otros usos [se pueden especificar](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) mediante la corrección posterior de `.env` y un descriptor semántico, como `.env.stage` o `.env.production`. Se pueden usar `.env` archivos diferentes al ejecutar o generar la aplicación React, configurando `REACT_APP_ENV` antes de ejecutar un comando `npm`.

Por ejemplo, la aplicación React `package.json` puede contener la siguiente configuración `scripts`:

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

#### cliente sin encabezado de AEM

El [cliente sin encabezado de AEM para JavaScript](../../how-to/aem-headless-sdk.md) contiene un cliente sin encabezado de AEM que realiza solicitudes HTTP a las API de GraphQL de AEM. El cliente sin encabezado de AEM debe inicializarse con el host de AEM con el que interactúa, utilizando el valor del archivo `.env` activo.

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

#### React: enlace useEffect(..)

Los vínculos personalizados de React useEffect llaman al cliente sin encabezado de AEM, inicializado con el host de AEM, en nombre del componente React que procesa la vista.

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

#### Componente React

Se importó el vínculo personalizado useEffect `useAdventureByPath` y se utilizó para obtener los datos mediante el cliente sin encabezado de AEM y, finalmente, para representar el contenido al usuario final.

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

Este ejemplo, basado en el [ejemplo de aplicación iOS™ sin encabezado de AEM](../../example-apps/ios-swiftui-app.md), ilustra cómo se pueden configurar las solicitudes de la API de GraphQL de AEM para conectarse a diferentes hosts de AEM en función de [variables de configuración específicas de la compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Las aplicaciones de iOS™ requieren un cliente sin encabezado de AEM personalizado para interactuar con las API de GraphQL de AEM. El cliente sin encabezado de AEM debe escribirse de modo que el host del servicio AEM sea configurable.

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

#### Inicialice el cliente sin encabezado personalizado de AEM

[La aplicación iOS sin encabezado de AEM de ejemplo](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) usa un cliente sin encabezado de AEM personalizado inicializado con los valores de configuración para `AEM_SCHEME` y `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

El cliente personalizado sin encabezado de AEM (`api/Aem.swift`) contiene un método `makeRequest(..)` que prefija las solicitudes de API de AEM GraphQL con los AEM `scheme` y `host` configurados.

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

[Se pueden crear nuevos archivos de configuración de compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conectarse a diferentes servicios de AEM. Los valores específicos de la compilación para `AEM_SCHEME` y `AEM_HOST` se utilizan en función de la compilación seleccionada en XCode, lo que da como resultado que el cliente personalizado sin encabezado de AEM se conecte con el servicio de AEM correcto.

+++

+++ Ejemplo de Android™

Este ejemplo, basado en el [ejemplo de aplicación Android™ sin encabezado de AEM](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra cómo se pueden configurar las solicitudes de la API de GraphQL de AEM para conectarse a diferentes servicios de AEM en función de variables de configuración específicas de la compilación (o variantes).

Las aplicaciones de Android™ (cuando se escriben en Java™) deben usar [AEM Headless Client for Java™](https://github.com/adobe/aem-headless-client-java) para interactuar con las API de GraphQL de AEM. El cliente sin encabezado de AEM, proporcionado por el cliente sin encabezado de AEM para Java™, debe inicializarse con el host del servicio de AEM al que se conecta.

#### Generar archivo de configuración

Las aplicaciones de Android™ definen &quot;productFlavors&quot;, que se utilizan para crear artefactos para diferentes usos.
Este ejemplo muestra cómo se pueden definir dos tipos de productos Android™, proporcionando valores de hosts de servicio AEM (`AEM_HOST`) diferentes para los usos de desarrollo (`dev`) y producción (`prod`).

En el archivo `build.gradle` de la aplicación, se crea un nuevo(a) `flavorDimension` denominado `env`.

En la dimensión `env`, se han definido dos `productFlavors`: `dev` y `prod`. Cada `productFlavor` usa `buildConfigField` para establecer variables específicas de la compilación que definen el servicio de AEM al que se va a conectar.

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

Inicialice el generador `AEMHeadlessClient`, proporcionado por el cliente sin encabezado de AEM para Java™ con el valor `AEM_HOST` del campo `buildConfigField`.

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

Al crear la aplicación de Android™ para diferentes usos, especifique el tipo `env` y el valor de host de AEM correspondiente.

+++

## URL de imagen de AEM

Las solicitudes de imagen de la aplicación sin encabezado a AEM deben configurarse para interactuar con el servicio AEM correcto, tal como se describe en la [tabla anterior](#managing-aem-hosts).

Adobe recomienda usar [imágenes optimizadas](../../how-to/images.md) disponibles a través del campo `_dynamicUrl` en las API de GraphQL de AEM. El campo `_dynamicUrl` devuelve una dirección URL sin host que puede ir precedida del host de servicio de AEM utilizado para consultar las API de GraphQL de AEM. Para el campo `_dynamicUrl` en la respuesta de GraphQL tiene este aspecto:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Ejemplos

A continuación se muestran ejemplos de cómo las direcciones URL de imagen pueden anteponer el valor de host de AEM, que se puede configurar para varios marcos de aplicaciones sin encabezado. Los ejemplos suponen el uso de consultas de GraphQL que devuelven referencias de imagen utilizando el campo `_dynamicUrl`.

Por ejemplo:

#### Consulta persistente de GraphQL

Esta consulta de GraphQL devuelve un `_dynamicUrl` de referencia de imagen. Como se ve en la [respuesta de GraphQL](#examples-react-graphql-response), que excluye un host.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### Respuesta de GraphQL

Esta respuesta de GraphQL devuelve el `_dynamicUrl` de la referencia de imagen que excluye un host.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ Ejemplo de React

Este ejemplo, basado en el [ejemplo de aplicación AEM Headless React](../../example-apps/react-app.md), ilustra cómo se pueden configurar las direcciones URL de imagen para conectarse a los servicios de AEM correctos en función de las variables de entorno.

Este ejemplo muestra el prefijo del campo de referencia de imagen `_dynamicUrl`, con una variable de entorno React `REACT_APP_AEM_HOST` configurable.

#### Archivo de entorno de React

React usa [archivos de entorno personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/) o `.env` archivos almacenados en la raíz del proyecto para definir valores específicos de la compilación. Por ejemplo, el archivo `.env.development` contiene valores utilizados para durante el desarrollo, mientras que `.env.production` contiene valores utilizados para las compilaciones de producción.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` archivos para otros usos [se pueden especificar](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) mediante la corrección posterior de `.env` y un descriptor semántico, como `.env.stage` o `.env.production`. Se puede usar un archivo `.env` diferente al ejecutar o generar la aplicación React, configurando `REACT_APP_ENV` antes de ejecutar un comando `npm`.

Por ejemplo, la aplicación React `package.json` puede contener la siguiente configuración `scripts`:

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

#### Componente React

El componente React importa la variable de entorno `REACT_APP_AEM_HOST` y prefija el valor de la imagen `_dynamicUrl` para proporcionar una dirección URL de imagen totalmente solucionable.

Esta misma variable de entorno `REACT_APP_AEM_HOST` se usa para inicializar el cliente sin encabezado de AEM que usa el vínculo useEffect personalizado `useAdventureByPath(..)` que se usa para recuperar los datos de GraphQL de AEM. Utilizando la misma variable para construir la solicitud de API de GraphQL como la dirección URL de la imagen, asegúrese de que la aplicación React interactúa con el mismo servicio de AEM para ambos casos de uso.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ Ejemplo de iOS™

Este ejemplo, basado en el [ejemplo de aplicación iOS™ sin encabezado de AEM](../../example-apps/ios-swiftui-app.md), ilustra cómo se pueden configurar las URL de imágenes de AEM para conectarse a diferentes hosts de AEM en función de [variables de configuración específicas de la compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

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

En `Aem.swift`, la implementación personalizada de cliente sin encabezado de AEM, una función personalizada `imageUrl(..)` toma la ruta de acceso de la imagen como se indica en el campo `_dynamicUrl` de la respuesta de GraphQL y la antepone al host de AEM. A continuación, esta función se invoca en las vistas de iOS cada vez que se procesa una imagen.

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
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### Vista de iOS

La vista de iOS y los prefijos del valor de la imagen `_dynamicUrl` para proporcionar una dirección URL de imagen totalmente solucionable.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[Se pueden crear nuevos archivos de configuración de compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conectarse a diferentes servicios de AEM. Los valores específicos de la compilación para `AEM_SCHEME` y `AEM_HOST` se utilizan en función de la compilación seleccionada en XCode, lo que da como resultado que el cliente personalizado sin encabezado de AEM interactúe con el servicio de AEM correcto.

+++

+++ Ejemplo de Android™

Este ejemplo, basado en el [ejemplo de aplicación Android™ sin encabezado de AEM](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra cómo se pueden configurar las direcciones URL de imágenes de AEM para conectarse a diferentes servicios de AEM en función de variables de configuración específicas de la compilación (o variantes).

#### Generar archivo de configuración

Las aplicaciones de Android™ definen &quot;productFlavors&quot;, que se utilizan para crear artefactos para diferentes usos.
Este ejemplo muestra cómo se pueden definir dos tipos de productos Android™, proporcionando valores de hosts de servicio AEM (`AEM_HOST`) diferentes para los usos de desarrollo (`dev`) y producción (`prod`).

En el archivo `build.gradle` de la aplicación, se crea un nuevo(a) `flavorDimension` denominado `env`.

En la dimensión `env`, se han definido dos `productFlavors`: `dev` y `prod`. Cada `productFlavor` usa `buildConfigField` para establecer variables específicas de la compilación que definen el servicio de AEM al que se va a conectar.

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

#### Carga de la imagen de AEM

Android™ usa un `ImageGetter` para recuperar y almacenar en caché localmente los datos de imagen de AEM. En `prepareDrawableFor(..)`, el host de servicio de AEM, definido en la configuración de compilación activa, se usa para prefijar la ruta de acceso de la imagen creando una dirección URL solucionable en AEM.

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
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Vista de Android™

La vista Android™ obtiene los datos de imagen por medio de `RemoteImagesCache` usando el valor `_dynamicUrl` de la respuesta de GraphQL.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

Al crear la aplicación de Android™ para diferentes usos, especifique el tipo `env` y el valor de host de AEM correspondiente.

+++

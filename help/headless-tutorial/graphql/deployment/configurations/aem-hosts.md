---
title: AEM AEM Administración de hosts de para GraphQL
description: AEM AEM Obtenga información sobre cómo configurar hosts de en la aplicación sin encabezado.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 496
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# AEM Administración de hosts

AEM AEM AEM La implementación de una aplicación sin encabezado requiere atención a cómo se construyen las direcciones URL de la para garantizar que se utiliza el host/dominio correcto. Los tipos de solicitud/URL principales que se deben tener en cuenta son:

+ AEM Solicitudes HTTP a __[API de GraphQL](#aem-graphql-api-requests)__
+ AEM __[URL de imagen](#aem-image-urls)__ a recursos de imagen a los que se hace referencia en fragmentos de contenido y que fueron entregados por el servicio de correo electrónico de

AEM AEM Por lo general, una aplicación sin encabezado de interactúa con un único servicio de ID tanto para la API de GraphQL como para las solicitudes de imagen. AEM AEM El servicio cambia en función de la implementación de la aplicación sin encabezado de:

| AEM Tipo de implementación sin encabezado | AEM entorno de | AEM servicio de |
|-------------------------------|:---------------------:|:----------------:|
| Producción | Producción | Publicación |
| Creación de previsualización | Producción | Vista previa |
| Desarrollo | Desarrollo | Publicación |

AEM Para gestionar las permutaciones del tipo de implementación, cada implementación de aplicación se crea con una configuración que especifica el servicio de la aplicación al que se va a conectar. AEM AEM A continuación, el host/dominio del servicio de configurado se utiliza para construir las URL de la API de GraphQL y las URL de la imagen de la. AEM Para determinar el método correcto para administrar las configuraciones dependientes de la versión, consulte la documentación del marco de trabajo de la aplicación sin encabezado de la aplicación (por ejemplo, React, iOS™ Android, etc.), ya que el método varía según el marco de trabajo.

| Tipo de cliente | SPA [Aplicación de una sola página ()](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| AEM configuración de hosts de | ✔ | ✔ | ✔ | ✔ |

AEM Los siguientes son ejemplos de posibles enfoques para construir direcciones URL para [API de GraphQL](#aem-graphql-api-requests) y [solicitudes de imagen](#aem-image-requests), para varios marcos y plataformas sin encabezado populares.

## AEM Solicitudes de API de GraphQL

Las solicitudes de GET AEM HTTP de la aplicación sin encabezado a las API de GraphQL AEM de la aplicación deben configurarse para que interactúen con el servicio de correcto, tal como se describe en la [tabla anterior](#managing-aem-hosts).

AEM Al utilizar [SDK sin encabezado](../../how-to/aem-headless-sdk.md) (disponible para JavaScript basado en explorador, JavaScript AEM AEM AEM basado en servidor y Java™), un host de puede inicializar el objeto de cliente sin encabezado de la con el servicio de la con el que conectarse.

AEM AEM Al desarrollar un cliente sin encabezado personalizado, asegúrese de que el host del servicio de sea parametrizable en función de los parámetros de compilación.

### Ejemplos

AEM A continuación, se muestran ejemplos de cómo las solicitudes de API de GraphQL AEM pueden tener el valor de host de que se puede configurar para varios marcos de aplicaciones sin encabezado.

+++ Ejemplo de React

AEM AEM Este ejemplo, basado de forma flexible en la aplicación [React sin encabezado](../../example-apps/react-app.md), ilustra cómo se pueden configurar las solicitudes de API de GraphQL AEM para conectarse a diferentes servicios de en función de las variables de entorno.

AEM Las aplicaciones de React deben usar [Cliente sin encabezado para JavaScript AEM](../../how-to/aem-headless-sdk.md) con el fin de interactuar con las API de GraphQL que se están utilizando para el uso de la interfaz de usuario de . AEM AEM El cliente sin encabezado de la, proporcionado por el cliente sin encabezado de la aplicación para JavaScript AEM, debe inicializarse con el host de servicio de la aplicación al que se conecta.

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

#### AEM cliente sin encabezado

AEM [Cliente sin encabezado para JavaScript AEM AEM](../../how-to/aem-headless-sdk.md) contiene un cliente sin encabezado para el que se realiza una petición HTTP para que las API de GraphQL se puedan usar para la. AEM AEM El cliente sin encabezado de la debe inicializarse con el host de la interfaz de usuario con el que interactúa, usando el valor del archivo `.env` activo.

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

AEM AEM Los enlaces personalizados de React useEffect llaman al cliente sin encabezado de la, inicializado con el host, en nombre del componente React que procesa la vista.

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

AEM Se importó el vínculo personalizado useEffect `useAdventureByPath` y se utilizó para obtener los datos mediante el cliente sin encabezado y, finalmente, para representar el contenido en el usuario final.

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

AEM Este ejemplo, basado en el [ejemplo de la aplicación iOSAEM ™ sin encabezado](../../example-apps/ios-swiftui-app.md), ilustra cómo se pueden configurar las solicitudes de la API de GraphQL AEM para conectarse a diferentes hosts de la en función de [variables de configuración específicas de la compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Las aplicaciones de iOSAEM AEM ™ requieren un cliente sin encabezado personalizado para poder interactuar con las API de GraphQL que se utilizan en la. AEM AEM El cliente sin encabezado de la debe escribirse de modo que el host de servicio de la sea configurable.

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

#### AEM Inicialice el cliente personalizado sin encabezado

AEM El [ejemplo de la aplicación iOS AEM sin encabezado ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) utiliza un cliente personalizado sin encabezado de la aplicación inicializado con los valores de configuración de `AEM_SCHEME` y `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

AEM AEM El cliente personalizado sin encabezado de la (`api/Aem.swift`) contiene un método `makeRequest(..)` que prefija las solicitudes de API de GraphQL AEM con los valores configurados de `scheme` y `host`.

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

AEM [Se pueden crear nuevos archivos de configuración de compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conectarse a diferentes servicios de la. AEM AEM Los valores específicos de la compilación para `AEM_SCHEME` y `AEM_HOST` se utilizan en función de la compilación seleccionada en XCode, lo que da como resultado el cliente personalizado sin encabezado para conectarse con el servicio de conexión de la interfaz de usuario de la interfaz de usuario de la interfaz de usuario de la interfaz de usuario correcta.

+++

+++ Ejemplo de Android™

AEM Este ejemplo, basado en el [ejemplo de la aplicación AndroidAEM ™ sin encabezado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra cómo se pueden configurar las solicitudes de API de GraphQL AEM para conectarse a diferentes servicios de según variables de configuración específicas de la compilación (o variantes).

Las aplicaciones de AndroidAEM AEM ™ (cuando se escriben en Java™) deben usar [Cliente sin encabezado para Java™](https://github.com/adobe/aem-headless-client-java) para interactuar con las API de GraphQL que se encuentran en proceso de. AEM AEM AEM El cliente sin encabezado de la, proporcionado por el cliente sin encabezado de la aplicación para Java™, debe inicializarse con el host de servicio al que se conecta.

#### Generar archivo de configuración

Las aplicaciones de Android™ definen &quot;productFlavors&quot;, que se utilizan para crear artefactos para diferentes usos.
Este ejemplo muestra cómo se pueden definir dos variantes de producto AndroidAEM ™, proporcionando valores de hosts de servicio (`AEM_HOST`) diferentes para los usos de desarrollo (`dev`) y producción (`prod`).

En el archivo `build.gradle` de la aplicación, se crea un nuevo(a) `flavorDimension` denominado `env`.

En la dimensión `env`, se han definido dos `productFlavors`: `dev` y `prod`. AEM Cada `productFlavor` usa `buildConfigField` para establecer variables específicas de la compilación que definen el servicio de la al que se va a conectar.

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

AEM Inicialice el generador `AEMHeadlessClient`, proporcionado por el cliente sin encabezado para Java™ con el valor `AEM_HOST` del campo `buildConfigField`.

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

Al crear la aplicación de AndroidAEM ™ para diferentes usos, especifique el tipo `env` y se utilizará el valor de host de la correspondiente.

+++

## AEM URL de imagen de

AEM AEM Las solicitudes de imagen de la aplicación sin encabezado a la que se va a realizar la solicitud deben estar configuradas para que interactúen con el servicio de correcto, tal como se describe en la [tabla anterior](#managing-aem-hosts).

El Adobe AEM recomienda usar [imágenes optimizadas](../../how-to/images.md) disponibles a través del campo `_dynamicUrl` en el uso de las API de GraphQL de la. AEM AEM El campo `_dynamicUrl` devuelve una dirección URL sin host que puede ir precedida del host de servicio de que se utiliza para consultar las API de GraphQL. Para el campo `_dynamicUrl` en la respuesta de GraphQL tiene este aspecto:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Ejemplos

AEM A continuación, se muestran ejemplos de cómo las direcciones URL de imagen pueden prefijar el valor de host que se hace configurable para varios marcos de aplicación sin encabezado. Los ejemplos suponen el uso de consultas de GraphQL que devuelven referencias de imagen utilizando el campo `_dynamicUrl`.

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

AEM AEM Este ejemplo, basado en el [ejemplo de la aplicación React sin encabezado](../../example-apps/react-app.md), ilustra cómo se pueden configurar las direcciones URL de imagen para conectarse a los servicios de correctos en función de las variables de entorno.

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

AEM Esta misma variable de entorno `REACT_APP_AEM_HOST` se usa para inicializar el cliente sin encabezado de la que usa el vínculo useEffect personalizado de `useAdventureByPath(..)` que se usa para recuperar los datos de GraphQL AEM de la. Utilizando la misma variable para construir la solicitud de API de GraphQL AEM como la dirección URL de la imagen, asegúrese de que la aplicación React interactúa con el mismo servicio de para ambos casos de uso.

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

AEM Este ejemplo, basado en el [ejemplo de la aplicación iOSAEM AEM ™ sin encabezado](../../example-apps/ios-swiftui-app.md), ilustra cómo se pueden configurar las direcciones URL de imagen para conectarse a diferentes hosts de la en función de [variables de configuración específicas de la compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

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

AEM En `Aem.swift`, la implementación personalizada de cliente sin encabezado de la, una función personalizada `imageUrl(..)` toma la ruta de acceso de la imagen como se indica en el campo `_dynamicUrl` de la respuesta de GraphQL AEM y la antepone con el host de. A continuación, esta función se invoca en las vistas de iOS cada vez que se procesa una imagen.

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

AEM [Se pueden crear nuevos archivos de configuración de compilación](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conectarse a diferentes servicios de la. AEM AEM Los valores específicos de la compilación para `AEM_SCHEME` y `AEM_HOST` se utilizan en función de la compilación seleccionada en XCode, lo que da como resultado que el cliente personalizado sin encabezado de la interactúe con el servicio correcto.

+++

+++ Ejemplo de Android™

AEM Este ejemplo, basado en el [ejemplo de la aplicación AndroidAEM AEM ™ sin encabezado](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra cómo se pueden configurar las direcciones URL de imagen para conectarse a diferentes servicios de en función de variables de configuración específicas de la compilación (o variantes).

#### Generar archivo de configuración

Las aplicaciones de Android™ definen &quot;productFlavors&quot;, que se utilizan para crear artefactos para diferentes usos.
Este ejemplo muestra cómo se pueden definir dos variantes de producto AndroidAEM ™, proporcionando valores de hosts de servicio (`AEM_HOST`) diferentes para los usos de desarrollo (`dev`) y producción (`prod`).

En el archivo `build.gradle` de la aplicación, se crea un nuevo(a) `flavorDimension` denominado `env`.

En la dimensión `env`, se han definido dos `productFlavors`: `dev` y `prod`. AEM Cada `productFlavor` usa `buildConfigField` para establecer variables específicas de la compilación que definen el servicio de la al que se va a conectar.

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

#### AEM Cargando la imagen de la

AndroidAEM ™ usa un `ImageGetter` para recuperar y almacenar en caché localmente los datos de imagen de los usuarios de la red de área de trabajo de la. AEM AEM En `prepareDrawableFor(..)`, el host de servicio de, definido en la configuración de la versión activa, se usa para prefijar la ruta de acceso de la imagen a la que se crea una dirección URL con resolución que se va a crear en el servidor de correo electrónico de.

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

Al crear la aplicación de AndroidAEM ™ para diferentes usos, especifique el tipo `env` y se utilizará el valor de host de la correspondiente.

+++

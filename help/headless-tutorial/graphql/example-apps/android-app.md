---
title: 'AEM Aplicación Android: Ejemplo de aplicación sin encabezado para la aplicación de'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). Esta aplicación para Android muestra cómo realizar consultas en el contenido mediante las API de GraphQL AEM de la interfaz de usuario de la aplicación de.
version: Cloud Service
mini-toc-levels: 2
kt: 10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 7938325427b6becb38ac230a3bc4b031353ca8b1
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 6%

---

# Aplicación de Android

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). Esta aplicación para Android muestra cómo realizar consultas en el contenido mediante las API de GraphQL AEM de la interfaz de usuario de la aplicación de. El [AEM Cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java) se utiliza para ejecutar las consultas de GraphQL y asignar datos a objetos Java para activar la aplicación.

![AEM Aplicación Java para Android con tecnología sin encabezado](./assets/android-java-app/android-app.png)


Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM requisitos de

AEM La aplicación de Android funciona con las siguientes opciones de implementación de. Todas las implementaciones requieren lo siguiente [Sitio WKND 3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) para instalar.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=es)

La aplicación de Android está diseñada para conectarse a un __AEM Publish__ , sin embargo, puede obtener contenido de AEM Author si la autenticación se proporciona en la configuración de la aplicación de Android.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) y abra la carpeta `android-app`
1. Modificación del archivo `config.properties` en `app/src/main/assets/config.properties` y actualizar `contentApi.endpoint` AEM para que coincida con su entorno de destino haga lo siguiente:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Autenticación básica__

   El `contentApi.user` y `contentApi.password` AEM autentique un usuario local de la con acceso al contenido de WKND GraphQL.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Descargar un [Dispositivo virtual Android](https://developer.android.com/studio/run/managing-avds) (API mínima 28).
1. Cree e implemente la aplicación mediante el emulador de Android.


### AEM Conexión a entornos de

AEM Si se conecta a un entorno de autor de [autorización](https://github.com/adobe/aem-headless-client-java#using-authorization) es obligatorio. El [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) proporciona la capacidad de usar [autenticación basada en token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Para usar el generador de cliente de actualización de autenticación basada en tokens en `AdventureLoader.java` y `AdventuresLoader.java`:

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## El código

A continuación se muestra un breve resumen de los archivos y el código importantes utilizados para activar la aplicación. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### Consultas persistentes

AEM Siguiendo las prácticas recomendadas de las consultas sin encabezado, la aplicación de iOS AEM utiliza consultas persistentes de GraphQL para consultar datos de aventuras, de forma predeterminada, de la manera más sencilla. La aplicación utiliza dos consultas persistentes:

+ `wknd/adventures-all` AEM consulta persistente, que devuelve todas las aventuras en con un conjunto abreviado de propiedades de, que se han guardado de forma predeterminada. Esta consulta persistente genera la lista de aventuras de la vista inicial.

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
                _dynamicUrl
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` consulta persistente, que devuelve una sola aventura de `slug` (una propiedad personalizada que identifica de forma exclusiva una aventura) con un conjunto completo de propiedades. Esta consulta persistente activa las vistas de detalles de aventura.

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
          _dynamicUrl
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

AEM Las consultas persistentes se ejecutan a través de la GET HTTP y, por lo tanto, la variable [AEM Cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java) se utiliza para ejecutar las consultas de GraphQL AEM persistentes contra los datos de la aplicación y cargar el contenido de la aventura en la aplicación.

AEM Cada consulta persistente tiene una clase de &quot;cargador&quot; correspondiente, que llama asincrónicamente al punto final de GET HTTP de la y devuelve los datos de la aventura utilizando el personalizado definido [modelo de datos](#data-models).

+ `loader/AdventuresLoader.java`

  Obtiene la lista de Aventuras en la pantalla principal de la aplicación utilizando `wknd-shared/adventures-all` consulta persistente.

+ `loader/AdventureLoader.java`

  Obtiene una sola aventura seleccionándola a través de la `slug` parámetro, con el parámetro `wknd-shared/adventure-by-slug` consulta persistente.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### Modelos de datos de respuesta de GraphQL{#data-models}

`Adventure.java` es un POJO de Java que se inicializa con los datos JSON de la petición GraphQL y modela una aventura para usarla en las vistas de la aplicación Android.

### Vistas

La aplicación para Android utiliza dos vistas para presentar los datos de la aventura en la experiencia móvil.

+ `AdventureListFragment.java`

  Invoca el `AdventuresLoader` y muestra las aventuras devueltas en una lista.

+ `AdventureDetailFragment.java`

  Invoca el `AdventureLoader` uso del `slug` parámetro pasado a través de la selección de aventuras en `AdventureListFragment` y muestra los detalles de una sola aventura.

### Imágenes remotas

`loader/RemoteImagesCache.java` es una clase de utilidad que ayuda a preparar imágenes remotas en una caché para que se puedan utilizar con elementos de la IU de Android. El contenido de aventura hace referencia a imágenes en AEM Assets a través de una dirección URL y esta clase se utiliza para mostrar ese contenido.

## Recursos adicionales

+ [AEM Introducción a la tecnología sin encabezado de: Tutorial de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es)
+ [AEM Cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java)

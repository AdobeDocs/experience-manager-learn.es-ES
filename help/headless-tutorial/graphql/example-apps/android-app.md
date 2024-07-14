---
title: 'Aplicación de Android AEM: Ejemplo sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). Esta aplicación para Android muestra cómo realizar consultas en el contenido mediante las API de GraphQL AEM de la.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM sin encabezado as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 160
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# Aplicación de Android

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager AEM (). Esta aplicación para Android muestra cómo realizar consultas en el contenido mediante las API de GraphQL AEM de la. AEM [Cliente sin encabezado para JavaScript ](https://github.com/adobe/aem-headless-client-java) se usa para ejecutar las consultas de GraphQL y asignar datos a objetos Java para poder activar la aplicación.

![Aplicación Java de Android AEM con encabezado sin encabezado](./assets/android-java-app/android-app.png)


Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM requisitos de

La aplicación de Android AEM funciona con las siguientes opciones de implementación de. Todas las implementaciones requieren que esté instalado [WKND Site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest).

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

La aplicación de Android AEM está diseñada para conectarse a un entorno __Publish AEM__, pero puede obtener contenido de la fuente de Autor de la aplicación o si se proporciona autenticación en la configuración de la aplicación de Android.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra [Android Studio](https://developer.android.com/studio) y abra la carpeta `android-app`
1. AEM Modifique el archivo `config.properties` en `app/src/main/assets/config.properties` y actualice `contentApi.endpoint` para que coincida con el entorno de destino de la aplicación de la:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Autenticación básica__

   AEM `contentApi.user` y `contentApi.password` autentican a un usuario local de la aplicación con acceso al contenido de WKND GraphQL.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=my-special-android-app-user
   contentApi.password=password123
   ```

1. Descargar un [dispositivo virtual Android](https://developer.android.com/studio/run/managing-avds) (API mínima 28).
1. Cree e implemente la aplicación mediante el emulador de Android.


### AEM Conexión a entornos de

AEM Si se requiere la conexión a un entorno de autor de [authorization](https://github.com/adobe/aem-headless-client-java#using-authorization). [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) proporciona la capacidad de usar [autenticación basada en token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Para usar el generador de cliente de actualización de autenticación basada en token en `AdventureLoader.java` y `AdventuresLoader.java`:

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

+ AEM `wknd/adventures-all` persistió en la consulta, lo que devuelve todas las aventuras en la que se ha realizado un seguimiento con un conjunto abreviado de propiedades. Esta consulta persistente genera la lista de aventuras de la vista inicial.

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
                _path
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` persistió en la consulta, lo que devuelve una sola aventura de `slug` (una propiedad personalizada que identifica de forma exclusiva una aventura) con un conjunto completo de propiedades. Esta consulta persistente activa las vistas de detalles de aventura.

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
          _path
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

AEM las consultas persistentes se ejecutan a través de la GET AEM HTTP y, por lo tanto, el cliente [sin encabezado para Java](https://github.com/adobe/aem-headless-client-java) se usa para ejecutar las consultas de GraphQL AEM persistentes en la aplicación y cargar el contenido de la aventura en la aplicación.

AEM Cada consulta persistente tiene una clase de &quot;cargador&quot; correspondiente, que llama asincrónicamente al punto final de GET HTTP de la y devuelve los datos de aventura utilizando el [modelo de datos](#data-models) definido personalizado.

+ `loader/AdventuresLoader.java`

  Obtiene la lista de aventuras en la pantalla principal de la aplicación mediante la consulta persistente `wknd-shared/adventures-all`.

+ `loader/AdventureLoader.java`

  Obtiene una sola aventura seleccionándola a través del parámetro `slug`, usando la consulta persistente `wknd-shared/adventure-by-slug`.

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

La aplicación de Android utiliza dos vistas para presentar los datos de la aventura en la experiencia móvil.

+ `AdventureListFragment.java`

  Invoca `AdventuresLoader` y muestra las aventuras devueltas en una lista.

+ `AdventureDetailFragment.java`

  Invoca a `AdventureLoader` mediante el parámetro `slug` pasado a través de la selección de aventuras en la vista `AdventureListFragment` y muestra los detalles de una sola aventura.

### Imágenes remotas

`loader/RemoteImagesCache.java` es una clase de utilidad que ayuda a preparar imágenes remotas en una caché para que se puedan utilizar con elementos de la interfaz de usuario de Android. El contenido de aventura hace referencia a imágenes en AEM Assets a través de una dirección URL y esta clase se utiliza para mostrar ese contenido.

## Recursos adicionales

+ AEM [Tutorial de introducción a la sin encabezado de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=es)
+ AEM [Cliente sin encabezado de para Java](https://github.com/adobe/aem-headless-client-java)

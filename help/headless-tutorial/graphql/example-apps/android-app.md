---
title: 'Aplicación Android: ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación de Android muestra cómo consultar contenido mediante las API de GraphQL de AEM.
version: Cloud Service
mini-toc-levels: 2
kt: 10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 985d52f02025dc9cb2b9c70ead4a88af07c63f29
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 4%

---

# Aplicación de Android

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación de Android muestra cómo consultar contenido mediante las API de GraphQL de AEM. La variable [AEM cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java) se utiliza para ejecutar las consultas de GraphQL y asignar datos a objetos Java para impulsar la aplicación.

![Aplicación Java de Android con AEM sin encabezado](./assets/android-java-app/android-app.png)


Consulte la [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM requisitos

La aplicación Android funciona con las siguientes opciones de implementación de AEM. Todas las implementaciones requieren la variable [Sitio WKND v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) para instalar.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)
+ [Inicio rápido de AEM 6.5 SP13+](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es?lang=en#install-local-aem-instances)

La aplicación Android está diseñada para conectarse a un __AEM Publish__ , sin embargo, puede obtener contenido de AEM Author si se proporciona autenticación en la configuración de la aplicación Android.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) y abra la carpeta `android-app`
1. Modificación del archivo `config.properties` at `app/src/main/assets/config.properties` y actualizar `contentApi.endpoint` para que coincida con el entorno de AEM de destino:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __Autenticación básica__

   La variable `contentApi.user` y `contentApi.password` autentique a un usuario AEM local con acceso al contenido de WKND GraphQL.

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Descargar un [Dispositivo virtual de Android](https://developer.android.com/studio/run/managing-avds) (API mínima 28).
1. Cree e implemente la aplicación con el emulador de Android.


### Conexión a entornos AEM

`10.0.2.2` es [alias especial IP](https://developer.android.com/studio/run/emulator-networking) para localhost al usar el emulador crear `10.0.2.2:4502` es equivalente a `localhost:4502`. Si se conecta a un entorno de publicación AEM (recomendado), no se requiere ninguna autorización y `contentAPi.user` y `contentApi.password` se puede dejar en blanco.

Si se conecta a un entorno de creación de AEM [autorización](https://github.com/adobe/aem-headless-client-java#using-authorization) es obligatorio. De forma predeterminada, la aplicación está configurada para utilizar la autenticación básica con un nombre de usuario y una contraseña de `admin:admin`. La variable [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) proporciona la capacidad de usar [autenticación basada en token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Para utilizar el generador de cliente de actualización de autenticación basada en token en `AdventureLoader.java` y `AdventuresLoader.java`:

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

A continuación se muestra un breve resumen de los archivos y el código importantes utilizados para activar la aplicación. El código completo se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

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

AEM las consultas persistentes se ejecutan a través de GET HTTP y, por lo tanto, la variable [AEM cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java) se utiliza para ejecutar las consultas de GraphQL persistentes con AEM y cargar el contenido de aventura en la aplicación.

Cada consulta persistente tiene una clase &quot;loader&quot; correspondiente, que llama asincrónicamente al punto final de la GET HTTP AEM y devuelve los datos de aventura utilizando el definido personalizado [modelo de datos](#data-models).

+ `loader/AdventuresLoader.java`

   Obtiene la lista de aventuras en la pantalla de inicio de la aplicación utilizando la variable `wknd-shared/adventures-all` consulta persistente.

+ `loader/AdventureLoader.java`

   Recopila una sola aventura seleccionándola a través de la variable `slug` usando el parámetro `wknd-shared/adventure-by-slug` consulta persistente.

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

`Adventure.java` es un POJO de Java que se inicializa con los datos JSON de la solicitud de GraphQL y modela una aventura para utilizarla en las vistas de la aplicación Android.

### Vistas

La aplicación Android utiliza dos vistas para presentar los datos de aventura en la experiencia móvil.

+ `AdventureListFragment.java`

   Invoca el `AdventuresLoader` y muestra las aventuras devueltas en una lista.

+ `AdventureDetailFragment.java`

   Invoca el `AdventureLoader` usando la variable `slug` parámetro transferido a través de la selección de aventura en la variable `AdventureListFragment` y muestra los detalles de una sola aventura.

### Imágenes remotas

`loader/RemoteImagesCache.java` es una clase de utilidad que ayuda a preparar imágenes remotas en una caché para que se puedan utilizar con elementos de interfaz de usuario de Android. El contenido de aventura hace referencia a imágenes en AEM Assets a través de una URL y esta clase se utiliza para mostrar ese contenido.

## Recursos adicionales

+ [Introducción a AEM sin encabezado: Tutorial de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [AEM cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java)

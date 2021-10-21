---
title: 'Aplicación Android: ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Se proporciona una aplicación de Android que muestra cómo consultar contenido mediante las API de GraphQL de AEM. El Apollo Client Android se utiliza para generar consultas de GraphQL y asignar datos a objetos de Swift para impulsar la aplicación. SwiftUI se utiliza para procesar una lista sencilla y una vista detallada del contenido.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 3%

---


# Aplicación Swift UI de Android

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Se proporciona una aplicación de Android que muestra cómo consultar contenido mediante las API de GraphQL de AEM. La variable [AEM cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java) se utiliza para ejecutar las consultas de GraphQL y asignar datos a objetos Java para impulsar la aplicación.

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

* [Android Studio](https://developer.android.com/studio)
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

1. Launch [Android Studio](https://developer.android.com/studio) y abra la carpeta `android-app`
1. Modificación del archivo `config.properties` at `app/src/main/assets/config.properties` y actualizar `contentApi.endpoint` para que coincida con el entorno de AEM de destino:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Descargar un [Dispositivo virtual de Android](https://developer.android.com/studio/run/managing-avds) (API principal 28)
1. Cree e implemente la aplicación con el emulador de Android.


### Conexión a entornos AEM

`10.0.2.2` es [alias especial](https://developer.android.com/studio/run/emulator-networking) para localhost al usar el emulador. So `10.0.2.2:4502` es equivalente a `localhost:4502`. Si se conecta a un entorno de publicación AEM (recomendado), no se requiere ninguna autorización y `contentAPi.user` y `contentApi.password` se puede dejar en blanco.

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

### Recuperación de contenido

La variable [AEM cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java) La aplicación utiliza para ejecutar la consulta de GraphQL con AEM y cargar el contenido de aventura en la aplicación.

`AdventuresLoader.java` es el archivo que busca y carga la lista inicial de aventuras en la pantalla principal de la aplicación. Utiliza [Consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) que es [preempaquetado](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) con el sitio de referencia WKND. El punto final es `/wknd/adventures-all`. `AEMHeadlessClientBuilder` crea una instancia nueva basada en el punto final de la api definido en `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` es el archivo que busca y carga el contenido de aventura para cada una de las vistas de detalles. Una vez más, el `AEMHeadlessClient` se utiliza para ejecutar la consulta. Se ejecuta una consulta de graphQL normal en función de la ruta al fragmento de contenido de aventura. La consulta se mantiene en un archivo independiente denominado [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` es un POJO que se inicializa con los datos JSON de la solicitud de GraphQL.

`RemoteImagesCache.java` es una clase de utilidad que ayuda a preparar imágenes remotas en una caché para que se puedan utilizar con elementos de interfaz de usuario de Android. El contenido de aventura hace referencia a imágenes en AEM Assets a través de una URL y esta clase se utiliza para mostrar ese contenido.

### Vistas

`AdventureListFragment.java` - cuando se llama a los déclencheur, `AdventuresLoader` y muestra las aventuras devueltas en una lista.

`AdventureDetailFragment.java` - inicializa `AdventureLoader` y muestra los detalles de una sola aventura.

## Recursos adicionales

* [Introducción a AEM sin encabezado: Tutorial de GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [AEM cliente sin encabezado para Java](https://github.com/adobe/aem-headless-client-java)


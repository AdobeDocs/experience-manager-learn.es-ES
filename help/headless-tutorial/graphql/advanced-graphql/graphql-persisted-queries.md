---
title: 'Consultas persistentes de GraphQL: conceptos avanzados de AEM sin encabezado - GraphQL'
description: En este capítulo de Conceptos avanzados de Adobe Experience Manager (AEM) sin encabezado, aprenda a crear y actualizar consultas de GraphQL persistentes con parámetros. Aprenda a pasar parámetros de control de caché en consultas persistentes.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 1%

---

# Consultas persistentes de GraphQL

Las consultas persistentes son consultas que se almacenan en el servidor de Adobe Experience Manager (AEM). Los clientes pueden enviar una solicitud de GET HTTP con el nombre de la consulta para ejecutarla. El beneficio de este enfoque es la accesibilidad. Aunque las consultas de GraphQL del lado del cliente también se pueden ejecutar mediante solicitudes de POST HTTP, que no se pueden almacenar en caché, las consultas persistentes se pueden almacenar en caché mediante cachés HTTP o una CDN, lo que mejora el rendimiento. Las consultas persistentes le permiten simplificar sus solicitudes y mejorar la seguridad, ya que las consultas están encapsuladas en el servidor y el administrador de AEM tiene control total sobre ellas. Es **prácticas recomendadas y recomendaciones** para utilizar consultas persistentes al trabajar con la API de AEM GraphQL.

En el capítulo anterior, ha explorado algunas consultas avanzadas de GraphQL para recopilar datos para la aplicación WKND. En este capítulo, persiste en las consultas para AEM y aprender a utilizar el control de caché en las consultas persistentes.

## Requisitos previos {#prerequisites}

Este documento forma parte de un tutorial en varias partes. Asegúrese de que la variable [capítulo anterior](explore-graphql-api.md) se ha completado antes de continuar con este capítulo.

## Objetivos {#objectives}

En este capítulo, aprenda a:

* Mantener consultas de GraphQL con parámetros
* Usar parámetros de control de caché con consultas persistentes

## Consulte _Consultas persistentes de GraphQL_ configuración

Vamos a revisar que _Consultas persistentes de GraphQL_ están habilitadas para el proyecto WKND Site en la instancia de AEM.

1. Vaya a **Herramientas** > **General** > **Explorador de configuración**.

1. Select **WKND compartido** y, a continuación, seleccione **Propiedades** en la barra de navegación superior para abrir las propiedades de configuración. En la página Propiedades de configuración, debería ver que la variable **Consultas persistentes de GraphQL** está habilitado.

   ![Propiedades de configuración](assets/graphql-persisted-queries/configuration-properties.png)

## Persist GraphQL queries using builtin GraphiQL Explorer tool

En esta sección, mantengamos la consulta GraphQL que se utiliza más adelante en la aplicación cliente para recuperar y procesar los datos del fragmento de contenido de aventura.

1. Introduzca la siguiente consulta en el Explorador de GraphiQL:

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   Compruebe que la consulta funciona antes de guardarla.

1. A continuación, pulse Guardar como e introduzca `adventure-details-by-slug` como Nombre de consulta.

   ![Consulta Persist GraphQL](assets/graphql-persisted-queries/persist-graphql-query.png)

## Ejecución de consultas persistentes con variables mediante la codificación de caracteres especiales

Comprendamos cómo las consultas persistentes con variables se ejecutan en la aplicación del lado del cliente codificando los caracteres especiales.

Para ejecutar una consulta persistente, la aplicación cliente realiza una solicitud de GET utilizando la siguiente sintaxis:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

Ejecución de una consulta persistente _con una variable_, la sintaxis anterior cambia a:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

Los caracteres especiales como punto y coma (;), signo igual (=), barras inclinadas (/) y espacio deben convertirse para utilizar la codificación UTF-8 correspondiente.

Al ejecutar el `getAllAdventureDetailsBySlug` consulta desde el terminal de línea de comandos, revisamos estos conceptos en acción.

1. Abra el Explorador de GraphiQL y haga clic en el botón **elipses** (...) junto a la consulta persistente `getAllAdventureDetailsBySlug`y haga clic en **Copiar URL**. Pegue la URL copiada en un panel de texto, de este modo:

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Agregar `yosemite-backpacking` como valor de variable

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. Codifique los caracteres especiales de punto y coma (;) y el signo igual (=)

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Abra un terminal de línea de comandos y utilice [Curl](https://curl.se/) ejecutar la consulta

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Si ejecuta la consulta anterior en el entorno de AEM Author , debe enviar las credenciales. Consulte [Token de acceso de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) para su demostración y [Llamar a la API de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) para obtener más información sobre la documentación.

Además, revise [Ejecución de una consulta persistente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [Uso de variables de consulta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables)y [Codificación de la URL de consulta para su uso en una aplicación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) para conocer la ejecución de consultas persistentes por aplicaciones cliente.

## Actualizar los parámetros de control de caché en consultas persistentes {#cache-control-all-adventures}

La API de AEM GraphQL permite actualizar los parámetros predeterminados de control de caché a las consultas para mejorar el rendimiento. Los valores predeterminados de control de caché son:

* 60 segundos es el TTL predeterminado (máximo=60) para el cliente (por ejemplo, un explorador)

* 7200 segundos es el TTL predeterminado (s-maxage=7200) para Dispatcher y CDN; también conocidas como cachés compartidas

Utilice la variable `adventures-all` para actualizar los parámetros de control de caché. La respuesta de consulta es grande y resulta útil controlar su `age` en la caché. Esta consulta persistente se utiliza más adelante para actualizar la variable [aplicación cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Abra el Explorador de GraphiQL y haga clic en el botón **elipses** (...) junto a la consulta persistente y, a continuación, haga clic en **Encabezados** para abrir **Configuración de caché** modal.

   ![Opción Persist GraphQL Header](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. En el **Configuración de caché** modal, actualice el `max-age` valor de encabezado a `600 `segundos (10 minutos) y, a continuación, haga clic en **Guardar**

   ![Persist GraphQL Cache Configuration](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Consulte [Almacenamiento en caché de las consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) para obtener más información sobre los parámetros predeterminados de control de caché.


## ¡Enhorabuena!

¡Enhorabuena! Ahora ha aprendido a mantener consultas de GraphQL con parámetros, actualizar consultas persistentes y usar parámetros de control de caché con consultas persistentes.

## Pasos siguientes

En el [capítulo siguiente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), implementará las solicitudes para consultas persistentes en la aplicación WKND.

---
title: 'Consultas persistentes de GraphQL: conceptos avanzados de AEM sin encabezado: GraphQL'
description: En este capítulo de Conceptos avanzados de Adobe Experience Manager (AEM) sin encabezado, aprenderá a crear y actualizar consultas de GraphQL persistentes con parámetros. Obtenga información sobre cómo pasar parámetros de control de caché en consultas persistentes.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
duration: 183
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# Consultas persistentes de GraphQL

Las consultas persistentes son consultas que se almacenan en el servidor de Adobe Experience Manager (AEM). Los clientes pueden enviar una petición HTTP GET con el nombre de la petición para ejecutarla. El beneficio de este enfoque es la accesibilidad. Aunque las consultas GraphQL del lado del cliente también se pueden ejecutar mediante solicitudes HTTP POST, que no se pueden almacenar en caché, las consultas persistentes se pueden almacenar en caché mediante cachés HTTP o una CDN, lo que mejora el rendimiento. Las consultas persistentes le permiten simplificar las solicitudes y mejorar la seguridad, ya que las consultas se encapsulan en el servidor y el administrador de AEM tiene control total sobre ellas. Es **práctica recomendada y muy recomendable** utilizar consultas persistentes al trabajar con la API de AEM GraphQL.

En el capítulo anterior, ha explorado algunas consultas avanzadas de GraphQL para recopilar datos para la aplicación WKND. En este capítulo, se mantienen las consultas en AEM y se aprende a utilizar el control de caché en consultas persistentes.

## Requisitos previos {#prerequisites}

Este documento forma parte de un tutorial de varias partes. Asegúrese de que el [capítulo anterior](explore-graphql-api.md) se haya completado antes de continuar con este capítulo.

## Objetivos {#objectives}

En este capítulo, aprenderá a:

* Persistir consultas de GraphQL con parámetros
* Usar parámetros de control de caché con consultas persistentes

## Revisar la configuración de _Consultas persistentes de GraphQL_

Revisemos que _Consultas persistentes de GraphQL_ están habilitadas para el proyecto del sitio WKND en su instancia de AEM.

1. Vaya a **Herramientas** > **General** > **Explorador de configuración**.

1. Seleccione **WKND compartido** y, a continuación, seleccione **Propiedades** en la barra de navegación superior para abrir las propiedades de configuración. En la página Propiedades de configuración, debería ver que el permiso **Consultas persistentes de GraphQL** está habilitado.

   ![Propiedades de configuración](assets/graphql-persisted-queries/configuration-properties.png)

## Persistir consultas de GraphQL mediante la herramienta Explorador de GraphiQL integrada

En esta sección, vamos a mantener la consulta de GraphQL que se utiliza posteriormente en la aplicación cliente para recuperar y procesar los datos del fragmento de contenido de aventura.

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

1. A continuación, pulse Guardar como e introduzca `adventure-details-by-slug` como nombre de la consulta.

   ![Continuar consulta de GraphQL](assets/graphql-persisted-queries/persist-graphql-query.png)

## Ejecución de consultas persistentes con variables mediante la codificación de caracteres especiales

Comprendamos cómo las consultas persistentes con variables se ejecutan en la aplicación del lado del cliente codificando los caracteres especiales.

Para ejecutar una consulta persistente, la aplicación cliente realiza una petición GET utilizando la siguiente sintaxis:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

Para ejecutar una consulta persistente _con una variable_, la sintaxis anterior cambia a:

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

Los caracteres especiales como punto y coma (;), signo igual (=), barras diagonales (/) y espacio deben convertirse para utilizar la codificación UTF-8 correspondiente.

Al ejecutar la consulta `getAllAdventureDetailsBySlug` desde el terminal de línea de comandos, se revisan estos conceptos en acción.

1. Abra el Explorador de GraphiQL, haga clic en los **puntos suspensivos** (...) junto a la consulta persistente `getAllAdventureDetailsBySlug` y, a continuación, haga clic en **Copiar URL**. Pegue la URL copiada en un panel de texto y tenga el siguiente aspecto:

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. Agregar `yosemite-backpacking` como valor de variable

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. Codifique los caracteres especiales punto y coma (;) y signo igual (=)

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. Abra un terminal de línea de comandos y ejecute la consulta con [Curl](https://curl.se/)

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    Si se ejecuta la consulta anterior en el entorno de creación de AEM, debe enviar las credenciales. Consulte [Token de acceso de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) para ver una demostración del mismo y [Llamar a la API de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) para obtener información detallada sobre la documentación.

Además, revise [Cómo ejecutar una consulta persistente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [Usar variables de consulta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables) y [Codificar la URL de consulta para que la use una aplicación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) para aprender la ejecución de consultas persistentes por aplicaciones cliente.

## Actualizar parámetros de control de caché en consultas persistentes {#cache-control-all-adventures}

La API de GraphQL de AEM le permite actualizar los parámetros predeterminados de control de caché a sus consultas para mejorar el rendimiento. Los valores predeterminados de control de caché son:

* 60 segundos es el TTL predeterminado (máximo=60) para el cliente (por ejemplo, un explorador)

* 7200 segundos es el TTL predeterminado (s-maxage=7200) para Dispatcher y CDN; también conocido como cachés compartidas

Utilice la consulta `adventures-all` para actualizar los parámetros de control de caché. La respuesta a la consulta es grande y resulta útil controlar su `age` en la caché. Esta consulta persistente se usa más adelante para actualizar la [aplicación cliente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. Abra el Explorador de GraphiQL, haga clic en los **puntos suspensivos** (...) junto a la consulta persistente y, a continuación, haga clic en **Encabezados** para abrir el modal **Configuración de caché**.

   ![Opción Persist GraphQL Header](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. En el modal **Cache Configuration**, actualice el valor del encabezado `max-age` a `600 `segundos (10 minutos) y luego haga clic en **Guardar**

   ![Conservar configuración de caché de GraphQL](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


Revise [Almacenamiento en caché de las consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) para obtener más información sobre los parámetros predeterminados de control de caché.


## Enhorabuena.

¡Enhorabuena! Ahora ha aprendido a hacer que persistan las consultas de GraphQL con parámetros, a actualizar las consultas persistentes y a utilizar parámetros de control de caché con consultas persistentes.

## Pasos siguientes

En el [capítulo siguiente](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md), implementará las solicitudes para consultas persistentes en la aplicación WKND.

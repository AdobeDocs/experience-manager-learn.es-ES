---
title: 'Explorar la API de AEM GraphQL: Conceptos avanzados de AEM sin encabezado: GraphQL'
description: Envíe consultas de GraphQL mediante el IDE de GraphiQL. Obtenga información sobre consultas avanzadas mediante filtros, variables y directivas. Consulta de referencias de fragmento y contenido, incluidas referencias de campos de texto multilínea.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 0%

---

# Explorar la API de AEM GraphQL

La API de GraphQL de AEM le permite exponer los datos de fragmento de contenido a aplicaciones descendentes. En el tutorial básico [tutorial de GraphQL de varios pasos](../multi-step/explore-graphql-api.md), ha utilizado el Explorador de GraphiQL para probar y refinar las consultas de GraphQL.

En este capítulo, se utiliza el Explorador de GraphiQL para definir consultas más avanzadas para recopilar datos de los fragmentos de contenido creados en la [capítulo anterior](../advanced-graphql/author-content-fragments.md).

## Requisitos previos {#prerequisites}

Este documento forma parte de un tutorial en varias partes. Asegúrese de que los capítulos anteriores se hayan completado antes de continuar con este capítulo.

## Objetivos {#objectives}

En este capítulo, aprenderá a:

* Filtrar una lista de fragmentos de contenido con referencias mediante variables de consulta
* Filtro para contenido dentro de una referencia de fragmento
* Consulta de contenido en línea y referencias de fragmento de un campo de texto multilínea
* Consulta mediante directivas
* Consulta del tipo de contenido de objeto JSON

## Uso del Explorador de GraphiQL


La variable [Explorador de GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) permite a los desarrolladores crear y probar consultas con contenido del entorno de AEM actual. La herramienta GraphiQL también permite a los usuarios **persistir o guardar** consultas que las aplicaciones cliente utilizarán en una configuración de producción.

A continuación, explore la potencia de AEM API de GraphQL mediante el Explorador de GraphiQL integrado.

1. En la pantalla Inicio de AEM, vaya a **Herramientas** > **General** > **Editor de consultas de GraphQL**.

   ![Vaya al IDE de GraphiQL](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>En, algunas versiones de AEM (6.X.X) la herramienta GraphiQL Explorer (también conocida como GraphiQL IDE) debe instalarse manualmente, siga [instrucciones aquí](../multi-step/explore-graphql-api.md#install-the-graphiql-tool-optional).

1. En la esquina superior derecha, asegúrese de que Endpoint está configurado como **Punto final compartido WKND**. Cambio de la variable _Punto final_ aquí se muestra el valor desplegable existente _Consultas persistentes_ en la esquina superior izquierda.

   ![Establecer extremo de GraphQL](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Esto abarcará todas las consultas a los modelos creados en la variable **WKND compartido** proyecto.


## Filtrado de una lista de fragmentos de contenido mediante variables de consulta

En el [tutorial de GraphQL de varios pasos](../multi-step/explore-graphql-api.md), definió y utilizó consultas persistentes básicas para obtener datos de fragmentos de contenido. En este caso, puede ampliar este conocimiento y filtrar los datos de fragmentos de contenido pasando variables a las consultas persistentes.

Al desarrollar aplicaciones cliente, normalmente debe filtrar los fragmentos de contenido en función de argumentos dinámicos. La API de AEM GraphQL permite pasar estos argumentos como variables en una consulta para evitar la construcción de cadenas en el lado del cliente durante la ejecución. Para obtener más información sobre las variables de GraphQL, consulte la [Documentación de GraphQL](https://graphql.org/learn/queries/#variables).

Para este ejemplo, consulte todos los Instructores que tengan una habilidad particular.

1. En el IDE de GraphiQL, pegue la siguiente consulta en el panel izquierdo:

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
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
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   La variable `listPersonBySkill` la consulta anterior acepta una variable (`skillFilter`) que es un requisito `String`. Esta consulta realiza una búsqueda con todos los fragmentos de contenido de persona y los filtra en función de la variable `skills` y la cadena que se transfiere `skillFilter`.

   La variable `listPersonBySkill` incluye el `contactInfo` , que es una referencia de fragmento al modelo de información de contacto definido en los capítulos anteriores. El modelo Información de contacto contiene `phone` y `email` campos. Debe estar presente al menos uno de estos campos en la consulta para que se realice correctamente.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. A continuación, defina `skillFilter` y obtenga todos los instructores que sean competentes en esquí. Pegue la siguiente cadena JSON en el panel Variables de consulta del IDE de GraphiQL:

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. Ejecute la consulta. El resultado debería ser similar al siguiente:

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

Pulse el botón **Play** en el menú superior para ejecutar la consulta. Debería ver los resultados de los fragmentos de contenido del capítulo anterior:

![Persona por resultados de habilidades](assets/explore-graphql-api/person-by-skill.png)

## Filtro para contenido dentro de una referencia de fragmento

La API de AEM GraphQL permite consultar fragmentos de contenido anidados. En el capítulo anterior, se agregaron tres nuevas referencias de fragmento a un fragmento de contenido de aventura: `location`, `instructorTeam`y `administrator`. Ahora, filtremos todas las aventuras para cualquier administrador que tenga un nombre en particular.

>[!CAUTION]
>
>Solo se debe permitir un modelo como referencia para que esta consulta se realice correctamente.

1. En el IDE de GraphiQL, pegue la siguiente consulta en el panel izquierdo:

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta :

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   La variable `getAdventureAdministratorDetailsByAdministratorName` la consulta filtra todas las aventuras de cualquier `administrator` de `fullName` &quot;Jacob Wester&quot;, que devuelve información de entre dos fragmentos de contenido anidados: Aventura e Instructor.

1. Ejecute la consulta. El resultado debería ser similar al siguiente:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## Consulta de referencias en línea desde un campo de texto multilínea {#query-rte-reference}

La API de AEM GraphQL permite realizar consultas de contenido y referencias de fragmento dentro de campos de texto multilínea. En el capítulo anterior, se agregaron ambos tipos de referencia al **Descripción** campo del fragmento de contenido de Yosemite Team. Ahora, recuperemos estas referencias.

1. En el IDE de GraphiQL, pegue la siguiente consulta en el panel izquierdo:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   La variable `getTeamByAdventurePath` la consulta filtra todas las aventuras por ruta y devuelve los datos del `instructorTeam` referencia de fragmento de una aventura específica.

   `_references` es un campo generado por el sistema que se utiliza para mostrar referencias, incluidas las que se insertan en campos de texto multilínea.

   La variable `getTeamByAdventurePath` La consulta recupera varias referencias. En primer lugar, utiliza el complemento `ImageRef` objeto para recuperar el `_path` y `__typename` de imágenes insertadas como referencias de contenido en el campo de texto multilínea. A continuación, utiliza `LocationModel` para recuperar los datos del fragmento de contenido de ubicación insertado en el mismo campo.

   La consulta también incluye el `_metadata` campo . Esto le permite recuperar el nombre del fragmento de contenido del equipo y mostrarlo más adelante en la aplicación WKND.

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta para obtener la Aventura de embalaje posterior Yosemite:

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Ejecute la consulta. El resultado debería ser similar al siguiente:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   La variable `_references` El campo revela tanto la imagen del logotipo como el fragmento de contenido del Lodge de Yosemite Valley que se insertó en el **Descripción** campo .


## Consulta mediante directivas

A veces, al desarrollar aplicaciones cliente, debe cambiar condicionalmente la estructura de las consultas. En este caso, la API de AEM GraphQL permite utilizar las directivas de GraphQL para cambiar el comportamiento de las consultas en función de los criterios proporcionados. Para obtener más información sobre las directivas de GraphQL, consulte la [Documentación de GraphQL](https://graphql.org/learn/queries/#directives).

En el [sección anterior](#query-rte-reference), ha aprendido a consultar referencias en línea dentro de campos de texto multilínea. El contenido se recuperó del `description` en la variable `plaintext` formato. A continuación, expandamos esa consulta y usemos una directiva para recuperar condicionalmente `description` en el `json` también.

1. En el IDE de GraphiQL, pegue la siguiente consulta en el panel izquierdo:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   La consulta anterior acepta una variable más (`includeJson`) que es un requisito `Boolean`, también conocido como la directiva de la consulta. Una directiva puede utilizarse para incluir condicionalmente los datos del `description` en el campo `json` basado en el booleano que se pasa `includeJson`.

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta :

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Ejecute la consulta. Debería obtener el mismo resultado que en la sección anterior de [consulta de referencias en línea dentro de campos de texto multilínea](#query-rte-reference).

1. Actualice el `includeJson` directiva `true` y ejecute de nuevo la consulta. El resultado debería ser similar al siguiente:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Consulta del tipo de contenido de objeto JSON

Recuerde que en el capítulo anterior sobre la creación de fragmentos de contenido, ha añadido un objeto JSON al **Tiempo por temporada** campo . Ahora recuperemos esos datos dentro del fragmento de contenido de ubicación.

1. En el IDE de GraphiQL, pegue la siguiente consulta en el panel izquierdo:

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
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
     }
   }
   ```

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta :

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Ejecute la consulta. El resultado debería ser similar al siguiente:

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   La variable `weatherBySeason` contiene el objeto JSON añadido en el capítulo anterior.

## Consulta de todo el contenido a la vez

Hasta ahora, se han ejecutado varias consultas para ilustrar las capacidades de la API de AEM GraphQL.

Los mismos datos podrían recuperarse con una sola consulta y esta consulta se utiliza más adelante en la aplicación cliente para recuperar información adicional como ubicación, nombre del equipo, integrantes del equipo de una aventura:

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


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## ¡Enhorabuena!

¡Enhorabuena! Ahora ha probado consultas avanzadas para recopilar datos de los fragmentos de contenido que ha creado en el capítulo anterior.

## Pasos siguientes

En el [capítulo siguiente](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), aprenderá a mantener las consultas de GraphQL y por qué es recomendable utilizar las consultas persistentes en sus aplicaciones.

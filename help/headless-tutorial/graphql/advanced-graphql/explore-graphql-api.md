---
title: 'AEM Explore la API de GraphQL AEM de: Conceptos avanzados de la tecnología sin encabezado: GraphQL'
description: Envíe consultas de GraphQL mediante el IDE de GraphiQL. Obtenga información acerca de consultas avanzadas mediante filtros, variables y directivas. Consulta de referencias de fragmento y contenido, incluidas las referencias de campos de texto multilínea.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
duration: 438
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# AEM Exploración de la API de GraphQL de

La API de GraphQL AEM en le permite exponer los datos de fragmentos de contenido a aplicaciones de flujo descendente. En el tutorial básico [tutorial de GraphQL de varios pasos](../multi-step/explore-graphql-api.md), utilizó el Explorador de GraphiQL para probar y refinar las consultas de GraphQL.

En este capítulo, utiliza el Explorador de GraphiQL para definir consultas más avanzadas con el fin de recopilar datos de los fragmentos de contenido que creó en el [capítulo anterior](../advanced-graphql/author-content-fragments.md).

## Requisitos previos {#prerequisites}

Este documento forma parte de un tutorial de varias partes. Asegúrese de que los capítulos anteriores se hayan completado antes de continuar con este capítulo.

## Objetivos {#objectives}

En este capítulo, aprenderá a hacer lo siguiente:

* Filtrado de una lista de fragmentos de contenido con referencias mediante variables de consulta
* Filtrado de contenido dentro de una referencia de fragmento
* Consulta de contenido en línea y referencias de fragmento desde un campo de texto multilínea
* Consulta mediante directivas
* Consulta del tipo de contenido de objeto JSON

## Uso del Explorador de GraphiQL


AEM La herramienta [Explorador de GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) permite a los desarrolladores crear y probar consultas con contenido en el entorno de la actual. La herramienta GraphiQL también permite a los usuarios **mantener o guardar** consultas para que las usen las aplicaciones cliente en una configuración de producción.

AEM A continuación, explore la potencia de la API de GraphQL de la mediante el Explorador de GraphiQL integrado.

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **Editor de consultas de GraphQL**.

   ![Vaya al IDE de GraphiQL](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>AEM En, algunas versiones de la herramienta Explorador de GraphiQL (también conocida como IDE de GraphiQL) de (6.X.X) deben instalarse manualmente, siga [instrucciones desde aquí](../how-to/install-graphiql-aem-6-5.md).

1. En la esquina superior derecha, asegúrese de que el extremo está establecido en **WKND Shared Endpoint**. Si se cambia el valor desplegable _Extremo_ aquí, se mostrarán las _Consultas persistentes_ existentes en la esquina superior izquierda.

   ![Establecer extremo de GraphQL](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Esto ampliará todas las consultas a los modelos creados en el proyecto **WKND compartido**.


## Filtrado de una lista de fragmentos de contenido mediante variables de consulta

En el [tutorial anterior de GraphQL de varios pasos](../multi-step/explore-graphql-api.md), definió y utilizó consultas persistentes básicas para obtener datos de fragmentos de contenido. Aquí puede ampliar este conocimiento y filtrar los datos de fragmentos de contenido pasando variables a las consultas persistentes.

Al desarrollar aplicaciones cliente, normalmente debe filtrar los fragmentos de contenido en función de argumentos dinámicos. AEM La API de GraphQL le permite pasar estos argumentos como variables en una consulta para evitar la construcción de cadenas en el lado del cliente durante la ejecución. Para obtener más información sobre las variables de GraphQL, consulte la [documentación de GraphQL](https://graphql.org/learn/queries/#variables).

Para este ejemplo, consulte todos los instructores que tengan una aptitud determinada.

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

   La consulta `listPersonBySkill` anterior acepta una variable (`skillFilter`) que es un(a) `String` obligatorio(a). Esta consulta realiza una búsqueda en todos los fragmentos de contenido de persona y los filtra en función del campo `skills` y de la cadena pasada en `skillFilter`.

   `listPersonBySkill` incluye la propiedad `contactInfo`, que es una referencia de fragmento al modelo de información de contacto definido en los capítulos anteriores. El modelo de información de contacto contiene `phone` y `email` campos. Debe haber al menos uno de estos campos en la consulta para que funcione correctamente.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. A continuación, definamos `skillFilter` y obtengamos todos los instructores que sean competentes en esquí. Pegue la siguiente cadena JSON en el panel Variables de consulta del IDE de GraphiQL:

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. Ejecute la consulta. El resultado debe ser similar al siguiente:

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

Presione el botón **Reproducir** en el menú superior para ejecutar la consulta. Debería ver los resultados de los fragmentos de contenido del capítulo anterior:

![Resultados de persona por aptitud](assets/explore-graphql-api/person-by-skill.png)

## Filtrado de contenido dentro de una referencia de fragmento

AEM La API de GraphQL le permite consultar fragmentos de contenido anidados. En el capítulo anterior, agregó tres nuevas referencias de fragmento a un fragmento de contenido de aventura: `location`, `instructorTeam` y `administrator`. Ahora, vamos a filtrar todas las aventuras para cualquier administrador que tenga un nombre en particular.

>[!CAUTION]
>
>Solo se debe permitir un modelo como referencia para que esta consulta funcione correctamente.

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

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta:

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   La consulta `getAdventureAdministratorDetailsByAdministratorName` filtra todas las aventuras para cualquier `administrator` de `fullName` &quot;Jacob Wester&quot;, devolviendo información de entre dos fragmentos de contenido anidados: Aventura e Instructor.

1. Ejecute la consulta. El resultado debe ser similar al siguiente:

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

AEM La API de GraphQL le permite consultar contenido y referencias de fragmento dentro de campos de texto multilínea. En el capítulo anterior, agregó ambos tipos de referencia al campo **Descripción** del fragmento de contenido del equipo de Yosemite. Ahora, vamos a recuperar estas referencias.

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

   La consulta `getTeamByAdventurePath` filtra todas las aventuras por ruta de acceso y devuelve datos para la referencia de fragmento `instructorTeam` de una aventura específica.

   `_references` es un campo generado por el sistema que se usa para mostrar referencias, incluidas las que se insertan en campos de texto multilínea.

   La consulta `getTeamByAdventurePath` recupera varias referencias. En primer lugar, utiliza el objeto integrado `ImageRef` para recuperar las `_path` y `__typename` de imágenes insertadas como referencias de contenido en el campo de texto multilínea. A continuación, utiliza `LocationModel` para recuperar los datos del fragmento de contenido de ubicación insertado en el mismo campo.

   La consulta también incluye el campo `_metadata`. Esto le permite recuperar el nombre del fragmento de contenido del equipo y mostrarlo más adelante en la aplicación WKND.

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta para obtener la aventura de mochilero de Yosemite:

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Ejecute la consulta. El resultado debe ser similar al siguiente:

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

   El campo `_references` muestra tanto la imagen del logotipo como el fragmento de contenido de Yosemite Valley Lodge que se insertó en el campo **Descripción**.


## Consulta mediante directivas

A veces, al desarrollar aplicaciones cliente, debe cambiar condicionalmente la estructura de las consultas. AEM En este caso, la API de GraphQL le permite utilizar directivas de GraphQL para cambiar el comportamiento de las consultas en función de los criterios proporcionados. Para obtener más información sobre las directivas de GraphQL, consulte la [documentación de GraphQL](https://graphql.org/learn/queries/#directives).

En la [sección anterior](#query-rte-reference), aprendió a consultar referencias en línea dentro de campos de texto multilínea. El contenido se recuperó del campo `description` en formato `plaintext`. A continuación, expandamos esa consulta y utilicemos una directiva para recuperar condicionalmente `description` en el formato `json`.

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

   La consulta anterior acepta una variable más (`includeJson`) que es una `Boolean` necesaria, también conocida como directiva de la consulta. Se puede usar una directiva para incluir condicionalmente datos del campo `description` en el formato `json` en función del booleano que se pasa en `includeJson`.

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Ejecute la consulta. Debería obtener el mismo resultado que en la sección anterior de [cómo consultar referencias en línea dentro de campos de texto multilínea](#query-rte-reference).

1. Actualice la directiva `includeJson` a `true` y ejecute de nuevo la consulta. El resultado debe ser similar al siguiente:

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

Recuerde que en el capítulo anterior sobre la creación de fragmentos de contenido, agregó un objeto JSON al campo **Clima por temporada**. Ahora recuperemos esos datos dentro del fragmento de contenido de ubicación.

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

1. A continuación, pegue la siguiente cadena JSON en el panel Variables de consulta:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Ejecute la consulta. El resultado debe ser similar al siguiente:

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

   El campo `weatherBySeason` contiene el objeto JSON agregado en el capítulo anterior.

## Consultar todo el contenido a la vez

AEM Hasta ahora, se han ejecutado varias consultas para ilustrar las capacidades de la API de GraphQL de la.

Los mismos datos se pudieron recuperar con una sola consulta y esta consulta se utiliza posteriormente en la aplicación cliente para recuperar información adicional como la ubicación, el nombre del equipo o los integrantes del equipo de una aventura:

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

## Enhorabuena.

Enhorabuena. Ahora ha probado las consultas avanzadas para recopilar datos de los fragmentos de contenido que creó en el capítulo anterior.

## Pasos siguientes

En el [capítulo siguiente](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), aprenderá a hacer que persistan las consultas de GraphQL y por qué es recomendable usar consultas persistentes en las aplicaciones.

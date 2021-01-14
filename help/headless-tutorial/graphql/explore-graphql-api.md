---
title: 'Explorar las API de GraphQL: Introducción a AEM sin cabeza: GraphQL'
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Explore AEM API de GraphQL mediante el IDE integrado de GrapiQL. Descubra cómo AEM genera automáticamente un esquema GraphQL basado en un modelo de fragmento de contenido. Experimente construyendo consultas básicas usando la sintaxis de GraphQL.
sub-product: activos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
translation-type: tm+mt
source-git-commit: eb2b556c5947b15a31a74a86dadd525fb06bcf14
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 0%

---


# Explorar las API de GraphQL {#explore-graphql-apis}

>[!CAUTION]
>
> El Envío API de GraphQL de AEM para fragmentos de contenido está disponible bajo petición.
> Póngase en contacto con la asistencia de Adobe para habilitar la API para su AEM como programa de Cloud Service.

La API de GraphQL de AEM proporciona un potente lenguaje de consulta para exponer los datos de fragmentos de contenido a aplicaciones de flujo descendente. Los modelos de fragmento de contenido definen el esquema de datos que utilizan los fragmentos de contenido. Cada vez que se crea o actualiza un modelo de fragmento de contenido, el esquema se traduce y se agrega al &quot;gráfico&quot; que conforma la API de GraphQL.

En este capítulo, exploraremos algunas consultas comunes de GraphQL para recopilar contenido. Integrado en AEM es un IDE llamado [GraphiQL](https://github.com/graphql/graphiql). El IDE de GraphiQL le permite probar y perfeccionar rápidamente las consultas y los datos devueltos. GraphiQL también proporciona un acceso fácil a la documentación, lo que facilita el aprendizaje y la comprensión de los métodos disponibles.

## Requisitos previos {#prerequisites}

Este es un tutorial en varias partes y se da por hecho que se han completado los pasos descritos en [Creación de fragmentos de contenido](./author-content-fragments.md).

## Objetivos {#objectives}

* Aprenda a utilizar la herramienta GraphiQL para construir una consulta con la sintaxis GraphQL.
* Aprenda a realizar la consulta de una lista de fragmentos de contenido y un solo fragmento de contenido.
* Obtenga información sobre cómo filtrar y solicitar atributos de datos específicos.
* Aprenda a realizar la consulta de una variación de un fragmento de contenido.
* Aprenda a unirse a una consulta de varios modelos de fragmento de contenido

## Consulta de una lista de fragmentos de contenido {#query-list-cf}

Un requisito común será la consulta de varios fragmentos de contenido.

1. Vaya al IDE de GraphiQL en [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html).
1. Pegue la siguiente consulta en el panel izquierdo (debajo de la lista de comentarios):

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. Pulse el botón **Reproducir** en el menú superior para ejecutar la consulta. Debería ver los resultados de los fragmentos de contenido de colaboradores del capítulo anterior:

   ![Resultados de Lista del colaborador](assets/explore-graphql-api/contributorlist-results.png)

1. Coloque el cursor debajo del texto `_path` e introduzca **CTRL+Espacio** en las sugerencias de código de déclencheur. Añada `fullName` y `occupation` a la consulta.

   ![Actualizar Consulta con la coincidencia de código](assets/explore-graphql-api/update-query-codehinting.png)

1. Vuelva a ejecutar la consulta pulsando el botón **Reproducir** y verá que los resultados incluyen las propiedades adicionales `fullName` y `occupation`.

   ![Nombre completo y resultados de ocupación](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` y  `occupation` son propiedades sencillas. Recuerde en el capítulo [Definición de modelos de fragmento de contenido](./content-fragment-models.md) que `fullName` y `occupation` son los valores utilizados al definir el **nombre de propiedad** de los campos respectivos.

1. `pictureReference` y  `biography` representan campos más complejos. Actualice la consulta con lo siguiente para devolver datos sobre los campos `pictureReference` y `biography`.

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biography {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biography` es un campo de texto multilínea y la API de GraphQL nos permite elegir una variedad de formatos para los resultados como  `html`,  `markdown`o  `json` o  `plaintext`.

   `pictureReference` es una referencia de contenido y se espera que sea una imagen, por lo que se utiliza un  `ImageRef` objeto integrado. Esto nos permite solicitar datos adicionales sobre la referencia de la imagen, como `width` y `height`.

1. Luego, experimente con una consulta para una lista de **aventuras**. Ejecute la siguiente consulta:

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   Debe ver una lista de **Aventuras** devueltas. No dude en experimentar añadiendo campos adicionales a la consulta.

## Filtrar una Lista de fragmentos de contenido {#filter-list-cf}

A continuación, veamos cómo es posible filtrar los resultados a un subconjunto de fragmentos de contenido basado en un valor de propiedad.

1. Introduzca la siguiente consulta en la interfaz de usuario de GraphiQL:

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   La consulta anterior realiza una búsqueda con todos los colaboradores del sistema. El filtro agregado al principio de la consulta realizará una comparación en el campo `occupation` y la cadena &quot;**fotógrafo**&quot;.

1. Ejecute la consulta, se espera que solo se devuelva un **colaborador**.
1. Introduzca la siguiente consulta para la consulta de una lista de **aventuras** donde `adventureActivity` es **no** igual a **&quot;Surfing&quot;**:

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. Ejecute la consulta e inspeccione los resultados. Observe que ninguno de los resultados incluye un `adventureType` igual a **&quot;Surfing&quot;**.

Hay muchas otras opciones para filtrar y crear consultas complejas. Los ejemplos anteriores son algunos.

## Consulta de un solo fragmento de contenido {#query-single-cf}

También es posible la consulta directa de un solo fragmento de contenido. El contenido de AEM se almacena de forma jerárquica y el identificador único de un fragmento se basa en la ruta del fragmento. Si el objetivo es devolver datos sobre un solo fragmento, es preferible utilizar la ruta y la consulta del modelo directamente. Usar esta sintaxis significa que la complejidad de la consulta será muy baja y generará un resultado más rápido.

1. Introduzca la siguiente consulta en el editor de GraphiQL:

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

1. Ejecute la consulta y observe que se devuelve el resultado único del fragmento **Stacey Roswells**.

   En el ejercicio anterior, se utilizó un filtro para reducir una lista de resultados. Se puede usar una sintaxis similar para filtrar por ruta, aunque la sintaxis anterior es preferible por motivos de rendimiento.

1. Recuerde en el capítulo [Creación de fragmentos de contenido](./author-content-fragments.md) que se creó una variación **Resumen** para **Stacey Roswells**. Actualice la consulta para devolver la variación **Summary**:

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biography {
           html
         }
       }
     }
   }
   ```

   Aunque la variación se denominó **Resumen**, las variaciones persisten en minúsculas y, por lo tanto, se utiliza `summary`.

1. Ejecute la consulta y observe que el campo `biography` contiene un resultado mucho más corto `html`.

## Consulta para varios modelos de fragmento de contenido {#query-multiple-models}

También es posible combinar consultas independientes en una única consulta. Esto resulta útil para minimizar el número de solicitudes HTTP necesarias para activar la aplicación. Por ejemplo, la vista *Inicio* de una aplicación puede mostrar contenido basado en **dos** modelos de fragmento de contenido diferentes. En lugar de ejecutar **dos** consultas independientes, podemos combinar las consultas en una sola solicitud.

1. Introduzca la siguiente consulta en el editor de GraphiQL:

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. Ejecute la consulta y observe que el conjunto de resultados contiene datos de **Aventuras** y **Colaboradores**:

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## Recursos adicionales

Para ver muchos más ejemplos de consultas de GraphQL, consulte: [Aprendiendo a usar GraphQL con AEM: Contenido de muestra y Consultas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Felicitaciones! {#congratulations}

Felicitaciones, acaba de crear y ejecutar varias consultas de GraphQL.

## Próximos pasos {#next-steps}

En el siguiente capítulo, [Consultando AEM desde una aplicación de React](./graphql-and-external-app.md), explorará cómo una aplicación externa puede realizar la consulta AEM los extremos de GraphQL. Aplicación externa que modifica la aplicación WKND GraphQL React de muestra para añadir consultas GraphQL de filtrado, lo que permite al usuario de la aplicación filtrar las aventuras por actividad. También se le proporcionará una gestión básica de errores.

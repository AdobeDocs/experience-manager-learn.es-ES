---
title: 'Explorar las API de GraphQL: Introducción a AEM Headless - GraphQL'
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Explore las API de GraphQL de AEM mediante el IDE integrado de GrapiQL. Descubra cómo AEM genera automáticamente un esquema de GraphQL basado en un modelo de fragmento de contenido. Experimento en la construcción de consultas básicas mediante la sintaxis de GraphQL.
sub-product: activos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 1%

---


# Explorar las API de GraphQL {#explore-graphql-apis}

La API de GraphQL de AEM proporciona un potente lenguaje de consulta para exponer los datos de fragmentos de contenido a aplicaciones descendentes. Los modelos de fragmento de contenido definen el esquema de datos que utilizan los fragmentos de contenido. Siempre que se crea o actualiza un modelo de fragmento de contenido, el esquema se traduce y se añade al &quot;gráfico&quot; que constituye la API de GraphQL.

En este capítulo, exploraremos algunas consultas comunes de GraphQL para recopilar contenido mediante un IDE llamado [GraphiQL](https://github.com/graphql/graphiql). El IDE de GraphiQL le permite probar y refinar rápidamente las consultas y los datos devueltos. GraphiQL también proporciona fácil acceso a la documentación, lo que facilita la comprensión de los métodos disponibles.

## Requisitos previos {#prerequisites}

Este es un tutorial en varias partes y se da por hecho que se han completado los pasos descritos en [Creación de fragmentos de contenido](./author-content-fragments.md).

## Objetivos {#objectives}

* Aprenda a utilizar la herramienta GraphiQL para construir una consulta utilizando la sintaxis GraphQL.
* Obtenga información sobre cómo consultar una lista de fragmentos de contenido y un solo fragmento de contenido.
* Obtenga información sobre cómo filtrar y solicitar atributos de datos específicos.
* Obtenga información sobre cómo consultar una variación de un fragmento de contenido.
* Aprenda a unir una consulta de varios modelos de fragmento de contenido

## Instale la herramienta GraphiQL {#install-graphiql}

El IDE de GraphiQL es una herramienta de desarrollo que solo se necesita en entornos de nivel inferior como un desarrollo o una instancia local. Por lo tanto, no se incluye en el proyecto AEM, sino que se presenta como un paquete independiente que se puede instalar según sea necesario.

1. Vaya al **[Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/es-ES/aemcloud.html)** > **AEM as a Cloud Service**.
1. Busque &quot;GraphiQL&quot; (asegúrese de incluir el **i** en **GraphiQL**.
1. Descargue el paquete de contenido más reciente de **GraphiQL v.x.x.x**

   ![Descargar paquete de GraphiQL](assets/explore-graphql-api/software-distribution.png)

   El archivo zip es un paquete de AEM que se puede instalar directamente.

1. En el menú **Inicio de AEM** vaya a **Herramientas** > **Implementación** > **Paquetes**.
1. Haga clic en **Cargar paquete** y elija el paquete descargado en el paso anterior. Haga clic en **Install** para instalar el paquete.

   ![Instalación del paquete GraphiQL](assets/explore-graphql-api/install-graphiql-package.png)

## Consultar una lista de fragmentos de contenido {#query-list-cf}

Un requisito habitual es consultar varios fragmentos de contenido.

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

1. Pulse el botón **Play** en el menú superior para ejecutar la consulta. Debería ver los resultados de los fragmentos de contenido de los colaboradores del capítulo anterior:

   ![Resultados de la lista de colaboradores](assets/explore-graphql-api/contributorlist-results.png)

1. Sitúe el cursor debajo del texto `_path` e introduzca **CTRL+Espacio** para activar las sugerencias de código. Agregue `fullName` y `occupation` a la consulta.

   ![Actualizar consulta con coincidencia de código](assets/explore-graphql-api/update-query-codehinting.png)

1. Vuelva a ejecutar la consulta presionando el botón **Play** y debería ver los resultados que incluyen las propiedades adicionales `fullName` y `occupation`.

   ![Nombres completos y resultados de ocupación](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` y  `occupation` son propiedades simples. Recuerde del capítulo [Definición de modelos de fragmento de contenido](./content-fragment-models.md) que `fullName` y `occupation` son los valores utilizados al definir el **Nombre de propiedad** de los campos respectivos.

1. `pictureReference` y  `biographyText` representan campos más complejos. Actualice la consulta con lo siguiente para devolver datos sobre los campos `pictureReference` y `biographyText`.

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biographyText {
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

   `biographyText` es un campo de texto multilínea y la API de GraphQL nos permite elegir una variedad de formatos para los resultados como  `html`,  `markdown`,  `json` o  `plaintext`.

   `pictureReference` es una referencia de contenido y se espera que sea una imagen, por lo que se utiliza un  `ImageRef` objeto integrado. Esto nos permite solicitar datos adicionales sobre la imagen a la que se hace referencia, como `width` y `height`.

1. A continuación, experimente consultando una lista de **aventuras**. Ejecute la siguiente consulta:

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

   Debería ver una lista de **Advertencias** devueltas. No dude en experimentar añadiendo campos adicionales a la consulta.

## Filtrar una lista de fragmentos de contenido {#filter-list-cf}

A continuación, veamos cómo es posible filtrar los resultados por un subconjunto de fragmentos de contenido basado en un valor de propiedad.

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

   La consulta anterior realiza una búsqueda con todos los colaboradores del sistema. El filtro añadido al principio de la consulta realizará una comparación en el campo `occupation` y en la cadena &quot;**Photograph**&quot;.

1. Ejecute la consulta, se espera que solo se devuelva un único **colaborador**.
1. Introduzca la siguiente consulta para consultar una lista de **Adventures** donde `adventureActivity` es **no** igual a **&quot;Surfing&quot;**:

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

Hay muchas otras opciones para filtrar y crear consultas complejas; los ejemplos anteriores son solo algunos ejemplos.

## Consultar un solo fragmento de contenido {#query-single-cf}

También es posible consultar directamente un solo fragmento de contenido. El contenido en AEM se almacena de forma jerárquica y el identificador único de un fragmento se basa en la ruta del fragmento. Si el objetivo es devolver datos sobre un solo fragmento, se prefiere utilizar la ruta y consultar el modelo directamente. El uso de esta sintaxis significa que la complejidad de la consulta será muy baja y generará un resultado más rápido.

1. Introduzca la siguiente consulta en el editor de GraphiQL:

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

1. Ejecute la consulta y observe que se devuelve el resultado único del fragmento **Stacey Roswells**.

   En el ejercicio anterior, se utilizaba un filtro para reducir la lista de resultados. Puede utilizar una sintaxis similar para filtrar por ruta, aunque la sintaxis anterior es preferible por motivos de rendimiento.

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
         biographyText {
           html
         }
       }
     }
   }
   ```

   Aunque la variación se denominó **Resumen**, las variaciones se conservan en minúsculas y, por lo tanto, se utiliza `summary`.

1. Ejecute la consulta y observe que el campo `biography` contiene un resultado mucho más corto `html`.

## Consulta de varios modelos de fragmento de contenido {#query-multiple-models}

También es posible combinar consultas independientes en una sola consulta. Esto resulta útil para minimizar el número de solicitudes HTTP necesarias para activar la aplicación. Por ejemplo, la vista *Home* de una aplicación puede mostrar contenido basado en **dos** diferentes modelos de fragmento de contenido. En lugar de ejecutar **dos** consultas independientes, podemos combinar las consultas en una sola solicitud.

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

1. Ejecute la consulta y observe que el conjunto de resultados contiene datos de **Adventures** y **Contributors**:

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

Para ver muchos más ejemplos de consultas de GraphQL, consulte: [Aprender a utilizar GraphQL con AEM: contenido de muestra y consultas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## Felicitaciones! {#congratulations}

¡Felicidades, acaba de crear y ejecutar varias consultas de GraphQL!

## Pasos siguientes {#next-steps}

En el siguiente capítulo, [Consulta de AEM desde una aplicación React](./graphql-and-external-app.md), explorará cómo una aplicación externa puede consultar los extremos de GraphQL de AEM. La aplicación externa modifica la aplicación WKND GraphQL React de ejemplo para añadir consultas GraphQL de filtrado, lo que permite al usuario de la aplicación filtrar aventuras por actividad. También se le introducirá en la gestión de errores básica.

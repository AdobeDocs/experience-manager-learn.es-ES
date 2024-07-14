---
title: 'Explore las API de GraphQL AEM: Introducción a la sin encabezado de la - GraphQL'
description: Introducción a Adobe Experience Manager AEM () y GraphQL. AEM Explore las API de GraphQL que se utilizan en el IDE integrado de GrapiQL para el uso de los clientes. AEM Descubra cómo genera automáticamente un esquema de GraphQL basado en un modelo de fragmento de contenido. Experimente construyendo consultas básicas usando la sintaxis de GraphQL.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
duration: 332
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 1%

---

# Explorar las API de GraphQL {#explore-graphql-apis}

La API de GraphQL AEM de proporciona un potente lenguaje de consulta para exponer los datos de los fragmentos de contenido a aplicaciones de flujo descendente. Los modelos de fragmento de contenido definen el esquema de datos que utilizan los fragmentos de contenido. Siempre que se crea o actualiza un modelo de fragmento de contenido, el esquema se traduce y se agrega al &quot;gráfico&quot; que conforma la API de GraphQL.

En este capítulo, vamos a explorar algunas consultas comunes de GraphQL para recopilar contenido mediante un IDE denominado [GraphiQL](https://github.com/graphql/graphiql). El IDE de GraphiQL permite probar y refinar rápidamente las consultas y los datos devueltos. También facilita el acceso a la documentación, lo que facilita la comprensión de los métodos disponibles.

## Requisitos previos {#prerequisites}

Este es un tutorial de varias partes y se da por hecho que se han completado los pasos descritos en [Creación de fragmentos de contenido](./author-content-fragments.md).

## Objetivos {#objectives}

* Aprenda a utilizar la herramienta GraphiQL para construir una consulta con sintaxis de GraphQL.
* Obtenga información sobre cómo consultar una lista de fragmentos de contenido y un solo fragmento de contenido.
* Obtenga información sobre cómo filtrar y solicitar atributos de datos específicos.
* Obtenga información sobre cómo unirse a una consulta de varios modelos de fragmentos de contenido
* Obtenga información sobre cómo mantener la consulta de GraphQL.

## Habilitación del punto de conexión de GraphQL {#enable-graphql-endpoint}

Se debe configurar un punto de conexión de GraphQL para habilitar las consultas de API de GraphQL para los fragmentos de contenido.

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **GraphQL**.

   ![Ir al extremo de GraphQL](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. Pulse **Crear** en la esquina superior derecha, en el cuadro de diálogo resultante, introduzca los siguientes valores:

   * Nombre*: **Mi Extremo De Proyecto**.
   * Usar esquema de GraphQL proporcionado por... *: **Mi proyecto**

   ![Crear extremo de GraphQL](assets/explore-graphql-api/create-graphql-endpoint.png)

   Pulse **Crear** para guardar el extremo.

   Los extremos de GraphQL creados en función de una configuración de proyecto solo permiten consultas con modelos que pertenecen a ese proyecto. En este caso, se pueden usar las únicas consultas contra los modelos **Person** y **Team**.

   >[!NOTE]
   >
   > También se puede crear un punto de conexión global para habilitar consultas de modelos en varias configuraciones. AEM Esto debe utilizarse con precaución, ya que puede abrir el entorno a vulnerabilidades de seguridad adicionales y aumentar la complejidad general de la administración de los recursos de la.

1. Ahora debería ver un extremo de GraphQL habilitado en su entorno.

   ![Puntos finales de graphql habilitados](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## Uso del IDE de GraphiQL

AEM La herramienta [GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) permite a los desarrolladores crear y probar consultas de contenido en el entorno de trabajo actual de la. La herramienta GraphiQL también permite a los usuarios **mantener o guardar** consultas para que las usen las aplicaciones cliente en una configuración de producción.

AEM A continuación, explore la potencia de la API de GraphQL de mediante el IDE integrado de GraphiQL.

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **Editor de consultas de GraphQL**.

   ![Vaya al IDE de GraphiQL](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > AEM En, es posible que las versiones anteriores del IDE de GraphiQL no estén integradas. Se puede instalar manualmente siguiendo estas [instrucciones](#install-graphiql).

1. En la esquina superior derecha, asegúrese de que el Extremo está establecido en **Mi extremo del proyecto**.

   ![Establecer extremo de GraphQL](assets/explore-graphql-api/set-my-project-endpoint.png)

Esto ampliará todas las consultas a los modelos creados en el proyecto **Mi proyecto**.

### Consulta de una lista de fragmentos de contenido {#query-list-cf}

Un requisito común es consultar varios fragmentos de contenido.

1. Pegue la siguiente consulta en el panel principal (reemplazando la lista de comentarios):

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. Presione el botón **Reproducir** en el menú superior para ejecutar la consulta. Debería ver los resultados de los fragmentos de contenido del capítulo anterior:

   ![Resultados de la lista de personas](assets/explore-graphql-api/all-teams-list.png)

1. Coloque el cursor debajo del texto `title` y escriba **CTRL+espacio** para almacenar en déclencheur las sugerencias para el código. Agregue `shortname` y `description` a la consulta.

   ![Actualizar consulta con visita de código](assets/explore-graphql-api/update-query-codehinting.png)

1. Vuelva a ejecutar la consulta presionando el botón **Reproducir** y debería ver que los resultados incluyen las propiedades adicionales de `shortname` y `description`.

   ![resultados de nombre corto y descripción](assets/explore-graphql-api/updated-query-shortname-description.png)

   `shortname` es una propiedad simple y `description` es un campo de texto multilínea y la API de GraphQL nos permite elegir varios formatos para los resultados como `html`, `markdown`, `json` o `plaintext`.

### Consulta de fragmentos anidados

A continuación, el experimento con la consulta está recuperando fragmentos anidados. Recuerde que el modelo **Equipo** hace referencia al modelo **Persona**.

1. Actualice la consulta para incluir la propiedad `teamMembers`. Recuerde que este es un campo **Referencia de fragmento** al modelo de persona. Se pueden devolver las propiedades del modelo Person:

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   Respuesta JSON:

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   AEM La capacidad de realizar consultas con fragmentos anidados es una práctica funcionalidad de la API de GraphQL de. En este ejemplo sencillo, el anidamiento tiene solo dos niveles de profundidad. Sin embargo, es posible anidar fragmentos aún más. Por ejemplo, si había un modelo **Address** asociado con una **Persona**, sería posible devolver datos de los tres modelos en una sola consulta.

### Filtrado de una lista de fragmentos de contenido {#filter-list-cf}

A continuación, veamos cómo es posible filtrar los resultados a un subconjunto de fragmentos de contenido en función de un valor de propiedad.

1. Introduzca la siguiente consulta en la interfaz de usuario de GraphiQL:

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   La consulta anterior realiza una búsqueda de todos los fragmentos de persona en el sistema. El filtro agregado al principio de la consulta realiza una comparación en el campo `name` y la cadena de variable `$name`.

1. En el panel **Variables de consulta**, escriba lo siguiente:

   ```json
   {"name": "John Doe"}
   ```

1. Ejecute la consulta, se espera que solo se devuelva el fragmento de contenido **Personas** con un valor de `John Doe`.

   ![Usar variables de consulta para filtrar](assets/explore-graphql-api/using-query-variables-filter.png)

   Hay muchas otras opciones para filtrar y crear consultas complejas. Vea [Aprender a usar GraphQL AEM con: contenido de muestra y consultas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html).

1. Mejore la consulta anterior para recuperar la imagen del perfil

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   `profilePicture` es una referencia de contenido y se espera que sea una imagen; por lo tanto, se utiliza el objeto `ImageRef` integrado. Esto nos permite solicitar datos adicionales sobre la imagen a la que se hace referencia, como `width` y `height`.

### Consultar un solo fragmento de contenido {#query-single-cf}

También es posible consultar directamente un solo fragmento de contenido. AEM El contenido de los fragmentos se almacena de manera jerárquica y el identificador único de un fragmento se basa en la ruta del fragmento.

1. Introduzca la siguiente consulta en el editor de GraphiQL:

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. Escriba lo siguiente para las **variables de consulta**:

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. Ejecute la consulta y observe que se devuelve el único resultado.

## Persistir consultas {#persist-queries}

AEM Una vez que el desarrollador esté satisfecho con la consulta y los datos de resultados devueltos por la consulta, el siguiente paso es almacenar o mantener la consulta en la que se va a realizar el. Las [consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html) son el mecanismo preferido para exponer la API de GraphQL a aplicaciones cliente. Una vez que una consulta ha persistido, se puede solicitar mediante una solicitud de GET y se puede almacenar en caché en las capas de Dispatcher y CDN. El rendimiento de las consultas persistentes es mucho mejor. Además de las ventajas de rendimiento, las consultas persistentes garantizan que los datos adicionales no se expongan accidentalmente a las aplicaciones cliente. Encontrará más detalles sobre [consultas persistentes aquí](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html).

A continuación, mantenga dos consultas simples, que se utilizan en el capítulo siguiente.

1. Introduzca la siguiente consulta en el IDE de GraphiQL:

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   Compruebe que la consulta funciona.

1. A continuación, pulse **Guardar como** e introduzca `all-teams` como **Nombre de la consulta**.

   La consulta debe mostrarse bajo **Consultas persistentes** en el carril izquierdo.

   ![Consulta persistente de todos los equipos](assets/explore-graphql-api/all-teams-persisted-query.png)
1. A continuación, pulse los puntos suspensivos **...** junto a la consulta persistente y pulse **Copiar URL** para copiar la ruta de acceso en el portapapeles.

   ![Copiar URL de consulta persistente](assets/explore-graphql-api/copy-persistent-query-url.png)

1. Abra una pestaña nueva y pegue la ruta copiada en el explorador:

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   Debe ser similar a la ruta anterior. Debe ver que se devuelven los resultados JSON de la consulta.

   Desglose de la dirección URL anterior:

   | Nombre | Descripción |
   | ---------|---------- |
   | `/graphql/execute.json` | Punto final de consulta persistente |
   | `/my-project` | Configuración del proyecto para `/conf/my-project` |
   | `/all-teams` | Nombre de la consulta persistente |

1. Vuelva al IDE de GraphiQL y utilice el botón más **+** para mantener la NUEVA consulta

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. Guarde la consulta como: `person-by-name`.
1. Debe tener dos consultas persistentes guardadas:

   ![Consultas persistentes finales](assets/explore-graphql-api/final-persisted-queries.png)


## Publish GraphQL Endpoint y consultas persistentes

Tras la revisión y verificación, publique `GraphQL Endpoint` y `Persisted Queries`

1. AEM En la pantalla Inicio de la, vaya a **Herramientas** > **General** > **GraphQL**.

1. Puntee en la casilla que está junto a **Mi extremo del proyecto** y luego en **Publish**

   ![Punto final de Publish GraphQL](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. AEM En la pantalla de inicio de la, vaya a **Herramientas** > **General** > **Editor de consultas de GraphQL**

1. Pulse la consulta **todos los equipos** del panel Consultas persistentes y pulse **Publish**

   ![Consultas persistentes de Publish](assets/explore-graphql-api/publish-persisted-query.png)

1. Repita el paso anterior para la consulta `person-by-name`

## Archivos de solución {#solution-files}

Descargue el contenido, los modelos y las consultas persistentes creados en los últimos tres capítulos: [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip)

## Recursos adicionales

Obtenga más información acerca de las consultas de GraphQL en [Aprender a usar GraphQL AEM con el contenido de muestra y las consultas de muestra](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html).

## Enhorabuena. {#congratulations}

¡Felicidades, ha creado y ejecutado varias consultas de GraphQL!

## Siguientes pasos {#next-steps}

En el capítulo siguiente, [Generar aplicación React](./graphql-and-react-app.md), explora cómo una aplicación externa puede consultar los extremos de GraphQL y utilizar estos dos extremos de consultas persistentes para crear consultas que se pueden usar para crear consultas de manera que se puedan usar en el caso de los extremos de AEM. También se le presentarán algunas funciones básicas de gestión de errores durante la ejecución de consultas de GraphQL.

## Instalación de la herramienta GraphiQL (opcional) {#install-graphiql}

AEM En, algunas versiones de la herramienta IDE de GraphiQL (6.X.X) que deben instalarse manualmente, utilizan las [instrucciones desde aquí](../how-to/install-graphiql-aem-6-5.md).


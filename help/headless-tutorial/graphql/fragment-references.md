---
title: Modelado de datos avanzado con referencias de fragmento - Introducción a AEM Headless - GraphQL
description: Introducción a Adobe Experience Manager (AEM) y GraphQL. Aprenda a utilizar la función Referencia de fragmento para el modelado de datos avanzado y para crear una relación entre dos fragmentos de contenido diferentes. Aprenda a modificar una consulta de GraphQL para incluir el campo de un modelo al que se hace referencia.
sub-product: activos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 1%

---


# Modelado de datos avanzado con referencias de fragmento

Es posible hacer referencia a un fragmento de contenido desde otro fragmento de contenido. Esto permite al usuario crear modelos de datos complejos con relaciones entre fragmentos.

En este capítulo, actualizará el modelo de aventura para incluir una referencia al modelo colaborador mediante el campo **Referencia de fragmento**. También aprenderá a modificar una consulta de GraphQL para incluir campos de un modelo al que se hace referencia.

## Requisitos previos

Se trata de un tutorial en varias partes y se da por hecho que se han completado los pasos descritos en las partes anteriores.

## Objetivos

En este capítulo, aprenderemos a:

* Actualizar un modelo de fragmento de contenido para utilizar el campo Referencia de fragmento
* Crear una consulta de GraphQL que devuelva campos de un modelo al que se hace referencia

## Agregar una referencia de fragmento {#add-fragment-reference}

Actualice el modelo de fragmento de contenido de aventura para añadir una referencia al modelo colaborador.

1. Abra un explorador nuevo y vaya a AEM.
1. En el menú **Inicio de AEM** vaya a **Herramientas** > **Recursos** > **Modelos de fragmento de contenido** > **Sitio de WKND**.
1. Abra el modelo de fragmento de contenido **Adventure**

   ![Abrir el modelo de fragmento de contenido de aventura](assets/fragment-references/adventure-content-fragment-edit.png)

1. En **Tipos de datos**, arrastre y suelte un campo **Referencia de fragmento** en el panel principal.

   ![Campo Agregar referencia de fragmento](assets/fragment-references/add-fragment-reference-field.png)

1. Actualice **Properties** para este campo con lo siguiente:

   * Procesar como - `fragmentreference`
   * Etiqueta de campo - **Colaborador de aventura**
   * Nombre de propiedad - `adventureContributor`
   * Tipo de modelo: seleccione el modelo **Contributor**
   * Ruta de acceso raíz - `/content/dam/wknd`

   ![Propiedades de referencia de fragmento](assets/fragment-references/fragment-reference-properties.png)

   El nombre de propiedad `adventureContributor` ahora se puede usar para hacer referencia a un fragmento de contenido de colaborador.

1. Guarde los cambios en el modelo.

## Asignar un colaborador a una aventura

Ahora que el modelo de fragmento de contenido de aventura se ha actualizado, podemos editar un fragmento existente y hacer referencia a un colaborador. Cabe señalar que la edición del modelo de fragmento de contenido *afecta* a cualquier fragmento de contenido existente creado a partir de él.

1. Vaya a **Assets** > **Files** > **WKND Site** > **English** > **Adventures** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**.

   ![Carpeta del Campamento Surf de Bali](assets/setup/bali-surf-camp-folder.png)

1. Haga clic en el fragmento de contenido **Bali Surf Camp** para abrir el Editor de fragmentos de contenido.
1. Actualice el campo **Colaborador de aventura** y seleccione un colaborador haciendo clic en el icono de carpeta.

   ![Seleccione Stacey Roswells como colaborador](assets/fragment-references/stacey-roswell-contributor.png)

   *Seleccionar una ruta a un fragmento de colaborador*

   ![ruta rellenada al colaborador](assets/fragment-references/populated-path.png)

   Observe que solo los fragmentos creados con el modelo **Contributor** pueden seleccionarse.

1. Guarde los cambios en el fragmento.

1. Repita los pasos anteriores para asignar un colaborador a aventuras como [Yosemite Backpacking](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) y [Colorado Rock Climbing](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing)

## Consultar fragmento de contenido anidado con GraphiQL

A continuación, realice una consulta para una aventura y añada propiedades anidadas del modelo colaborador al que se hace referencia. Utilizaremos la herramienta GraphiQL para verificar rápidamente la sintaxis de la consulta.

1. Vaya a la herramienta GraphiQL en AEM: [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. Introduzca la siguiente consulta:

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   La consulta anterior es para una sola Aventura por su ruta. La propiedad `adventureContributor` hace referencia al modelo del colaborador y, a continuación, podemos solicitar propiedades desde el fragmento de contenido anidado.

1. Ejecute la consulta y debe obtener un resultado como el siguiente:

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. Experimente con otras consultas como `adventureList` y agregue propiedades para el fragmento de contenido al que se hace referencia en `adventureContributor`.

## Actualizar la aplicación React para mostrar el contenido del colaborador

A continuación, actualice las consultas utilizadas por la aplicación React para incluir el nuevo colaborador y mostrar información sobre el colaborador como parte de la vista de detalles de Aventura.

1. Abra la aplicación WKND GraphQL React en su IDE.

1. Abra el archivo `src/components/AdventureDetail.js`.

   ![IDE de componente de detalle de aventura](assets/fragment-references/adventure-detail-ide.png)

1. Busque la función `adventureDetailQuery(_path)`. La función `adventureDetailQuery(..)` simplemente ajusta una consulta GraphQL de filtrado, que utiliza la sintaxis `<modelName>ByPath` de AEM para consultar un solo fragmento de contenido identificado por su ruta JCR.

1. Actualice la consulta para incluir información sobre el colaborador al que se hace referencia:

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
           item {
               _path
               adventureTitle
               adventureActivity
               adventureType
               adventurePrice
               adventureTripLength
               adventureGroupSize
               adventureDifficulty
               adventurePrice
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   Con esta actualización, se incluirán en la consulta propiedades adicionales sobre `adventureContributor`, `fullName`, `occupation` y `pictureReference`.

1. Inspeccione el componente `Contributor` incrustado en el archivo `AdventureDetail.js` en `function Contributor(...)`. Este componente renderizará el nombre, la ocupación y la imagen del colaborador si existen las propiedades.

   Se hace referencia al componente `Contributor` en el método `AdventureDetail(...)` `return`:

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. Guarde los cambios en el archivo.
1. Inicie la aplicación React si aún no se está ejecutando:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. Vaya a [http://localhost:3000](http://localhost:3000/) y haga clic en una aventura que tenga un colaborador al que se haga referencia. Ahora debería ver la información del colaborador listada debajo del **itinerario**:

   ![Colaborador añadido en la aplicación](assets/fragment-references/contributor-added-detail.png)

## Felicitaciones!{#congratulations}

Felicitaciones! Ha actualizado un modelo de fragmento de contenido existente para hacer referencia a un fragmento de contenido anidado mediante el campo **Referencia de fragmento**. También ha aprendido a modificar una consulta de GraphQL para incluir campos de un modelo al que se hace referencia.

## Pasos siguientes {#next-steps}

En el capítulo siguiente, [Implementación de producción mediante un entorno de AEM Publish](./production-deployment.md), obtenga información sobre los servicios de AEM Author y Publish y el patrón de implementación recomendado para aplicaciones sin periféricos. Actualizará una aplicación existente para utilizar variables de entorno para cambiar dinámicamente un extremo de GraphQL en función del entorno de destino. También aprenderá a configurar correctamente AEM para el uso compartido de recursos de origen cruzado (CORS).

---
title: Agregar componentes fijos editables a un SPA remoto
description: Aprenda a añadir componentes fijos editables a una SPA remota.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---

# Componentes fijos editables

Los componentes React editables se pueden &quot;corregir&quot; o codificar en las vistas de SPA. Esto permite a los desarrolladores colocar componentes compatibles con SPA Editor en las vistas de SPA y permitir a los usuarios crear el contenido de los componentes en AEM SPA Editor.

![Componentes fijos](./assets/spa-fixed-component/intro.png)

En este capítulo, reemplazamos el título de la vista Inicio, &quot;Aventuras actuales&quot;, que es texto codificado en `Home.js` con un componente de título fijo pero editable. Los componentes fijos garantizan la colocación del título, pero también permiten que el texto del título se cree y cambie fuera del ciclo de desarrollo.

## Actualizar la aplicación WKND

Para agregar un __Fijo__ a la vista Inicio:

+ Importe el componente Título del componente principal de React de AEM y regístrelo en el tipo de recurso del Título del proyecto
+ Coloque el componente Título editable en la vista Inicio de SPA

### Importar en el componente Título del componente principal de React de AEM

En la vista Inicio de SPA, sustituya el texto codificado `<h2>Current Adventures</h2>` con el componente Título de los componentes principales de React de AEM. Antes de poder utilizar el componente Título , debemos:

1. Importar el componente Título desde `@adobe/aem-core-components-react-base`
1. Regístrelo usando `withMappable` para que los desarrolladores puedan colocarlo en el SPA
1. Además, regístrese con `MapTo` para que se pueda usar en [componente contenedor más tarde](./spa-container-component.md).

Para ello:

1. Abra el proyecto SPA remoto en `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` en su IDE
1. Cree un componente React en `react-app/src/components/aem/AEMTitle.js`
1. Agregue el siguiente código a `AEMTitle.js`.

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

Lea los comentarios del código para conocer los detalles de implementación.

La variable `AEMTitle.js` debe tener el siguiente aspecto:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### Utilice el componente AEMTitle React

Ahora que el componente Título del componente principal de React de AEM está registrado en y disponible para su uso en la aplicación React, reemplace el texto de título codificado en la vista Inicio.

1. Editar `react-app/src/Home.js`
1. En el `Home()` en la parte inferior, sustituya el título codificado por el nuevo `AEMTitle` componente:

   ```
   <h2>Current Adventures</h2>
   ```

   con

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Actualizar `Home.js` con el siguiente código:

   ```
   ...
   import { AEMTitle } from './aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

La variable `Home.js` debe tener el siguiente aspecto:

![Home.js](./assets/spa-fixed-component/home-js.png)

## Cree el componente Título en AEM

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > Aplicación WKND__
1. Toque __Página principal__ y seleccione __Editar__ desde la barra de acciones superior
1. Select __Editar__ desde el selector de modo de edición en la parte superior derecha del Editor de páginas
1. Pase el ratón sobre el texto predeterminado del título debajo del logotipo de WKND y encima de la lista de aventuras, hasta que aparezca el contorno azul de edición
1. Toque para exponer la barra de acciones del componente y, a continuación, toque la __llave__  para editar

   ![Barra de acciones del componente Título](./assets/spa-fixed-component/title-action-bar.png)

1. Cree el componente Título :
   + Título: __Aventuras de WKND__
   + Tipo/tamaño: __H2__

      ![Cuadro de diálogo del componente Título](./assets/spa-fixed-component/title-dialog.png)

1. Toque __Listo__ para guardar
1. Previsualizar los cambios en AEM Editor SPA
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000) y vea que el título creado cambia inmediatamente reflejado.

   ![Componente de título en SPA](./assets/spa-fixed-component/title-final.png)

## Felicitaciones!

¡Ha agregado un componente fijo y editable a la aplicación WKND! Ahora sabe cómo:

+ Importe y vuelva a utilizar un componente principal de React AEM en el SPA
+ Agregue un componente fijo, pero editable, al SPA
+ Cree el componente fijo en AEM
+ Ver el contenido creado en el SPA remoto

## Pasos siguientes

Los siguientes pasos son para [añadir un componente contenedor AEM ResponsiveGrid](./spa-container-component.md) a la SPA que permite al autor añadir y editar componentes a la SPA!

---
title: Agregar componentes fijos editables a un SPA remoto
description: Aprenda a añadir componentes fijos editables a una SPA remota.
topic: Sin cabeza, SPA, desarrollo
feature: Editor de SPA, componentes principales, API, desarrollo
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 1%

---


# Componentes fijos editables

Los componentes React editables se pueden &quot;corregir&quot; o codificar en las vistas de SPA. Esto permite a los desarrolladores colocar componentes compatibles con SPA Editor en las vistas de SPA y permitir a los usuarios crear el contenido de los componentes en AEM SPA Editor.

![Componentes fijos](./assets/spa-fixed-component/intro.png)

En este capítulo, reemplazamos el título de la vista Inicio, &quot;Aventuras actuales&quot;, que es texto codificado en `Home.js` con un componente de Título fijo pero editable. Los componentes fijos garantizan la colocación del título, pero también permiten que el texto del título se cree y cambie fuera del ciclo de desarrollo.

## Actualizar la aplicación WKND

Para añadir un componente Fijo a la vista Inicio:

+ Importe el componente Título del componente principal de React de AEM y regístrelo en el tipo de recurso del Título del proyecto
+ Coloque el componente Título editable en la vista Inicio de SPA

### Importar en el componente Título del componente principal de React de AEM

En la vista Inicio de SPA, sustituya el texto codificado `<h2>Current Adventures</h2>` por el componente Título de los componentes principales de React AEM. Antes de poder utilizar el componente Título , debemos:

1. Importar el componente Título desde `@adobe/aem-core-components-react-base`
1. Regístrela mediante `withMappable` para que los desarrolladores puedan colocarla en el SPA
1. Además, regístrese con `MapTo` para que se pueda utilizar en el [componente contenedor más adelante](./spa-container-component.md).

Para ello:

1. Abra el proyecto SPA remoto en `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` en su IDE
1. Crear un componente React en `react-app/src/components/aem/AEMTitle.js`
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

El archivo `AEMTitle.js` debería tener el siguiente aspecto:

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### Utilice el componente AEMTitle React

Ahora que el componente Título del componente principal de React de AEM está registrado en y disponible para su uso en la aplicación React, reemplace el texto de título codificado en la vista Inicio.

1. Editar `react-app/src/App.js`
1. en `Home()` en la parte inferior, sustituya el título codificado por el nuevo componente `AEMTitle`:

   ```
   <h2>Current Adventures</h2>
   ```

   con

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   Actualice `Apps.js` con el siguiente código:

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
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

El archivo `Apps.js` debería tener el siguiente aspecto:

![App.js](./assets/spa-fixed-component/app-js.png)

## Cree el componente Título en AEM

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > WKND App__
1. Pulse __Inicio__ y seleccione __Editar__ en la barra de acciones superior
1. Seleccione __Editar__ en el selector de modo de edición en la parte superior derecha del Editor de páginas
1. Pase el ratón sobre el texto predeterminado del título debajo del logotipo de WKND y encima de la lista de aventuras, hasta que aparezca el contorno azul de edición
1. Pulse para exponer la barra de acciones del componente y, a continuación, pulse la __llave inglesa__ para editar

   ![Barra de acciones del componente Título](./assets/spa-fixed-component/title-action-bar.png)

1. Cree el componente Título :
   + Título: __Aventuras de WKND__
   + Tipo/tamaño: __H2__

      ![Cuadro de diálogo del componente Título](./assets/spa-fixed-component/title-dialog.png)

1. Toque __Listo__ para guardar
1. Previsualizar los cambios en AEM Editor SPA
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000) y vea los cambios de título creados inmediatamente reflejados.

   ![Componente de título en SPA](./assets/spa-fixed-component/title-final.png)

## Felicitaciones!

¡Ha agregado un componente fijo y editable a la aplicación WKND! Ahora sabe cómo:

+ Importe y vuelva a utilizar un componente principal de React AEM en el SPA
+ Agregue un componente fijo, pero editable, al SPA
+ Cree el componente fijo en AEM
+ Ver el contenido creado en el SPA remoto

## Pasos siguientes

Los siguientes pasos son [agregar un componente de contenedor AEM ResponsiveGrid](./spa-container-component.md) a la SPA que permite al autor agregar y editar componentes a la SPA!

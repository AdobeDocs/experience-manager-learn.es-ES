---
title: Adición de componentes fijos editables a una SPA remota
description: Aprenda a añadir componentes fijos editables a una SPA remota.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer
level: Beginner
jira: KT-7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
duration: 164
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 1%

---

# Componentes fijos editables

{{spa-editor-deprecation}}

Los componentes de React editables pueden ser &quot;fijos&quot; o codificados en las vistas de la SPA. Esto permite a los desarrolladores colocar componentes compatibles con el Editor de SPA en las vistas de SPA y a los usuarios crear el contenido de los componentes en el Editor de SPA de AEM.

![Componentes fijos](./assets/spa-fixed-component/intro.png)

En este capítulo, reemplazamos el título de la vista Inicio, &quot;Aventuras actuales&quot;, que es texto codificado en `Home.js` con un componente de título fijo pero editable. Los componentes fijos garantizan la colocación del título, pero también permiten crear el texto del título y cambiarlo fuera del ciclo de desarrollo.

## Actualización de la aplicación WKND

Para agregar un componente __Fixed__ a la vista Inicio:

* Cree un componente de título editable personalizado y regístrelo en el tipo de recurso Título del proyecto
* Coloque el componente Título editable en la vista Inicio de la SPA

### Crear un componente de título de React editable

En la vista Inicio de la SPA, reemplace el texto predefinido `<h2>Current Adventures</h2>` por un componente de título editable personalizado. Para poder utilizar el componente Título, debemos hacer lo siguiente:

1. Crear un componente React de título personalizado
1. Decore el componente Título personalizado con los métodos de `@adobe/aem-react-editable-components` para que se pueda editar.
1. Registre el componente de título editable con `MapTo` para que pueda usarse en [componente contenedor más adelante](./spa-container-component.md).

Para ello, haga lo siguiente:

1. Abrir proyecto de SPA remoto en `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` en su IDE
1. Crear un componente de React en `react-app/src/components/editable/core/Title.js`
1. Agregue el siguiente código a `Title.js`.

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   Tenga en cuenta que este componente de React aún no se puede editar con AEM SPA Editor. Este componente base se podrá editar en el siguiente paso.

   Lea los comentarios del código para obtener más información sobre la implementación.

1. Crear un componente de React en `react-app/src/components/editable/EditableTitle.js`
1. Agregue el siguiente código a `EditableTitle.js`.

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   Este componente de React `EditableTitle` ajusta el componente de React `Title`, ajustándolo y decorándolo para que pueda editarse en el Editor de SPA de AEM.

### Uso del componente React EditableTitle

Ahora que el componente React Título editable está registrado en y disponible para usar en la aplicación React, reemplace el texto de título predefinido en la vista Inicio.

1. Editar `react-app/src/components/Home.js`
1. En `Home()`, en la parte inferior, importe `EditableTitle` y reemplace el título predefinido por el nuevo componente `AEMTitle`:

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

El archivo `Home.js` debe tener el siguiente aspecto:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## Crear el componente Título en AEM

1. Iniciar sesión en AEM Author
1. Vaya a __Sitios > Aplicación WKND__
1. Pulse __Inicio__ y seleccione __Editar__ en la barra de acciones superior
1. Seleccione __Editar__ del selector de modo de edición en la parte superior derecha del Editor de páginas
1. Pase el ratón sobre el texto del título predeterminado debajo del logotipo de WKND y encima de la lista de aventuras hasta que se muestre el contorno de edición azul
1. Puntee para mostrar la barra de acciones del componente y, a continuación, puntee en la __llave inglesa__ que desea editar

   ![Barra de acciones del componente Título](./assets/spa-fixed-component/title-action-bar.png)

1. Cree el componente Título:
   1. Título: __WKND Adventures__
   1. Tipo/Tamaño: __H2__

      ![Cuadro de diálogo del componente Título](./assets/spa-fixed-component/title-dialog.png)

1. Pulse __Listo__ para guardar
1. Previsualice los cambios en el Editor de SPA de AEM
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000) y vea los cambios de título creados inmediatamente reflejados.

   ![Componente de título en la SPA](./assets/spa-fixed-component/title-final.png)

## Enhorabuena.

¡Ha agregado un componente fijo editable a la aplicación WKND! Ahora ya sabe cómo:

* Se ha creado un componente fijo, pero editable, en la SPA
* Crear el componente fijo en AEM
* Ver el contenido creado en la SPA remota

## Próximos pasos

Los siguientes pasos son [agregar un componente contenedor de AEM ResponsiveGrid](./spa-container-component.md) a la SPA que permita al autor agregar y editar componentes a la SPA.

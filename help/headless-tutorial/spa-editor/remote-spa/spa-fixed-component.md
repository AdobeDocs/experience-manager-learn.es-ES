---
title: SPA Agregar componentes fijos editables a un grupo de informes remoto
description: SPA Obtenga información sobre cómo agregar componentes fijos editables a un grupo de informes.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
duration: 212
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 1%

---

# Componentes fijos editables

SPA Los componentes de React editables pueden ser &quot;fijos&quot; o codificados en las vistas de la. SPA SPA AEM SPA Esto permite a los desarrolladores colocar componentes compatibles con el Editor de la en las vistas de componentes y permitir a los usuarios crear el contenido de los componentes en el Editor de la.

![Componentes fijos](./assets/spa-fixed-component/intro.png)

En este capítulo, reemplazamos el título de la vista Inicio, &quot;Aventuras actuales&quot;, que es texto codificado en `Home.js` con un componente de título fijo, pero editable. Los componentes fijos garantizan la colocación del título, pero también permiten crear el texto del título y cambiarlo fuera del ciclo de desarrollo.

## Actualización de la aplicación WKND

Para agregar un __Fijo__ a la vista Inicio:

+ Cree un componente de título editable personalizado y regístrelo en el tipo de recurso Título del proyecto
+ SPA Coloque el componente Título editable en la vista Inicio de la

### Crear un componente de título de React editable

SPA En la vista Página de inicio de la aplicación, reemplace el texto codificado de forma rígida `<h2>Current Adventures</h2>` con un componente de título editable personalizado. Para poder utilizar el componente Título, debemos hacer lo siguiente:

1. Crear un componente React de título personalizado
1. Decorar el componente Título personalizado con métodos de `@adobe/aem-react-editable-components` para que sea editable.
1. Registre el componente de título editable con `MapTo` por lo que se puede utilizar en [componente de contenedor posterior](./spa-container-component.md).

Para ello, haga lo siguiente:

1. SPA Abra el proyecto remoto de en `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` en su IDE
1. Cree un componente de React en `react-app/src/components/editable/core/Title.js`
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

   AEM SPA Tenga en cuenta que este componente de React aún no se puede editar con el Editor de. Este componente base se podrá editar en el siguiente paso.

   Lea los comentarios del código para obtener más información sobre la implementación.

1. Cree un componente de React en `react-app/src/components/editable/EditableTitle.js`
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

   Esta `EditableTitle` El componente React ajusta el `Title` AEM SPA Reaccione el componente, envolviéndolo y decorándolo para que pueda editarse en el Editor de.

### Uso del componente React EditableTitle

Ahora que el componente React Título editable está registrado en y disponible para usar en la aplicación React, reemplace el texto de título predefinido en la vista Inicio.

1. Editar `react-app/src/components/Home.js`
1. En el `Home()` en la parte inferior, importe `EditableTitle` y reemplace el título codificado por el nuevo `AEMTitle` componente:

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

El `Home.js` el archivo debe tener un aspecto similar al siguiente:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## AEM Cree el componente Título en el menú de

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sitios > Aplicación WKND__
1. Tocar __Inicio__ y seleccione __Editar__ desde la barra de acciones superior
1. Seleccionar __Editar__ en el selector de modo de edición, en la parte superior derecha del Editor de páginas
1. Pase el ratón sobre el texto del título predeterminado debajo del logotipo de WKND y encima de la lista de aventuras hasta que se muestre el contorno de edición azul
1. Pulse para mostrar la barra de acciones del componente y, a continuación, pulse el botón __llave__  para editar

   ![Barra de acciones del componente Título](./assets/spa-fixed-component/title-action-bar.png)

1. Cree el componente Título:
   + Título: __WKND Adventures__
   + Tipo/tamaño: __H2__

     ![Cuadro de diálogo Componente Título](./assets/spa-fixed-component/title-dialog.png)

1. Tocar __Listo__ para guardar
1. AEM SPA Previsualice los cambios en el editor de
1. Actualice la aplicación WKND ejecutándose localmente en [http://localhost:3000](http://localhost:3000) y ver los cambios de título creados inmediatamente reflejados.

   ![SPA Componente de título en la](./assets/spa-fixed-component/title-final.png)

## Enhorabuena.

¡Ha agregado un componente fijo editable a la aplicación WKND! Ahora ya sabe cómo:

+ SPA Se ha creado un componente fijo, pero editable, para el grupo de informes de
+ AEM Creación del componente fijo en el entorno de trabajo de
+ SPA Ver el contenido creado en la interfaz de usuario de la aplicación remota

## Pasos siguientes

Los siguientes pasos son [AEM añadir un componente contenedor de cuadrícula interactiva de la aplicación de datos](./spa-container-component.md) SPA SPA a la que permite al autor añadir y editar componentes a la.

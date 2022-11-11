---
title: Agregar componentes fijos editables a un SPA remoto
description: Aprenda a añadir componentes fijos editables a una SPA remota.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# Componentes fijos editables

Los componentes React editables se pueden &quot;corregir&quot; o codificar en las vistas de SPA. Esto permite a los desarrolladores colocar componentes compatibles con SPA Editor en las vistas de SPA y permitir a los usuarios crear el contenido de los componentes en AEM SPA Editor.

![Componentes fijos](./assets/spa-fixed-component/intro.png)

En este capítulo, reemplazamos el título de la vista Inicio, &quot;Aventuras actuales&quot;, que es texto codificado en `Home.js` con un componente de título fijo pero editable. Los componentes fijos garantizan la colocación del título, pero también permiten que el texto del título se cree y cambie fuera del ciclo de desarrollo.

## Actualizar la aplicación WKND

Para agregar un __Fijo__ a la vista Inicio:

+ Cree un componente Título editable personalizado y regístrelo en el tipo de recurso Título del proyecto
+ Coloque el componente Título editable en la vista Inicio de SPA

### Creación de un componente de título React editable

En la vista Inicio de SPA, sustituya el texto codificado `<h2>Current Adventures</h2>` con un componente Título editable personalizado. Antes de poder utilizar el componente Título , debemos:

1. Crear un componente Reacción de título personalizado
1. Decoración del componente Título personalizado utilizando métodos de `@adobe/aem-react-editable-components` para que sea editable.
1. Registre el componente Título editable con `MapTo` para que se pueda usar en [componente contenedor más tarde](./spa-container-component.md).

Para ello:

1. Abra el proyecto SPA remoto en `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` en su IDE
1. Cree un componente React en `react-app/src/components/editable/core/Title.js`
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

   Tenga en cuenta que este componente React aún no se puede editar con AEM SPA Editor. Este componente base se convertirá en editable en el siguiente paso.

   Lea los comentarios del código para conocer los detalles de implementación.

1. Cree un componente React en `react-app/src/components/editable/EditableTitle.js`
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

   Esta `EditableTitle` El componente React ajusta el `Title` Reaccione el componente, ajuste y decorándolo para que se pueda editar en AEM Editor SPA.

### Uso del componente ReactEditableTitle

Ahora que el componente React de título editable está registrado en y disponible para su uso en la aplicación React, reemplace el texto de título codificado en la vista Inicio.

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

La variable `Home.js` debe tener el siguiente aspecto:

![Home.js](./assets/spa-fixed-component/home-js-update.png)

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

## ¡Enhorabuena!

¡Ha agregado un componente fijo y editable a la aplicación WKND! Ahora sabe cómo:

+ Se ha creado un componente fijo, pero editable, en el SPA
+ Cree el componente fijo en AEM
+ Ver el contenido creado en el SPA remoto

## Pasos siguientes

Los siguientes pasos son para [añadir un componente contenedor AEM ResponsiveGrid](./spa-container-component.md) a la SPA que permite al autor añadir y editar componentes a la SPA!

---
title: SPA Agregar componentes editables del contenedor de React a un grupo de informes remoto
description: SPA AEM Obtenga información sobre cómo agregar componentes de contenedor editables a una aplicación remota que permite a los autores de la arrastrar y soltar componentes en ellos.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
duration: 306
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 1%

---

# Componentes de contenedor editables

SPA [Los componentes fijos](./spa-fixed-component.md) proporcionan cierta flexibilidad para la creación de contenido de la aplicación, pero este enfoque es rígido y requiere que los desarrolladores definan la composición exacta del contenido editable. SPA SPA Para admitir la creación de experiencias excepcionales por parte de los autores, Editor de la admite el uso de componentes de contenedor en la creación de experiencias de los autores. Los componentes de contenedor permiten a los autores arrastrar y soltar los componentes permitidos en el contenedor y crearlos, tal como lo pueden hacer en la creación tradicional de AEM Sites.

![Componentes de contenedor editables](./assets/spa-container-component/intro.png)

SPA En este capítulo, agregamos un contenedor editable a la vista de inicio que permite a los autores componer y diseñar experiencias de contenido enriquecido mediante componentes de React editables directamente en la.

## Actualización de la aplicación WKND

Para agregar un componente de contenedor a la vista Inicio:

+ AEM Importar el componente `ResponsiveGrid` del componente editable de React de la
+ Importar y registrar componentes React editables personalizados (texto e imagen) para utilizarlos en el componente Cuadrícula interactiva

### Uso del componente Cuadrícula interactiva

Para agregar un área editable a la vista Inicio:

1. Abrir y editar `react-app/src/components/Home.js`
1. Importar el componente `ResponsiveGrid` desde `@adobe/aem-react-editable-components` y agregarlo al componente `Home`.
1. Establezca los siguientes atributos en el componente `<ResponsiveGrid...>`
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   AEM Esto indica al componente `ResponsiveGrid` que recupere su contenido del recurso de la:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   AEM AEM AEM El elemento `itemPath` se asigna al nodo `responsivegrid` definido en la plantilla de `Remote SPA Page` y se crea automáticamente en las nuevas páginas de creadas a partir de la plantilla de publicación `Remote SPA Page`.

   Actualice `Home.js` para agregar el componente `<ResponsiveGrid...>`.

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

El archivo `Home.js` debe tener el siguiente aspecto:

![Home.js](./assets/spa-container-component/home-js.png)

## Crear componentes editables

SPA Para obtener el máximo efecto de la flexibilidad que ofrecen los contenedores de experiencia de creación en el Editor de. Ya hemos creado un componente Título editable, pero vamos a hacer algunos más que permitan a los autores utilizar componentes Texto e Imagen editables en el componente Cuadrícula interactiva recién agregado.

Los nuevos componentes Texto editable y Reacción de imagen se crean utilizando el patrón de definición de componente editable exportado en [componentes fijos editables](./spa-fixed-component.md).

### Componente de texto editable

1. SPA Abra el proyecto de la en su IDE
1. Crear un componente de React en `src/components/editable/core/Text.js`
1. Agregar el siguiente código a `Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. Crear un componente React editable en `src/components/editable/EditableText.js`
1. Agregar el siguiente código a `EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

La implementación del componente Texto editable debería tener un aspecto similar al siguiente:

![Componente de texto editable](./assets/spa-container-component/text-js.png)

### Componente de imagen

1. SPA Abra el proyecto de la en su IDE
1. Crear un componente de React en `src/components/editable/core/Image.js`
1. Agregar el siguiente código a `Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. Crear un componente React editable en `src/components/editable/EditableImage.js`
1. Agregar el siguiente código a `EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. Cree un archivo SCSS `src/components/editable/EditableImage.scss` que proporcione estilos personalizados para `EditableImage.scss`. Estos estilos se dirigen a las clases CSS del componente React editable.
1. Agregar el SCSS siguiente a `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importar `EditableImage.scss` en `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

La implementación del componente Imagen editable debe tener un aspecto similar al siguiente:

![Componente de imagen editable](./assets/spa-container-component/image-js.png)


### Importar los componentes editables

SPA AEM Se hace referencia en el documento a los componentes `EditableText` y `EditableImage` React recién creados y se crean instancias de manera dinámica en función del JSON devuelto por los componentes de React. SPA Para asegurarse de que estos componentes están disponibles para los usuarios, cree instrucciones de importación para ellos en `Home.js`

1. SPA Abra el proyecto de la en su IDE
1. Abrir el archivo `src/Home.js`
1. Agregar instrucciones de importación para `AEMText` y `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

El resultado debería ser similar al siguiente:

![Home.js](./assets/spa-container-component/home-js-imports.png)

SPA Si se agregan estas importaciones _no_, el código `EditableText` y `EditableImage` no se invocará por parte de los usuarios, y por lo tanto, los componentes no se asignan a los tipos de recursos proporcionados.

## AEM Configuración del contenedor en la

AEM Los componentes de contenedor de utilizan directivas para dictar los componentes permitidos. SPA AEM SPA SPA Se trata de una configuración esencial al utilizar el Editor de componentes, ya que solo los componentes de la aplicación que tienen asignados componentes de la interfaz de usuario de la aplicación de componentes se pueden procesar mediante el Editor de componentes de la. SPA Asegúrese de que solo se permiten los componentes para los que hemos proporcionado implementaciones de la:

+ `EditableTitle` se asignó a `wknd-app/components/title`
+ `EditableText` se asignó a `wknd-app/components/text`
+ `EditableImage` se asignó a `wknd-app/components/image`

SPA Para configurar el contenedor de cuadrícula adaptable de la plantilla de página de remoto:

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Herramientas > General > Plantillas > Aplicación WKND__
1. SPA Editar __Página de informes__

   ![Políticas de cuadrícula adaptable](./assets/spa-container-component/templates-remote-spa-page.png)

1. Seleccione __Estructura__ en el conmutador de modo en la parte superior derecha
1. Pulse para seleccionar __Contenedor de diseño__
1. Pulse el icono __Directiva__ en la barra emergente

   ![Políticas de cuadrícula adaptable](./assets/spa-container-component/templates-policies-action.png)

1. A la derecha, en la ficha __Componentes permitidos__, expanda __WKND APP - CONTENT__
1. Asegúrese de que solo están seleccionadas las siguientes opciones:
   + Imagen
   + Texto
   + Título

   SPA ![Página de acceso remoto](./assets/spa-container-component/templates-allowed-components.png)

1. Pulse __Listo__

## AEM Creación del contenedor en la

SPA AEM Después de actualizar el elemento para incrustar el elemento `<ResponsiveGrid...>`, los contenedores para tres componentes editables de React (`EditableTitle`, `EditableText` y `EditableImage`), y de actualizar el elemento con una directiva de plantilla coincidente, se puede empezar a crear contenido en el componente contenedor, lo cual se actualiza de forma independiente.

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sitios > Aplicación WKND__
1. Pulse __Inicio__ y seleccione __Editar__ en la barra de acciones superior
   + AEM Se muestra un componente de texto &quot;Hello World&quot;, ya que este se añadió automáticamente al generar el proyecto a partir del arquetipo de proyecto de la lista de tipos de archivo
1. Seleccione __Editar__ en el selector de modo en la parte superior derecha del Editor de páginas
1. Busque el área editable __Contenedor de diseño__ debajo del título
1. Abra la __barra lateral del editor de páginas__ y seleccione la __vista Componentes__
1. Arrastre los siguientes componentes al __contenedor de diseño__
   + Imagen
   + Título
1. Arrastre los componentes para reordenarlos al siguiente orden:
   1. Título
   1. Imagen
   1. Texto
1. __Autor__ del componente __Título__
   1. Pulse el componente Título y pulse el icono __llave inglesa__ para __editar__ el componente Título
   1. Añada el siguiente texto:
      + Título: __Se acerca el verano, aprovechemos al máximo__
      + Tipo: __H1__
   1. Pulse __Listo__
1. __Autor__ del componente __Imagen__
   1. Arrastre una imagen desde la barra lateral (después de cambiar a la vista de Assets) en el componente Imagen
   1. Pulse el componente Imagen y pulse el icono __llave inglesa__ para editar
   1. Marque la casilla __La imagen es decorativa__
   1. Pulse __Listo__
1. __Autor__ del componente __Texto__
   1. Edite el componente Texto tocando el componente Texto y pulsando el icono __wrench__
   1. Añada el siguiente texto:
      + _Ahora mismo, puedes obtener un 15% de descuento en todas las aventuras de una semana y un 20% de descuento en todas las aventuras de dos semanas o más. Al finalizar la compra, agrega el código de campaña SUMMERISCOMING para obtener tus descuentos._
   1. Pulse __Listo__

1. Los componentes ya se han creado, pero se apilan verticalmente.

   ![Componentes creados](./assets/spa-container-component/authored-components.png)

AEM Utilice el modo Diseño de la para permitirnos ajustar el tamaño y el diseño de los componentes.

1. Cambie a __Modo de diseño__ con el selector de modo en la esquina superior derecha
1. __Cambiar el tamaño__ de los componentes Imagen y Texto, de manera que estén uno al lado del otro
   + El componente __Image__ debe tener __8 columnas de ancho__
   + El componente __Texto__ debe tener __3 columnas de ancho__

   ![Componentes de diseño](./assets/spa-container-component/layout-components.png)

1. AEM __Vista previa__ de sus cambios en el Editor de páginas de la página de la
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000) para ver los cambios creados.

   SPA ![Componente de contenedor en el código de tiempo ](./assets/spa-container-component/localhost-final.png)


## Enhorabuena.

Ha agregado un componente contenedor que permite a los autores agregar componentes editables a la aplicación WKND. Ahora ya sabe cómo:

+ AEM SPA Usar el componente `ResponsiveGrid` del componente editable de React de la
+ SPA Cree y registre componentes React editables (texto e imagen) para usarlos en la lista de componentes a través del componente de contenedor de
+ SPA SPA Configure la plantilla Página de administración remota de para permitir los componentes habilitados para la creación de informes
+ Agregar componentes editables al componente contenedor
+ SPA Componentes de autor y diseño en el editor de

## Siguientes pasos

SPA El siguiente paso usa esta misma técnica para [agregar un componente editable a una ruta de detalles de aventura](./spa-dynamic-routes.md) en el.

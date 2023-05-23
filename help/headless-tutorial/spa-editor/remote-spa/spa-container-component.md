---
title: SPA Agregar componentes editables del contenedor de React a un grupo de informes remoto
description: SPA AEM Obtenga información sobre cómo agregar componentes de contenedor editables a una aplicación remota que permite a los autores de la arrastrar y soltar componentes en ellos.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 2%

---

# Componentes de contenedor editables

[Componentes fijos](./spa-fixed-component.md) SPA proporcionan cierta flexibilidad para la creación de contenido de la, pero este enfoque es rígido y requiere que los desarrolladores definan la composición exacta del contenido editable. SPA SPA Para admitir la creación de experiencias excepcionales por parte de los autores, Editor de la admite el uso de componentes de contenedor en la creación de experiencias de los autores. Los componentes de contenedor permiten a los autores arrastrar y soltar los componentes permitidos en el contenedor y crearlos, tal como lo pueden hacer en la creación tradicional de AEM Sites.

![Componentes de contenedor editables](./assets/spa-container-component/intro.png)

SPA En este capítulo, agregamos un contenedor editable a la vista de inicio que permite a los autores componer y diseñar experiencias de contenido enriquecido mediante componentes de React editables directamente en la.

## Actualización de la aplicación WKND

Para agregar un componente de contenedor a la vista Inicio:

+ AEM Importar el componente editable de React de la `ResponsiveGrid` componente
+ Importar y registrar componentes React editables personalizados (texto e imagen) para utilizarlos en el componente Cuadrícula interactiva

### Uso del componente Cuadrícula interactiva

Para agregar un área editable a la vista Inicio:

1. Abrir y editar `react-app/src/components/Home.js`
1. Importe el `ResponsiveGrid` componente de `@adobe/aem-react-editable-components` y agréguelo a la `Home` componente.
1. Establezca los siguientes atributos en la variable `<ResponsiveGrid...>` componente
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Esto indica a la `ResponsiveGrid` AEM para recuperar su contenido del recurso de la:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   El `itemPath` se asigna a `responsivegrid` nodo definido en la `Remote SPA Page` AEM AEM La plantilla y la plantilla se crean automáticamente en las nuevas páginas de creadas a partir de `Remote SPA Page` AEM Plantilla de.

   Actualizar `Home.js` para añadir el `<ResponsiveGrid...>` componente.

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

El `Home.js` el archivo debe tener un aspecto similar al siguiente:

![Home.js](./assets/spa-container-component/home-js.png)

## Crear componentes editables

SPA Para obtener el máximo efecto de la flexibilidad que ofrecen los contenedores de experiencia de creación en el Editor de. Ya hemos creado un componente Título editable, pero vamos a hacer algunos más que permitan a los autores utilizar componentes Texto e Imagen editables en el componente Cuadrícula interactiva recién agregado.

Los nuevos componentes Texto editable y Reacción de imagen se crean utilizando el patrón de definición de componente editable exportado en [componentes fijos editables](./spa-fixed-component.md).

### Componente de texto editable

1. SPA Abra el proyecto de la en su IDE
1. Cree un componente de React en `src/components/editable/core/Text.js`
1. Agregue el siguiente código a `Text.js`

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

1. Cree un componente React editable en `src/components/editable/EditableText.js`
1. Agregue el siguiente código a `EditableText.js`

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
1. Cree un componente de React en `src/components/editable/core/Image.js`
1. Agregue el siguiente código a `Image.js`

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

1. Cree un componente React editable en `src/components/editable/EditableImage.js`
1. Agregue el siguiente código a `EditableImage.js`

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


1. Creación de un archivo SCSS `src/components/editable/EditableImage.scss` que proporciona estilos personalizados para `EditableImage.scss`. Estos estilos se dirigen a las clases CSS del componente React editable.
1. Agregue el siguiente SCSS a `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importar `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

La implementación del componente Imagen editable debe tener un aspecto similar al siguiente:

![Componente de imagen editable](./assets/spa-container-component/image-js.png)


### Importar los componentes editables

El recién creado `EditableText` y `EditableImage` SPA AEM En el informe se hace referencia a los componentes de React y se crean instancias de forma dinámica en función del JSON devuelto por los componentes de React. SPA Para asegurarse de que estos componentes están disponibles para el usuario, cree instrucciones de importación para ellos en `Home.js`

1. SPA Abra el proyecto de la en su IDE
1. Abra el archivo `src/Home.js`
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

Si estas importaciones son _no_ se ha añadido, la variable `EditableText` y `EditableImage` SPA no invoca el código de y, por lo tanto, los componentes no se asignan a los tipos de recursos proporcionados.

## AEM Configuración del contenedor en la

AEM Los componentes de contenedor de utilizan directivas para dictar los componentes permitidos. SPA AEM SPA SPA Se trata de una configuración esencial al utilizar el Editor de componentes, ya que solo los componentes de la aplicación que tienen asignados componentes de la interfaz de usuario de la aplicación de componentes se pueden procesar mediante el Editor de componentes de la. SPA Asegúrese de que solo se permiten los componentes para los que hemos proporcionado implementaciones de la:

+ `EditableTitle` asignado a `wknd-app/components/title`
+ `EditableText` asignado a `wknd-app/components/text`
+ `EditableImage` asignado a `wknd-app/components/image`

SPA Para configurar el contenedor de cuadrícula adaptable de la plantilla de página de remoto:

1. Inicie sesión en AEM Author
1. Vaya a __Herramientas > General > Plantillas > Aplicación WKND__
1. Editar __SPA Página de informe__

   ![Políticas de cuadrícula interactiva](./assets/spa-container-component/templates-remote-spa-page.png)

1. Seleccionar __Estructura__ en el interruptor de modo, en la parte superior derecha
1. Pulse para seleccionar __Contenedor de diseño__
1. Pulse el botón __Política__ en la barra emergente

   ![Políticas de cuadrícula interactiva](./assets/spa-container-component/templates-policies-action.png)

1. A la derecha, debajo de __Componentes permitidos__ pestaña, expandir __APLICACIÓN WKND: CONTENIDO__
1. Asegúrese de que solo están seleccionadas las siguientes opciones:
   + Imagen
   + Texto
   + Título

   ![SPA Página de remota](./assets/spa-container-component/templates-allowed-components.png)

1. Pulse __Listo__

## AEM Creación del contenedor en la

SPA Después de la actualización de la para incrustar `<ResponsiveGrid...>`, contenedores para tres componentes React editables (`EditableTitle`, `EditableText`, y `EditableImage`AEM ), y si se actualiza con una política de plantilla correspondiente, podemos empezar a crear contenido en el componente contenedor.

1. Inicie sesión en AEM Author
1. Vaya a __Sitios > Aplicación WKND__
1. Tocar __Inicio__ y seleccione __Editar__ desde la barra de acciones superior
   + AEM Se muestra un componente de texto &quot;Hello World&quot;, ya que este se añadió automáticamente al generar el proyecto a partir del arquetipo de proyecto de la lista de tipos de archivo
1. Seleccionar __Editar__ en el selector de modo situado en la parte superior derecha del Editor de páginas
1. Busque el __Contenedor de diseño__ área editable debajo del título
1. Abra el __Barra lateral del editor de páginas__ y seleccione la opción __Vista de componentes__
1. Arrastre los siguientes componentes al __Contenedor de diseño__
   + Imagen
   + Título
1. Arrastre los componentes para reordenarlos al siguiente orden:
   1. Título
   1. Imagen
   1. Texto
1. __Autor__ el __Título__ componente
   1. Pulse el componente Título y pulse el botón __llave__ icono para __editar__ el componente Título
   1. Añada el siguiente texto:
      + Título: __¡El verano se acerca, vamos a aprovecharlo al máximo!__
      + Tipo: __H1__
   1. Pulse __Listo__
1. __Autor__ el __Imagen__ componente
   1. Arrastre una imagen desde la barra lateral (después de cambiar a la vista Recursos) en el componente Imagen
   1. Pulse el componente Imagen y pulse el botón __llave__ icono para editar
   1. Compruebe la __La imagen es decorativa__ casilla de verificación
   1. Pulse __Listo__
1. __Autor__ el __Texto__ componente
   1. Edite el componente Texto al pulsar el componente Texto y luego el botón __llave__ icono
   1. Añada el siguiente texto:
      + _Ahora mismo, puedes obtener un 15% en todas las aventuras de 1 semana, y un 20% de descuento en todas las aventuras que son de 2 semanas o más! Al finalizar la compra, añada el código de campaña SUMMERISCOMING para obtener sus descuentos._
   1. Pulse __Listo__

1. Los componentes ya se han creado, pero se apilan verticalmente.

   ![Componentes creados](./assets/spa-container-component/authored-components.png)

AEM Utilice el modo Diseño de la para permitirnos ajustar el tamaño y el diseño de los componentes.

1. Cambiar a __Modo de diseño__ uso del selector de modo en la parte superior derecha
1. __Redimensionar__ los componentes Imagen y Texto, de modo que estén uno al lado del otro
   + __Imagen__ el componente debe ser __8 columnas de ancho__
   + __Texto__ el componente debe ser __3 columnas de ancho__

   ![Componentes de diseño](./assets/spa-container-component/layout-components.png)

1. __Previsualizar__ AEM Sus cambios en el Editor de páginas de la
1. Actualice la aplicación WKND ejecutándose localmente en [http://localhost:3000](http://localhost:3000) para ver los cambios realizados.

   ![SPA Componente Contenedor en la](./assets/spa-container-component/localhost-final.png)


## Enhorabuena.

Ha agregado un componente contenedor que permite a los autores agregar componentes editables a la aplicación WKND. Ahora ya sabe cómo:

+ AEM Usar el componente editable de React de la `ResponsiveGrid` SPA componente en el recurso de la
+ SPA Cree y registre componentes React editables (texto e imagen) para usarlos en la lista de componentes a través del componente de contenedor de
+ SPA SPA Configure la plantilla Página de administración remota de para permitir los componentes habilitados para la creación de informes
+ Agregar componentes editables al componente contenedor
+ SPA Componentes de autor y diseño en el editor de

## Pasos siguientes

El siguiente paso utiliza esta misma técnica para [añadir un componente editable a una ruta de detalles de aventura](./spa-dynamic-routes.md) SPA en el menú de la.

---
title: Agregar componentes de contenedor editables a un SPA remoto
description: Aprenda a añadir componentes de contenedor editables a un SPA remoto que permita a los autores de AEM arrastrar y soltar componentes en ellos.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 2%

---

# Componentes de contenedor editables

[Componentes fijos](./spa-fixed-component.md) proporciona cierta flexibilidad para la creación SPA contenido, aunque este enfoque es rígido y requiere que los desarrolladores definan la composición exacta del contenido editable. Para que los autores puedan crear experiencias excepcionales, SPA Editor admite el uso de componentes de contenedor en el SPA. Los componentes de contenedor permiten a los autores arrastrar y soltar los componentes permitidos en el contenedor y crearlos, como pueden en la creación tradicional de AEM Sites.

![Componentes de contenedor editables](./assets/spa-container-component/intro.png)

En este capítulo, se añade un contenedor editable a la vista de inicio para que los autores puedan componer y diseñar experiencias de contenido enriquecido utilizando AEM componentes principales React directamente en el SPA.

## Actualizar la aplicación WKND

Para añadir un componente contenedor a la vista Inicio:

+ Importar el componente ResponsiveGrid del componente React Editable de AEM
+ Importar y registrar AEM componentes principales de React (texto e imagen) para utilizarlos en el componente contenedor

### Importar en el componente contenedor de cuadrícula interactiva

Para colocar un área editable en la vista Inicio, debemos:

1. Importar el componente ResponsiveGrid desde `@adobe/aem-react-editable-components`
1. Regístrelo usando `withMappable` para que los desarrolladores puedan colocarlo en el SPA
1. Además, regístrese con `MapTo` de modo que se pueda reutilizar en otros componentes de contenedor, anidando efectivamente contenedores.

Para ello:

1. Abra el proyecto SPA en su IDE
1. Cree un componente React en `src/components/aem/AEMResponsiveGrid.js`
1. Agregue el siguiente código a `AEMResponsiveGrid.js`

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

El código es similar `AEMTitle.js` that [se ha importado el componente Título de los componentes principales de AEM alcance](./spa-fixed-component.md).


La variable `AEMResponsiveGrid.js` debe tener el siguiente aspecto:

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### Uso del componente SPA AEMResponsiveGrid

Ahora que AEM componente ResponsiveGrid está registrado en y disponible para su uso dentro de la SPA, podemos colocarlo en la vista Inicio.

1. Abra y edite `react-app/src/Home.js`
1. Importe el `AEMResponsiveGrid` y colóquelo encima de `<AEMTitle ...>` componente.
1. Establezca los atributos siguientes en la variable `<AEMResponsiveGrid...>` componente
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Esto indica a la `AEMResponsiveGrid` para recuperar su contenido del recurso de AEM:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   La variable `itemPath` se asigna a la variable `responsivegrid` nodo definido en la variable `Remote SPA Page` AEM plantilla y se crea automáticamente en las nuevas páginas AEM creadas a partir de la variable `Remote SPA Page` Plantilla AEM.

   Actualizar `Home.js` para agregar la variable `<AEMResponsiveGrid...>` componente.

   ```
   ...
   import AEMResponsiveGrid from './aem/AEMResponsiveGrid';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <AEMResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
               <Adventures />
           </div>
       );
   }
   ```

La variable `Home.js` debe tener el siguiente aspecto:

![Home.js](./assets/spa-container-component/home-js.png)

## Crear componentes editables

Para obtener el efecto completo de la experiencia de creación flexible, los contenedores se proporcionan en SPA Editor. Ya hemos creado un componente Título editable, pero vamos a hacer algunos más que permitan a los autores utilizar Componentes principales de Texto e Imagen AEM WCM en el componente contenedor recién agregado.

### Componente de texto

1. Abra el proyecto SPA en su IDE
1. Cree un componente React en `src/components/aem/AEMText.js`
1. Agregue el siguiente código a `AEMText.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

La variable `AEMText.js` debe tener el siguiente aspecto:

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### Componente de imagen

1. Abra el proyecto SPA en su IDE
1. Cree un componente React en `src/components/aem/AEMImage.js`
1. Agregue el siguiente código a `AEMImage.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. Creación de un archivo SCSS `src/components/aem/AEMImage.scss` que proporciona estilos personalizados para la variable `AEMImage.scss`. Estos estilos se dirigen a las clases CSS de notación BEM del componente principal React de AEM.
1. Agregue la siguiente SCSS a `AEMImage.scss`

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importar `AEMImage.scss` en `AEMImage.js`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

La variable `AEMImage.js` y `AEMImage.scss` debería tener el siguiente aspecto:

![AEMImage.js y AEMImage.scs](./assets/spa-container-component/aem-image-js-scss.png)

### Importación de componentes editables

El recién creado `AEMText` y `AEMImage` SPA componentes son referenciados en la SPA y se crean instancias de ellos de forma dinámica en función del JSON devuelto por AEM. Para asegurarse de que estos componentes están disponibles para el SPA, cree instrucciones de importación para ellos en `Home.js`

1. Abra el proyecto SPA en su IDE
1. Abra el archivo `src/Home.js`
1. Agregar instrucciones de importación para `AEMText` y `AEMImage`

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


El resultado debería ser como:

![Home.js](./assets/spa-container-component/home-js-imports.png)

Si estas importaciones son _not_ se ha añadido la variable `AEMText` y `AEMImage` SPA no invoca el código y, por lo tanto, los componentes no se registran en los tipos de recurso proporcionados.

## Configuración del contenedor en AEM

AEM componentes de contenedor utilizan políticas para dictar sus componentes permitidos. Esta es una configuración crítica cuando se utiliza SPA Editor, ya que solo AEM componentes principales de WCM que tienen asignados SPA componentes equivalentes pueden procesarlos los SPA. Asegúrese de que solo están permitidos los componentes que hemos proporcionado SPA implementaciones de :

+ `AEMTitle` asignado a `wknd-app/components/title`
+ `AEMText` asignado a `wknd-app/components/text`
+ `AEMImage` asignado a `wknd-app/components/image`

Para configurar el contenedor de cuadrícula de respuesta de la plantilla Página de SPA remota:

1. Iniciar sesión en AEM Author
1. Vaya a __Herramientas > General > Plantillas > Aplicación WKND__
1. Editar __Página SPA informe__

   ![Políticas de cuadrícula adaptable](./assets/spa-container-component/templates-remote-spa-page.png)

1. Select __Estructura__ en el conmutador de modo en la parte superior derecha
1. Toque para seleccionar el __Contenedor de diseño__
1. Toque . __Política__ en la barra emergente

   ![Políticas de cuadrícula adaptable](./assets/spa-container-component/templates-policies-action.png)

1. A la derecha, debajo de __Componentes permitidos__ pestaña, expandir __APLICACIÓN WKND: CONTENIDO__
1. Asegúrese de que solo están seleccionadas las siguientes opciones:
   + Imagen
   + Texto
   + Título

   ![Página SPA remota](./assets/spa-container-component/templates-allowed-components.png)

1. Puntee __Listo__

## Creación del contenedor en AEM

Después de actualizar el SPA para incrustar el `<AEMResponsiveGrid...>`, contenedores para tres componentes principales AEM React (`AEMTitle`, `AEMText`y `AEMImage`), y AEM se actualiza con una directiva de plantilla coincidente, podemos empezar a crear contenido en el componente contenedor.

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > Aplicación WKND__
1. Toque __Página principal__ y seleccione __Editar__ desde la barra de acciones superior
   + Aparece un componente Texto &quot;Hola a todos&quot;, ya que se agregó automáticamente al generar el proyecto a partir del tipo de archivo del proyecto AEM
1. Select __Editar__ del selector de modo en la parte superior derecha del Editor de páginas
1. Busque la variable __Contenedor de diseño__ área editable debajo del título
1. Abra el __Barra lateral del Editor de páginas__ y seleccione __Vista Componentes__
1. Arrastre los componentes siguientes al __Contenedor de diseño__
   + Imagen
   + Título
1. Arrastre los componentes para reordenarlos en el siguiente orden:
   1. Título
   1. Imagen
   1. Texto
1. __Autor__ el __Título__ componente
   1. Pulse el componente Título y pulse el botón __llave__ para __editar__ el componente Título
   1. Añada el siguiente texto:
      + Título: __El verano viene, ¡aprovechémoslo al máximo!__
      + Tipo: __H1__
   1. Puntee __Listo__
1. __Autor__ el __Imagen__ componente
   1. Arrastre una imagen desde la barra lateral (después de cambiar a la vista Recursos) del componente Imagen
   1. Pulse el componente Imagen y pulse el botón __llave__ icono para editar
   1. Marque la __La imagen es decorativa__ casilla de verificación
   1. Puntee __Listo__
1. __Autor__ el __Texto__ componente
   1. Edite el componente Texto tocando el componente Texto y tocando el __llave__ icono
   1. Añada el siguiente texto:
      + _En este momento, puede obtener un 15% de todas las aventuras de una semana y un 20% de descuento en todas las aventuras de dos semanas o más. En el cierre de compra, añada el código de campaña SUMERISCOMING para obtener sus descuentos._
   1. Puntee __Listo__

1. Los componentes ahora se crean, pero se apilan verticalmente.

   ![Componentes creados](./assets/spa-container-component/authored-components.png)

   Utilice AEM modo Diseño para que podamos ajustar el tamaño y el diseño de los componentes.

1. Cambie a __Modo de diseño__ uso del selector de modo en la parte superior derecha
1. __Cambiar tamaño__ los componentes Imagen y texto, de forma que estén uno al lado del otro
   + __Imagen__ debe ser __8 columnas anchas__
   + __Texto__ debe ser __3 columnas anchas__

   ![Componentes de diseño](./assets/spa-container-component/layout-components.png)

1. __Vista previa__ los cambios realizados en AEM Editor de páginas
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000) para ver los cambios creados.

   ![Componente de contenedor en SPA](./assets/spa-container-component/localhost-final.png)


## Felicitaciones!

Ha agregado un componente contenedor que permite que los autores añadan componentes editables a la aplicación WKND. Ahora sabe cómo:

+ Utilice el componente AEM React Editable Component&#39;s ResponsiveGrid en el SPA
+ Registre AEM componentes principales de React (texto e imagen) para utilizarlos en la SPA a través del componente contenedor
+ Configure la plantilla Página de SPA remota para permitir los componentes principales habilitados para SPA
+ Añadir componentes editables al componente contenedor
+ Creación y diseño de componentes en SPA Editor

## Siguientes pasos

El siguiente paso será utilizar esta misma técnica para [añadir un componente editable a una ruta de detalles de aventura](./spa-dynamic-routes.md) en el SPA.

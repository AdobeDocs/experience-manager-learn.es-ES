---
title: Añadir componentes editables a las rutas remotas SPA dinámicas
description: Aprenda a añadir componentes editables a rutas dinámicas en un SPA remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 1%

---

# Rutas dinámicas y componentes editables

En este capítulo, habilitamos dos rutas dinámicas de detalle de aventura para admitir componentes editables; __Campo de surf de Bali__ y __Beervana en Portland__.

![Rutas dinámicas y componentes editables](./assets/spa-dynamic-routes/intro.png)

La ruta SPA detalle de aventura se define como `/adventure:path` donde `path` es la ruta a WKND Adventure (fragmento de contenido) para mostrar detalles sobre.

## Asignar las direcciones URL SPA a páginas AEM

En los dos capítulos anteriores, asignamos el contenido de componentes editables de la vista Inicio de SPA a la página raíz de SPA remota correspondiente en AEM en `/content/wknd-app/us/en/`.

La definición de la asignación para componentes editables para las rutas dinámicas de SPA es similar, sin embargo, se debe crear un esquema de asignación 1:1 entre instancias de las páginas de ruta y AEM.

En este tutorial, tomamos el nombre del fragmento de contenido de WKND Adventure, que es el último segmento de la ruta, y lo asignamos a una ruta simple en `/content/wknd-app/us/en/adventure`.

| Ruta de SPA remota | AEM ruta de página |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/es/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/es/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/es/home/adventure/__beervana-in-portland__ |

Por lo tanto, en función de esta asignación, debemos crear dos nuevas páginas de AEM en:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Asignación de SPA remota

La asignación para solicitudes que salen de SPA remotas se configura mediante la variable `setupProxy` configuración realizada en [Bootstrap del SPA](./spa-bootstrap.md).

## Asignación del editor de SPA

La asignación para solicitudes SPA cuando el SPA se abre mediante AEM editor SPA se configura mediante la configuración de asignaciones de Sling realizada en [Configurar AEM](./aem-configure.md).

## Crear páginas de contenido en AEM

Primero, cree el intermediario `adventure` Segmento de página:

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > Aplicación WKND > us > es > Página de inicio de la aplicación WKND__
   + Esta página de AEM se asigna como la raíz de la SPA, por lo que aquí es donde empezamos a crear la estructura de páginas de AEM para otras rutas de SPA.
1. Toque __Crear__ y seleccione __Página__
1. Seleccione el __Página SPA remota__ plantilla y toque __Siguiente__
1. Complete las Propiedades de página
   + __Título__: Aventura
   + __Nombre__: `adventure`
      + Este valor define la dirección URL de la página AEM y, por lo tanto, debe coincidir con el segmento de ruta del SPA.
1. Puntee __Listo__

A continuación, cree las páginas de AEM que correspondan a cada una de las direcciones URL de SPA que requieran áreas editables.

1. Vaya al nuevo __Aventura__ en el
1. Toque __Crear__ y seleccione __Página__
1. Seleccione el __Página SPA remota__ plantilla y toque __Siguiente__
1. Complete las Propiedades de página
   + __Título__: Campo de surf de Bali
   + __Nombre__: `bali-surf-camp`
      + Este valor define la dirección URL de la página de AEM y, por lo tanto, debe coincidir con el último segmento de la ruta de SPA
1. Puntee __Listo__
1. Repita los pasos del 3 al 6 para crear la variable __Beervana en Portland__ con:
   + __Título__: Beervana en Portland
   + __Nombre__: `beervana-in-portland`
      + Este valor define la dirección URL de la página de AEM y, por lo tanto, debe coincidir con el último segmento de la ruta de SPA

Estas dos páginas AEM contienen el contenido creado en cada caso para sus rutas SPA coincidentes. Si otras rutas SPA requieren creación, se deben crear nuevas AEM de páginas en la dirección URL SPA debajo de la página raíz de la página de SPA remota (`/content/wknd-app/us/en/home`) en AEM.

## Actualizar la aplicación WKND

Vamos a colocar el `<AEMResponsiveGrid...>` componente creado en el [último capítulo](./spa-container-component.md), dentro de `AdventureDetail` SPA componente, crear un contenedor editable.

### Colocación del componente SPA AEMResponsiveGrid

Colocación de la variable `<AEMResponsiveGrid...>` en el `AdventureDetail` crea un contenedor editable en esa ruta. El truco es que las rutas múltiples utilizan la variable `AdventureDetail` para procesar, debemos ajustar dinámicamente el  `<AEMResponsiveGrid...>'s pagePath` atributo. La variable `pagePath` debe derivarse para que apunte a la página de AEM correspondiente, según la aventura que muestre la instancia de la ruta.

1. Abra y edite `react-app/src/components/AdventureDetail.js`
1. Añada la siguiente línea antes de `AdventureDetail(..)'s` segundo `return(..)` , que deriva el nombre de la aventura de la ruta del fragmento de contenido.

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = _path.split('/').pop();
   ...
   ```

1. Importe el `AEMResponsiveGrid` y colóquelo encima de `<h2>Itinerary</h2>` componente.
1. Establezca los atributos siguientes en la variable `<AEMResponsiveGrid...>` componente
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Esto indica a la `AEMResponsiveGrid` para recuperar su contenido del recurso de AEM:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Actualizar `AdventureDetail.js` con las siguientes líneas:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = _path.split('/').pop();

    return(
        ...
        <AEMResponsiveGrid 
            pagePath={`/content/wknd-app/us/en/home/adventure/${adventureName}`}
            itemPath="root/responsivegrid"/>
            
        <h2>Itinerary</h2>
        ...
    )
}
```

La variable `AdventureDetail.js` debe tener el siguiente aspecto:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Creación del contenedor en AEM

Con la variable `<AEMResponsiveGrid...>` in situ y `pagePath` se configura de forma dinámica en función de la aventura que se representa, probamos crear contenido en ella.

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > Aplicación WKND > us > es__
1. __Editar__ el __Página principal de la aplicación WKND__ página
   + Vaya a la __Campo de surf de Bali__ enrute en el SPA para editarlo
1. Select __Vista previa__ desde el selector de modo en la parte superior derecha
1. Toque en la __Campo de surf de Bali__ en el SPA para navegar a su ruta
1. Select __Editar__ desde el selector de modo
1. Busque la variable __Contenedor de diseño__ área editable justo encima de __Itinerario__
1. Abra el __Barra lateral del Editor de páginas__ y seleccione __Vista Componentes__
1. Arrastre algunos de los componentes habilitados al __Contenedor de diseño__
   + Imagen
   + Texto
   + Título

   Y crear material promocional de marketing. Podría parecerse a esto:

   ![Creación de detalles de Bali Adventure](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Vista previa__ los cambios realizados en AEM Editor de páginas
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000), vaya a la __Campo de surf de Bali__ para ver los cambios creados.

   ![SPA remoto Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

Al navegar a una ruta de detalles de aventura que no tiene una página AEM asignada, no hay capacidad de creación en esa instancia de ruta. Para habilitar la creación en estas páginas, simplemente realice una Página AEM con el nombre coincidente en la sección __Aventura__ página!

## Felicitaciones!

Felicitaciones! ¡Ha añadido la capacidad de creación a las rutas dinámicas en el SPA!

+ Se ha agregado el componente AEM ReactEditable Component del componente ResponsiveGrid a una ruta dinámica
+ Creación de páginas AEM para apoyar la creación de dos rutas específicas en el SPA (Bali Surf Camp y Beervana en Portland)
+ Contenido creado sobre la dinámica ruta del campamento de surf de Bali.

Ya ha terminado de explorar los primeros pasos de cómo se puede utilizar AEM editor de SPA para agregar áreas editables específicas a un SPA remoto.

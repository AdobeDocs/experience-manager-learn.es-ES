---
title: SPA Agregar componentes editables a rutas dinámicas de remoto
description: SPA Aprenda a agregar componentes editables a rutas dinámicas en un entorno remoto
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7636
thumbnail: kt-7636.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 4accc1ca-6f4b-449e-bf2e-06f19d2fe17d
duration: 260
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Rutas dinámicas y componentes editables

En este capítulo, habilitamos dos rutas dinámicas de Detalle de aventura para admitir componentes editables; __Bali Surf Camp__ y __Beervana en Portland__.

![Rutas dinámicas y componentes editables](./assets/spa-dynamic-routes/intro.png)

SPA La ruta del Detalle de la Aventura se define como `/adventure/:slug` donde `slug` es una propiedad de identificador único en el fragmento de contenido de aventura.

## SPA AEM Asignación de las URL de la a páginas de la

SPA SPA AEM En los dos capítulos anteriores, asignamos contenido de componente editable de la vista Inicio de la página de la página de inicio de la página de la página de inicio de la página de la página de la página de inicio de la página de la página de la página de la página de la página de la página de inicio de la página de remota en la página de la `/content/wknd-app/us/en/`.

SPA AEM La definición de la asignación para componentes editables para las rutas dinámicas de la es similar, pero debemos crear un esquema de asignación 1:1 entre instancias de la ruta y páginas de la.

En este tutorial, tomamos el nombre del fragmento de contenido de WKND Adventure, que es el último segmento de la ruta, y lo asignamos a una ruta simple en `/content/wknd-app/us/en/adventure`.

| SPA Ruta remota | AEM Ruta de página |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__bervana-portland__ | /content/wknd-app/us/en/home/adventure/__bervana-in-portland__ |

AEM Por lo tanto, en función de esta asignación, debemos crear dos páginas nuevas de la en:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## SPA Asignación remota

SPA La asignación para solicitudes que salen del sitio remoto se configura mediante la opción de asignación de la variable `setupProxy` configuración realizada en [Bootstrap SPA de la](./spa-bootstrap.md).

## SPA Asignación de editor de

SPA SPA AEM SPA Las asignaciones para solicitudes de cuando el recurso se abre mediante el Editor de se configuran mediante la Configuración de asignaciones de Sling, que se realiza en [AEM Configurar la](./aem-configure.md).

## AEM Creación de páginas de contenido en la

Primero, cree el intermediario `adventure` Segmento de página:

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sites > Aplicación WKND > us > es > Página de inicio de la aplicación WKND__
   + AEM SPA AEM SPA Esta página se asigna como la raíz de la, por lo que aquí es donde empezamos a crear la estructura de la página de la página para otras rutas de.
1. Tocar __Crear__ y seleccione __Página__
1. Seleccione el __SPA Página de remota__ plantilla y pulse __Siguiente__
1. Rellene las propiedades de la página
   + __Título__: Aventura
   + __Nombre__: `adventure`
      + AEM SPA Este valor define la dirección URL de la página de la y, por lo tanto, debe coincidir con el segmento de ruta de la página de la.
1. Tocar __Listo__

AEM SPA A continuación, cree las páginas de que correspondan a cada una de las direcciones URL que requieran áreas editables.

1. Vaya al nuevo __Aventura__ página en el Administrador del sitio
1. Tocar __Crear__ y seleccione __Página__
1. Seleccione el __SPA Página de remota__ plantilla y pulse __Siguiente__
1. Rellene las propiedades de la página
   + __Título__: Campamento de Surf en Bali
   + __Nombre__: `bali-surf-camp`
      + AEM SPA Este valor define la dirección URL de la página de la y, por lo tanto, debe coincidir con el último segmento de la ruta de la página de la ruta de la
1. Tocar __Listo__
1. Repita los pasos del 3 al 6 para crear el __Beervana en Portland__ página, con:
   + __Título__: Beervana en Portland
   + __Nombre__: `beervana-in-portland`
      + AEM SPA Este valor define la dirección URL de la página de la y, por lo tanto, debe coincidir con el último segmento de la ruta de la página de la ruta de la

AEM SPA Estas dos páginas de contienen el contenido creado por ellos para sus rutas de acceso a la coincidentes. SPA AEM SPA SPA Si es necesario crear otras rutas de, se deben crear nuevas Páginas de en su dirección URL de la página raíz de la página de la página de la página remota de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página de la página remota (`/content/wknd-app/us/en/home`AEM ) en el caso de los.

## Actualización de la aplicación WKND

Vamos a colocar el `<ResponsiveGrid...>` componente creado en [último capítulo](./spa-container-component.md), en nuestro `AdventureDetail` SPA componente, creación de un contenedor editable.

### SPA Colocar el componente ResponsiveGrid

Colocación de la `<ResponsiveGrid...>` en el `AdventureDetail` crea un contenedor editable en esa ruta. El truco se debe a que varias rutas utilizan el `AdventureDetail` componente para procesar, debemos ajustar dinámicamente la variable  `<ResponsiveGrid...>'s pagePath` atributo. El `pagePath` AEM debe derivarse para que apunte a la página de la ruta correspondiente, en función de la aventura que muestre la instancia de la ruta.

1. Abrir y editar `react-app-/src/components/AdventureDetail.js`
1. Importe el `ResponsiveGrid` y colóquelo encima del componente `<h2>Itinerary</h2>` componente.
1. Establezca los siguientes atributos en la variable `<ResponsiveGrid...>` componente. Tenga en cuenta `pagePath` el atributo añade el `slug` que se asigna a la página de aventura según la asignación definida anteriormente.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   Esto indica a la `ResponsiveGrid` AEM para recuperar su contenido del recurso de la:

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

Actualizar `AdventureDetail.js` con las líneas siguientes:

```javascript
...
import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
...

function AdventureDetailRender(props) {
    ...
    // Get the slug from the React route parameter, this will be used to specify the AEM Page to store/read editable content from
    const { slug } = useParams();

    return(
        ...
        // Pass the slug in
        function AdventureDetailRender({ title, primaryImage, activity, adventureType, tripLength, 
                groupSize, difficulty, price, description, itinerary, references, slug }) {
            ...
            return (
                ...
                <ResponsiveGrid 
                    pagePath={`/content/wknd-app/us/en/home/adventure/${slug}`}
                    itemPath="root/responsivegrid"/>
                    
                <h2>Itinerary</h2>
                ...
            )
        }
    )
}
```

El `AdventureDetail.js` el archivo debe tener un aspecto similar al siguiente:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEM Crear el contenedor en el espacio de trabajo de

Con el `<ResponsiveGrid...>` en su lugar, y su `pagePath` dinámicamente establecido en función de la aventura que se está representando, intentamos crear contenido en él.

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sites > Aplicación WKND > us > es__
1. __Editar__ el __Página de inicio de la aplicación WKND__ página
   + Vaya a __Bali Surf Camp__ SPA ruta en la lista de rutas para editarla
1. Seleccionar __Previsualizar__ desde el selector de modo en la parte superior derecha
1. Pulse en __Bali Surf Camp__ SPA en la tarjeta de de para ir a la ruta que le corresponde.
1. Seleccionar __Editar__ desde el selector de modo
1. Busque el __Contenedor de diseño__ área editable justo encima de __Itinerario__
1. Abra el __Barra lateral del editor de páginas__ y seleccione la opción __Vista de componentes__
1. Arrastre algunos de los componentes habilitados al __Contenedor de diseño__
   + Imagen
   + Texto
   + Título

   Y crear material de marketing promocional. Podría tener un aspecto similar al siguiente:

   ![Creación de detalles de aventura de Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. __Previsualizar__ AEM Sus cambios en el Editor de páginas de la
1. Actualice la aplicación WKND ejecutándose localmente en [http://localhost:3000](http://localhost:3000), vaya al __Bali Surf Camp__ para ver los cambios creados.

   ![SPA Remoto Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

AEM Al navegar a una ruta de detalles de aventura que no tiene una página de ruta asignada, no hay capacidad de creación en esa instancia de ruta. AEM Para habilitar la creación de en estas páginas, simplemente cree una Página de con el nombre correspondiente en la __Aventura__ página!

## Enhorabuena.

Felicitaciones. SPA ¡Ha agregado la capacidad de creación a las rutas dinámicas en la!

+ AEM Se ha agregado el componente ResponsiveGrid del componente Editable React de la a una ruta dinámica
+ AEM SPA Creación de páginas de apoyo a la creación de dos rutas específicas en el campo de surf de la (Bali y Beervana en Portland)
+ Contenido creado en la dinámica ruta Bali Surf Camp!

AEM SPA SPA ¡Ya ha completado la exploración de los primeros pasos de cómo se puede utilizar el Editor de para agregar áreas editables específicas a un control de acceso remoto

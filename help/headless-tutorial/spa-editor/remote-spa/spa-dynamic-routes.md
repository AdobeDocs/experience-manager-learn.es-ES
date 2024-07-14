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
duration: 202
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Rutas dinámicas y componentes editables

En este capítulo, habilitamos dos rutas dinámicas de Adventure Detail para admitir componentes editables: __Bali Surf Camp__ y __Beervana en Portland__.

![Rutas dinámicas y componentes editables](./assets/spa-dynamic-routes/intro.png)

SPA La ruta del Detalle de la aventura se define como `/adventure/:slug`, donde `slug` es una propiedad de identificador único en el fragmento de contenido de aventura.

## SPA AEM Asignación de las URL de la a páginas de la

SPA SPA AEM En los dos capítulos anteriores, asignamos contenido de componente editable de la vista Inicio de la página de inicio de la página de la página de inicio de la página de la página de inicio de la remota correspondiente en la página de inicio de la página de la página de la página de la página de inicio de la página de la página de la página de la página de la remota en la página de la página `/content/wknd-app/us/en/`.

SPA AEM La definición de la asignación para componentes editables para las rutas dinámicas de la es similar, pero debemos crear un esquema de asignación 1:1 entre instancias de la ruta y páginas de la.

En este tutorial, tomamos el nombre del fragmento de contenido de WKND Adventure, que es el último segmento de la ruta, y lo asignamos a una ruta de acceso simple bajo `/content/wknd-app/us/en/adventure`.

| SPA Ruta remota | AEM Ruta de página |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/en/home |
| /adventure/__bali-surf-camp__ | /content/wknd-app/us/en/home/adventure/__bali-surf-camp__ |
| /adventure/__bervana-portland__ | /content/wknd-app/us/en/home/adventure/__bervana-in-portland__ |

AEM Por lo tanto, en función de esta asignación, debemos crear dos páginas nuevas de la en:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## SPA Asignación remota

La asignación de las solicitudes que salen del sitio remoto se ha configurado a través de la configuración `setupProxy` realizada en el Bootstrap SPA [el SPA de la base de datos ](./spa-bootstrap.md).

## SPA Asignación de editor de

SPA SPA AEM SPA AEM La asignación para las solicitudes de cuando se abre el a través del Editor de se configura mediante la configuración Asignaciones de Sling realizada en [Configurar el](./aem-configure.md).

## AEM Creación de páginas de contenido en la

Primero, cree el segmento de página `adventure` intermedio:

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sitios > Aplicación WKND > us > es > Página de inicio de la aplicación WKND__
   + AEM SPA AEM SPA Esta página se asigna como la raíz de la, por lo que aquí es donde empezamos a crear la estructura de la página de la página para otras rutas de.
1. Pulse __Crear__ y seleccione __Página__
1. SPA Seleccione la plantilla __Página de acceso remoto__ y pulse __Siguiente__
1. Rellene las propiedades de la página
   + __Título__: Aventura
   + __Nombre__: `adventure`
      + AEM SPA Este valor define la dirección URL de la página de la y, por lo tanto, debe coincidir con el segmento de ruta de la página de la.
1. Pulse __Listo__

AEM SPA A continuación, cree las páginas de que correspondan a cada una de las direcciones URL que requieran áreas editables.

1. Vaya a la nueva página __Aventura__ en el Administrador del sitio
1. Pulse __Crear__ y seleccione __Página__
1. SPA Seleccione la plantilla __Página de acceso remoto__ y pulse __Siguiente__
1. Rellene las propiedades de la página
   + __Título__: Campamento de Surf en Bali
   + __Nombre__: `bali-surf-camp`
      + AEM SPA Este valor define la dirección URL de la página de la y, por lo tanto, debe coincidir con el último segmento de la ruta de la página de la ruta de la
1. Pulse __Listo__
1. Repita los pasos del 3 al 6 para crear la página __Beervana in Portland__ con:
   + __Título__: Beervana en Portland
   + __Nombre__: `beervana-in-portland`
      + AEM SPA Este valor define la dirección URL de la página de la y, por lo tanto, debe coincidir con el último segmento de la ruta de la página de la ruta de la

AEM SPA Estas dos páginas de contienen el contenido creado por ellos para sus rutas de acceso a la coincidentes. SPA AEM SPA SPA AEM Si otras rutas de requieren creación, se deben crear nuevas Páginas de en su dirección URL de la página raíz de la página de la página remota de la página de la página de la página de la página remota (`/content/wknd-app/us/en/home`) en la dirección URL de la.

## Actualización de la aplicación WKND

SPA Coloquemos el componente `<ResponsiveGrid...>` creado en el [último capítulo](./spa-container-component.md), en nuestro componente `AdventureDetail`, para crear un contenedor editable.

### SPA Colocar el componente ResponsiveGrid

Al colocar `<ResponsiveGrid...>` en el componente `AdventureDetail`, se crea un contenedor editable en esa ruta. El truco se debe a que varias rutas utilizan el componente `AdventureDetail` para representarse, debemos ajustar dinámicamente el atributo `<ResponsiveGrid...>'s pagePath`. AEM Se debe derivar `pagePath` para que apunte a la página de correspondiente, según la aventura que muestre la instancia de la ruta.

1. Abrir y editar `react-app-/src/components/AdventureDetail.js`
1. Importe el componente `ResponsiveGrid` y colóquelo sobre el componente `<h2>Itinerary</h2>`.
1. Establezca los atributos siguientes en el componente `<ResponsiveGrid...>`. Tenga en cuenta que el atributo `pagePath` agrega el elemento `slug` actual que se asigna a la página de aventura según la asignación definida anteriormente.
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${slug}'`
   + `itemPath = 'root/responsivegrid'`

   AEM Esto indica al componente `ResponsiveGrid` que recupere su contenido del recurso de la:

   + `/content/wknd-app/us/en/home/adventure/${slug}/jcr:content/root/responsivegrid`

Actualice `AdventureDetail.js` con las líneas siguientes:

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

El archivo `AdventureDetail.js` debe tener el siguiente aspecto:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## AEM Crear el contenedor en el espacio de trabajo de

Con `<ResponsiveGrid...>` en su lugar y su `pagePath` establecido dinámicamente en función de la aventura que se está representando, intentamos crear contenido en él.

1. AEM Iniciar sesión en el autor de la
1. Vaya a __Sitios > Aplicación WKND > us > en__
1. __Editar__ la página de inicio de la aplicación __WKND__
   + SPA Navegue hasta la ruta __Bali Surf Camp__ en la ruta para editarla
1. Seleccione __Vista previa__ en el selector de modo en la parte superior derecha
1. SPA Toca la tarjeta __Bali Surf Camp__ en la pantalla para navegar a la ruta que te ofrece el juego de la ruta.
1. Seleccione __Editar__ del selector de modo
1. Busque el área editable __Contenedor de diseño__ justo encima del __Itinerario__
1. Abra la __barra lateral del editor de páginas__ y seleccione la __vista Componentes__
1. Arrastre algunos de los componentes habilitados al __contenedor de diseño__
   + Imagen
   + Texto
   + Título

   Y crear material de marketing promocional. Podría tener un aspecto similar al siguiente:

   ![Creación de detalles de aventura en Bali](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. AEM __Vista previa__ de sus cambios en el Editor de páginas de la página de la
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000), navegue hasta la ruta __Bali Surf Camp__ para ver los cambios creados.

   SPA ![Acceso remoto a Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

AEM Al navegar a una ruta de detalles de aventura que no tiene una página de ruta asignada, no hay capacidad de creación en esa instancia de ruta. AEM Para habilitar la creación de contenido en estas páginas, simplemente cree una página de la página con el nombre que coincida en la página de __Aventura__.

## Enhorabuena.

Enhorabuena. SPA ¡Ha agregado la capacidad de creación a las rutas dinámicas en la!

+ AEM Se ha agregado el componente ResponsiveGrid del componente Editable React de la a una ruta dinámica
+ AEM SPA Creación de páginas de apoyo a la creación de dos rutas específicas en el campo de surf de la (Bali y Beervana en Portland)
+ Contenido creado en la dinámica ruta Bali Surf Camp!

AEM SPA SPA ¡Ya ha completado la exploración de los primeros pasos de cómo se puede utilizar el Editor de para agregar áreas editables específicas a un control de acceso remoto

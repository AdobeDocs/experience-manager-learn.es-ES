---
title: Añadir componentes editables a las rutas remotas SPA dinámicas
description: Aprenda a añadir componentes editables a rutas dinámicas en un SPA remoto.
topic: Sin cabeza, SPA, desarrollo
feature: Editor de SPA, componentes principales, API, desarrollo
role: Developer, Architect
level: Beginner
kt: 7636
thumbnail: kt-7636.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '958'
ht-degree: 1%

---


# Rutas dinámicas y componentes editables

En este capítulo, habilitamos dos rutas dinámicas de detalle de aventura para admitir componentes editables; __Campamento de Surf de Bali__ y __Beervana en Portland__.

![Rutas dinámicas y componentes editables](./assets/spa-dynamic-routes/intro.png)

La ruta de SPA de detalle de aventura se define como `/adventure:path` donde `path` es la ruta a WKND Adventure (fragmento de contenido) para mostrar detalles sobre.

## Asignar las direcciones URL SPA a páginas AEM

En los dos capítulos anteriores, asignamos contenido de componente editable desde la vista Inicio de SPA a la página raíz de SPA remota correspondiente en AEM en `/content/wknd-app/us/en/`.

La definición de la asignación para componentes editables para las rutas dinámicas de SPA es similar, sin embargo, se debe crear un esquema de asignación 1:1 entre instancias de las páginas de ruta y AEM.

En este tutorial, tomaremos el nombre del fragmento de contenido de WKND Adventure, que es el último segmento de la ruta, y lo asignaremos a una ruta simple en `/content/wknd-app/us/en/adventure`.

| Ruta de SPA remota | AEM ruta de página |
|------------------------------------|--------------------------------------------|
| / | /content/wknd-app/us/es/home |
| /adventure:/content/dam/wknd/en/adventures/bali-surf-camp/__bali-surf-camp__ | /content/wknd-app/es/home/adventure/__bali-surf-camp__ |
| /adventure:/content/dam/wknd/en/adventures/beervana-portland/__beervana-portland__ | /content/wknd-app/us/es/home/adventure/__beervana-in-portland__ |

Por lo tanto, en función de esta asignación, debemos crear dos nuevas páginas de AEM en:

+ `/content/wknd-app/us/en/home/adventure/bali-surf-camp`
+ `/content/wknd-app/us/en/home/adventure/beervana-in-portland`

## Asignación de SPA remota

La asignación para las solicitudes que salen del SPA remoto se configura mediante la configuración `setupProxy` realizada en el Bootstrap [SPA](./spa-bootstrap.md).

## Asignación del editor de SPA

La asignación para solicitudes de SPA cuando la SPA se abre mediante AEM Editor SPA se configura mediante la configuración de Asignaciones de Sling realizada en [Configurar AEM](./aem-configure.md).

## Crear páginas de contenido en AEM

Primero, cree el segmento de página `adventure` intermedio:

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > WKND App > us > es > Página principal de la aplicación WKND__
   + Esta página de AEM se asigna como la raíz de la SPA, por lo que aquí es donde empezamos a crear la estructura de páginas de AEM para otras rutas de SPA.
1. Toque __Crear__ y seleccione __Página__
1. Seleccione la plantilla __Página de SPA remota__ y pulse __Siguiente__
1. Complete las Propiedades de página
   + __Título__: Aventura
   + __Nombre__: `adventure`
      + Este valor define la dirección URL de la página AEM y, por lo tanto, debe coincidir con el segmento de ruta del SPA.
1. Puntee __Listo__

A continuación, cree las páginas de AEM que correspondan a cada una de las direcciones URL de SPA que requieran áreas editables.

1. Vaya a la nueva página __Aventura__ en el administrador del sitio
1. Toque __Crear__ y seleccione __Página__
1. Seleccione la plantilla __Página de SPA remota__ y pulse __Siguiente__
1. Complete las Propiedades de página
   + __Título__: Campo de surf de Bali
   + __Nombre__: `bali-surf-camp`
      + Este valor define la dirección URL de la página de AEM y, por lo tanto, debe coincidir con el último segmento de la ruta de SPA
1. Puntee __Listo__
1. Repita los pasos 3-6 para crear la página __Beervana in Portland__ con:
   + __Título__: Beervana en Portland
   + __Nombre__: `beervana-in-portland`
      + Este valor define la dirección URL de la página de AEM y, por lo tanto, debe coincidir con el último segmento de la ruta de SPA

Estas dos páginas AEM contienen el contenido creado correspondiente para sus rutas SPA coincidentes. Si otras rutas de SPA requieren creación, se deben crear nuevas páginas de AEM en la dirección URL de su SPA bajo la página raíz de la página de SPA remota (`/content/wknd-app/us/en/home`) en AEM.

## Actualizar la aplicación WKND

Vamos a colocar el componente `<AEMResponsiveGrid...>` creado en el [último capítulo](./spa-container-component.md) en nuestro componente de SPA `AdventureDetail`, creando un contenedor editable.

### Colocación del componente SPA AEMResponsiveGrid

Colocar el `<AEMResponsiveGrid...>` en el componente `AdventureDetail` crea un contenedor editable en esa ruta. El truco se debe a que varias rutas utilizan el componente `AdventureDetail` para renderizar, debemos ajustar dinámicamente el atributo `<AEMResponsiveGrid...>'s pagePath`. El `pagePath` debe derivarse para que apunte a la página de AEM correspondiente, en función de la aventura que muestra la instancia de la ruta.

1. Abra y edite `react-app/src/components/AdventureDetail.js`
1. Añada la siguiente línea antes de la segunda instrucción `AdventureDetail(..)'s`, que deriva el nombre de la aventura de la ruta del fragmento de contenido.`return(..)`

   ```
   ...
   // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
   const adventureName = adventureData._path.split('/').pop();
   ...
   ```

1. Importe el componente `AEMResponsiveGrid` y colóquelo encima del componente `<h2>Itinerary</h2>`.
1. Establezca los siguientes atributos en el componente `<AEMResponsiveGrid...>`
   + `pagePath = '/content/wknd-app/us/en/home/adventure/${adventureName}'`
   + `itemPath = 'root/responsivegrid'`

   Esto indica al componente `AEMResponsiveGrid` que recupere su contenido del recurso de AEM:

   + `/content/wknd-app/us/en/home/adventure/${adventureName}/jcr:content/root/responsivegrid`


Actualice `AdventureDetail.js` con las siguientes líneas:

```
...
import AEMResponsiveGrid from '../components/aem/AEMResponsiveGrid';
...

function AdventureDetail(props) {
    ...
    // Get the last segment of the Adventure Content Fragment path to used to generate the pagePath for the AEMResponsiveGrid
    const adventureName = adventureData._path.split('/').pop();

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

El archivo `AdventureDetail.js` debería tener el siguiente aspecto:

![AdventureDetail.js](./assets/spa-dynamic-routes/adventure-detail-js.png)

## Creación del contenedor en AEM

Con el `<AEMResponsiveGrid...>` en su lugar y su `pagePath` configurado dinámicamente en función de la aventura que se representa, intentamos crear contenido en él.

1. Iniciar sesión en AEM Author
1. Vaya a __Sites > WKND App > us > en__
1. ____ Edición de la página principal de la aplicación  __WKND__ 
   + Vaya a la ruta __Bali Surf Camp__ en la SPA para editarla
1. Seleccione __Preview__ del selector de modo en la parte superior derecha
1. Toque en la tarjeta __Bali Surf Camp__ de la SPA para navegar a su ruta
1. Seleccione __Editar__ en el selector de modo
1. Busque el __Contenedor de diseño__ área editable justo encima del __Itinerario__
1. Abra la __barra lateral del Editor de páginas__ y seleccione la __Vista Componentes__
1. Arrastre algunos de los componentes habilitados al __Contenedor de diseño__
   + Imagen
   + Texto
   + Título

   Y crear material promocional de marketing. Podría parecerse a esto:

   ![Creación de detalles de Bali Adventure](./assets/spa-dynamic-routes/adventure-detail-edit.png)

1. ____ Previsualizar los cambios en AEM Editor de páginas
1. Actualice la aplicación WKND que se ejecuta localmente en [http://localhost:3000](http://localhost:3000), vaya a la ruta __Bali Surf Camp__ para ver los cambios creados.

   ![SPA remoto Bali](./assets/spa-dynamic-routes/remote-spa-final.png)

Al navegar a una ruta de detalles de aventura que no tenga una página AEM asignada, no habrá capacidad de creación en esa instancia de ruta. Para habilitar la creación en estas páginas, simplemente haga una Página AEM con el nombre coincidente en la página __Aventura__.

## Felicitaciones!

Felicitaciones! ¡Ha añadido la capacidad de creación a las rutas dinámicas en el SPA!

+ Se ha agregado el componente AEM ReactEditable Component del componente ResponsiveGrid a una ruta dinámica
+ Creación de páginas AEM para apoyar la creación de dos rutas específicas en el SPA (Bali Surf Camp y Beervana en Portland)
+ Contenido creado sobre la dinámica ruta del campamento de surf de Bali.

Ya ha terminado de explorar los primeros pasos de cómo se puede utilizar AEM editor de SPA para agregar áreas editables específicas a un SPA remoto.


>[!NOTE]
>
>¡Manténgase atento! Este tutorial se ampliará para abarcar las prácticas recomendadas y recomendaciones de Adobe sobre cómo implementar la solución Editor de SPA para AEM como Cloud Service y los entornos de producción.
---
title: Vista previa del fragmento de contenido
description: Aprenda a utilizar la vista previa de fragmentos de contenido para todos los autores a fin de ver rápidamente cómo afectan los cambios de contenido a sus experiencias sin encabezado AEM.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: ea7cd118d9cba97d2b497f6659f74d2fe8331c66
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# Vista previa del fragmento de contenido

AEM aplicaciones sin encabezado admiten la previsualización de creación integrada. La experiencia de vista previa vincula el editor de fragmentos de contenido del autor de AEM con su aplicación personalizada (accesible mediante HTTP), lo que permite un vínculo profundo a la aplicación que procesa la vista previa del fragmento de contenido.

>[!VIDEO](https://video.tv.adobe.com/v/3416906/?quality=12&learn=on)

Para utilizar la vista previa del fragmento de contenido, se deben cumplir varias condiciones:

1. La aplicación debe implementarse en una URL accesible a los autores
1. La aplicación debe estar configurada para conectarse al servicio AEM Author (en lugar del servicio AEM Publish)
1. La aplicación debe estar diseñada con direcciones URL o rutas que puedan usar [Ruta o ID del fragmento de contenido](#url-expressions) para seleccionar los fragmentos de contenido que se van a mostrar y previsualizar en la experiencia de la aplicación.

## Vista previa de direcciones URL

Vista previa de direcciones URL, con [Expresiones de URL](#url-expressions), se establecen en las propiedades del modelo de fragmento de contenido.

![Dirección URL de vista previa del modelo de fragmento de contenido](./assets/preview/cf-model-preview-url.png)

1. Inicie sesión en el servicio de AEM Author como administrador
1. Vaya a __Herramientas > General > Modelos de fragmento de contenido__
1. Seleccione el __Modelo de fragmento de contenido__ y seleccione __Propiedades__ en la barra de acciones superior.
1. Introduzca la dirección URL de vista previa para el modelo de fragmento de contenido mediante [Expresiones de URL](#url-expressions)
   + La dirección URL de vista previa debe señalar a una implementación de la aplicación que se conecte al servicio de AEM Author.

### Expresiones de URL

Cada modelo de fragmento de contenido puede tener una URL de vista previa configurada. La dirección URL de vista previa se puede parametrizar por fragmento de contenido mediante las expresiones de URL que se enumeran en la tabla siguiente. Se pueden usar varias expresiones de URL en una sola dirección URL de vista previa.

|  | Expresión URL | Valor |
| --------------------------------------- | ----------------------------------- | ----------- |
| Ruta del fragmento de contenido | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| ID del fragmento de contenido | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variación del fragmento de contenido | `${contentFragment.variation}` | `main` |
| Ruta del modelo de fragmento de contenido | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Nombre del modelo de fragmento de contenido | `${contentFragment.model.name}` | `adventure` |

Direcciones URL de vista previa de ejemplo:

+ Una dirección URL de vista previa en el __Aventura__ el modelo podría parecerse a `https://preview.app.wknd.site/adventure${contentFragment.path}` que se resuelve en `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ Una dirección URL de vista previa en el __Artículo__ el modelo podría parecerse a `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` resuelve `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Vista previa en la aplicación

Cualquier fragmento de contenido que utilice el modelo de fragmento de contenido configurado tiene un botón de vista previa. El botón Vista previa abre la dirección URL de vista previa del modelo de fragmento de contenido e introduce los valores del fragmento de contenido abierto en el [Expresiones de URL](#url-expressions).

![Botón Vista previa](./assets/preview/preview-button.png)

Realice una actualización dura (borrando la caché local del explorador) al obtener una vista previa de los cambios del fragmento de contenido en la aplicación.

## Ejemplo de reacción

Vamos a explorar la aplicación WKND, una sencilla aplicación React que muestra las aventuras de AEM mediante API de GraphQL sin encabezado.

El código de ejemplo está disponible en [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-app).

## Direcciones URL y rutas

Las direcciones URL o rutas utilizadas para obtener una vista previa de un fragmento de contenido deben ser comparables mediante [Expresiones de URL](#url-expressions). En esta versión de la aplicación WKND habilitada para la vista previa, los fragmentos de contenido de aventura se muestran mediante la variable `AdventureDetail` componente enlazado a la ruta `/adventure<CONTENT FRAGMENT PATH>`. Por lo tanto, la URL de vista previa del modelo WKND Adventure debe establecerse en `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` para resolver esta ruta.

La vista previa del fragmento de contenido solo funciona si la aplicación tiene una ruta a la que se puede dirigir, que se puede rellenar con [Expresiones de URL](#url-expressions) que procesan ese fragmento de contenido en la aplicación de forma que se pueda previsualizar.

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Switch>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*'>
            <AdventureDetail />
          </Route>
          <Route path="/">
            <Home />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

export default App;
```

### Mostrar el contenido creado

La variable `AdventureDetail` El componente simplemente analiza la ruta del fragmento de contenido, insertado en la URL de vista previa mediante la variable `${contentFragment.path}` [Expresión URL](#url-expressions), desde la dirección URL de ruta, y la utiliza para recopilar y procesar la aventura WKND.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the content fragment path value which is the parameter used to query for the adventure's details
    
    // Add the leading '/' back on since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const path = '/' + useParams()[0];

    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```

---
title: Previsualización del fragmento de contenido
description: AEM Aprenda a utilizar la previsualización de fragmentos de contenido para todos los autores a fin de ver rápidamente cómo los cambios de contenido afectan a sus experiencias sin encabezado de la.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Previsualización del fragmento de contenido

AEM Las aplicaciones sin encabezado admiten la previsualización de creación integrada. La experiencia de vista previa vincula el editor de fragmentos de contenido del autor de AEM con su aplicación personalizada (a la que se puede dirigir mediante HTTP), lo que permite establecer un vínculo profundo en la aplicación que procesa el fragmento de contenido que se está previsualizando.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

Para utilizar la vista previa de fragmentos de contenido, se deben cumplir varias condiciones:

1. La aplicación debe implementarse en una URL accesible para los autores
1. La aplicación debe configurarse para conectarse al servicio de AEM Author (en lugar de al de AEM Publish)
1. La aplicación debe diseñarse con direcciones URL o rutas que puedan utilizar [Ruta o ID del fragmento de contenido](#url-expressions) para seleccionar los fragmentos de contenido que se mostrarán para su vista previa en la experiencia de la aplicación.

## Previsualizar direcciones URL

Previsualizar direcciones URL, uso [Expresiones de URL](#url-expressions), se establecen en las Propiedades del modelo de fragmento de contenido.

![URL de vista previa del modelo de fragmento de contenido](./assets/preview/cf-model-preview-url.png)

1. Inicie sesión en el servicio de AEM Author como administrador
1. Vaya a __Herramientas > General > Modelos de fragmentos de contenido__
1. Seleccione el __Modelo de fragmento de contenido__ y seleccione __Propiedades__ de la barra de acciones superior.
1. Introduzca la URL de vista previa del modelo de fragmento de contenido mediante [Expresiones de URL](#url-expressions)
   + La URL de vista previa debe apuntar a una implementación de la aplicación que se conecta al servicio de AEM Author.

### Expresiones de URL

Cada modelo de fragmento de contenido puede tener una URL de vista previa establecida. La URL de vista previa se puede parametrizar por fragmento de contenido mediante las expresiones de URL enumeradas en la siguiente tabla. Se pueden utilizar varias expresiones URL en una sola dirección URL de vista previa.

|  | Expresión de URL | Valor |
| --------------------------------------- | ----------------------------------- | ----------- |
| Ruta de fragmento de contenido | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| ID de fragmento de contenido | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variación de fragmento de contenido | `${contentFragment.variation}` | `main` |
| Ruta del modelo de fragmento de contenido | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Nombre del modelo del fragmento de contenido | `${contentFragment.model.name}` | `adventure` |

URL de vista previa de ejemplo:

+ Una URL de vista previa en __Aventura__ el modelo podría parecerse a `https://preview.app.wknd.site/adventure${contentFragment.path}` que se resuelve en `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ Una URL de vista previa en __Artículo__ el modelo podría parecerse a `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` el resuelve `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Vista previa en la aplicación

Cualquier fragmento de contenido que utilice el modelo de fragmento de contenido configurado tiene un botón Vista previa. El botón Previsualizar abre la URL de vista previa del modelo de fragmento de contenido e inserta los valores del fragmento de contenido abierto en la [Expresiones de URL](#url-expressions).

![Botón Vista previa](./assets/preview/preview-button.png)

Realice una actualización automática (borrando la caché local del explorador) al obtener una vista previa de los cambios en los fragmentos de contenido en la aplicación.

## Ejemplo de React

AEM AEM Vamos a explorar la aplicación WKND, una aplicación sencilla de React que muestra aventuras de los usuarios que utilizan las API de GraphQL sin encabezado de la aplicación de la aplicación de la manera más rápida y sencilla.

El código de ejemplo está disponible en [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## Direcciones URL y rutas

Las direcciones URL o rutas utilizadas para previsualizar un fragmento de contenido deben ser componibles mediante [Expresiones de URL](#url-expressions). En esta versión con vista previa habilitada de la aplicación WKND, los fragmentos de contenido de aventura se muestran a través de la `AdventureDetail` componente enlazado a la ruta `/adventure<CONTENT FRAGMENT PATH>`. Por lo tanto, la URL de vista previa del modelo WKND Adventure debe configurarse como `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` para resolver en esta ruta.

La vista previa de fragmentos de contenido solo funciona si la aplicación tiene una ruta direccionable, que se puede rellenar con [Expresiones de URL](#url-expressions) que procesan ese fragmento de contenido en la aplicación de forma previsualizable.

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
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### Mostrar el contenido creado

El `AdventureDetail` El componente simplemente analiza la ruta del fragmento de contenido, insertado en la URL de vista previa mediante el `${contentFragment.path}` [Expresión de URL](#url-expressions), desde la dirección URL de ruta, y lo utiliza para recopilar y procesar la WKND Adventure.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
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

---
title: 'Aplicación React: Ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Se proporciona una aplicación React que muestra cómo consultar contenido mediante las API de GraphQL de AEM. El cliente sin encabezado de AEM para JavaScript se utiliza para ejecutar las consultas de GraphQL que alimentan la aplicación.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 0ab14016c27d3b91252f3cbf5f97550d89d4a0c9
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 2%

---


# React App

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Se proporciona una aplicación React que muestra cómo consultar contenido mediante las API de GraphQL de AEM. El cliente sin encabezado de AEM para JavaScript se utiliza para ejecutar las consultas de GraphQL que alimentan la aplicación.

![React Application](./assets/react-screenshot.png)

Hay disponible un tutorial paso a paso completo [here](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html).

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## Requisitos AEM

La aplicación está diseñada para conectarse a un AEM **Autor** o **Publicación** con la última versión de [Sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) instalado.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=es#service-pack)

Recomendamos [implementación del sitio de referencia WKND en un entorno de Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Una configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) o [6.5 QuickStart jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) también se puede utilizar.

## Utilización

1. Clonar el `aem-guides-wknd-graphql` repositorio:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Edite el `aem-guides-wknd/react-app/.env.development` y asegúrese de que `REACT_APP_HOST_URI` apunta a la instancia de AEM de destino. Actualice el método de autenticación (si se conecta a una instancia de autor).

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   ...
   ```

1. Abra un terminal y ejecute los comandos:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```
1. Se debe cargar una nueva ventana del explorador en [http://localhost:3000](http://localhost:3000)
1. Se debe mostrar en la aplicación una lista de las aventuras del sitio de referencia WKND.

## El código

A continuación se muestra un breve resumen de los archivos y el código importantes utilizados para activar la aplicación. El código completo se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql).

### AEM cliente sin encabezado para JavaScript

La variable [AEM cliente sin encabezado](https://github.com/adobe/aem-headless-client-js) se utiliza para ejecutar la consulta de GraphQL. El cliente AEM sin encabezado proporciona dos métodos para ejecutar consultas. [`runQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunqueryquery-options--promiseany) y [`runPersistedQuery`](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany).

`runQuery` ejecuta una consulta GraphQL estándar para AEM contenido y es el tipo más común de ejecución de consultas.

[Consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) son una función de AEM que almacena en caché los resultados de una consulta de GraphQL y, a continuación, hace que el resultado esté disponible durante la GET. Las consultas persistentes deben usarse para consultas comunes que se ejecutan una y otra vez. En esta aplicación, la lista de aventuras es la primera consulta ejecutada en la pantalla principal. Esta será una consulta muy popular y, por lo tanto, se debe utilizar una consulta persistente. `runPersistedQuery` ejecuta una solicitud en un extremo de consulta persistente.

`src/api/useGraphQL.js` es [Vínculo React Effect](https://reactjs.org/docs/hooks-overview.html#effect-hook) que escucha los cambios en el parámetro `query` y `path`. If `query` está en blanco, se utiliza una consulta persistente basada en la variable `path`. Aquí es donde se construye AEM cliente sin encabezado y se utiliza para recuperar datos.

```js
function useGraphQL(query, path) {
    let [data, setData] = useState(null);
    let [errorMessage, setErrors] = useState(null);

    useEffect(() => {
      // construct a new AEMHeadless client based on the graphQL endpoint
      const sdk = new AEMHeadless({ endpoint: REACT_APP_GRAPHQL_ENDPOINT })

      // if query is not null runQuery otherwise fall back to runPersistedQuery
      const request = query ? sdk.runQuery.bind(sdk) : sdk.runPersistedQuery.bind(sdk);

      request(query || path)
        .then(({ data, errors }) => {
          //If there are errors in the response set the error message
          if(errors) {
            setErrors(mapErrors(errors));
          }
          //If data in the response set the data as the results
          if(data) {
            setData(data);
          }
        })
        .catch((error) => {
          setErrors(error);
        });
    }, [query, path]);

    return {data, errorMessage}
}
```

### Contenido de aventura

La aplicación muestra principalmente una lista de aventuras y ofrece a los usuarios la opción de hacer clic en los detalles de una aventura.

`Adventures.js` - Muestra una lista de Aventuras con tarjeta.  El estado inicial hace uso de [Consultas persistentes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) que es [preempaquetado](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) con el sitio de referencia WKND. El punto final es `/wknd/adventures-all`. Hay varios botones que permiten al usuario experimentar con el filtrado de resultados en función de una actividad:

```javascript
function filterQuery(activity) {
  return `
    {
      adventureList (filter: {
        adventureActivity: {
          _expressions: [
            {
              value: "${activity}"
            }
          ]
        }
      }){
        items {
          _path
        adventureTitle
        adventurePrice
        adventureTripLength
        adventurePrimaryImage {
          ... on ImageRef {
            _path
            mimeType
            width
            height
          }
        }
      }
    }
  }
  `;
}
```

`AdventureDetail.js` - Muestra una vista detallada de la aventura. Realiza una consulta graphQL basada en la ruta a la aventura que se analiza desde la dirección url:

```javascript
//parse the content fragment from the url
const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
...
function adventureDetailQuery(_path) {
  return `{
    adventureByPath (_path: "${_path}") {
      item {
        _path
          adventureTitle
          adventureActivity
          adventureType
          adventurePrice
          adventureTripLength
          adventureGroupSize
          adventureDifficulty
          adventurePrice
          adventurePrimaryImage {
            ... on ImageRef {
              _path
              mimeType
              width
              height
            }
          }
          adventureDescription {
            html
          }
          adventureItinerary {
            html
          }
      }
    }
  }
  `;
}
```

### Variables de entorno

Varios [variables de entorno](https://create-react-app.dev/docs/adding-custom-environment-variables) se utilizan en este proyecto para conectarse a un entorno AEM. El valor predeterminado se conecta a un entorno de creación AEM que se ejecuta en http://localhost:4502. Si desea cambiar este comportamiento, actualice el `.env.development` en consecuencia:

* `REACT_APP_HOST_URI=http://localhost:4502` - Se establece en AEM host de destino
* `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json` - Establecer la ruta del extremo de GraphQL
* `REACT_APP_AUTH_METHOD=` - El método de autenticación preferido. Opcional, de forma predeterminada no se utiliza ninguna autenticación.
   * `service-token` - utilizar el intercambio de tokens de servicio para Cloud Env PROD
   * `dev-token` - usar el token de desarrollo local con Cloud Env
   * `basic` - usar user/pass para desarrollo local con Local Author Env
   * deje en blanco para no usar ningún método de autenticación
* `REACT_APP_AUTHORIZATION=admin:admin` : establezca credenciales de autenticación básicas para utilizarlas si se conectan a un entorno de AEM Author (solo para desarrollo). Si se conecta a un entorno de publicación, esta configuración no es necesaria.
* `REACT_APP_DEV_TOKEN` - Cadena de token de desarrollador. Para conectarse a una instancia remota, junto a la autenticación básica (user:pass), puede utilizar la autenticación del portador con el token DEV de la consola de la nube
* `REACT_APP_SERVICE_TOKEN` : ruta al archivo token de servicio. Para conectarse a una instancia remota, la autenticación se puede realizar con el token de servicio también (descargue el archivo desde la consola de Cloud)

### Solicitudes de API de proxy

Al utilizar el servidor de desarrollo de webpack (`npm start`) el proyecto se basa en un [configuración de proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware`. El archivo está configurado en [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) y se basa en varias variables de entorno personalizadas configuradas en `.env` y `.env.development`.

Si se conecta a un entorno de creación AEM, debe configurarse el método de autenticación correspondiente.

### CORS - Uso compartido de recursos de origen cruzado

Este proyecto se basa en una configuración CORS que se ejecuta en el entorno de AEM de destino y asume que la aplicación se ejecuta en http://localhost:3000 en modo de desarrollo. La variable [Configuración de CORs](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) forma parte del [Sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd).

![Configuración CORS](assets/cross-origin-resource-sharing-configuration.png)

*Configuración CORS de ejemplo para entorno de creación*
---
title: Uso de contenido localizado con AEM sin encabezado
description: Aprenda a utilizar GraphQL para consultar AEM de contenido localizado.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 2%

---


# Contenido localizado con AEM sin encabezado

AEM proporciona un [Marco de integración de traducción](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) para contenido sin encabezado, lo que permite traducir fácilmente fragmentos de contenido y recursos auxiliares para su uso en distintas configuraciones regionales. Este es el mismo marco utilizado para traducir otros contenidos AEM, como páginas, fragmentos de experiencias, recursos y Forms. Una vez [se ha traducido contenido sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=es), y publicado, está listo para el consumo mediante aplicaciones sin encabezado.

## Estructura de carpetas de recursos{#assets-folder-structure}

Asegúrese de que los fragmentos de contenido localizados en AEM siguen el [estructura de localización recomendada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![Carpetas de recursos AEM localizadas](./assets/localized-content/asset-folders.jpg)

Las carpetas de configuración regional deben ser del mismo nivel y el nombre de la carpeta, en lugar de el título, debe ser válido [Código ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) que representa la configuración regional del contenido contenido de la carpeta.

El código de configuración regional también es el valor utilizado para filtrar los fragmentos de contenido devueltos por la consulta de GraphQL.

| Código regional | AEM ruta | Configuración regional del contenido |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Contenido alemán |
| en | /content/dam/.../**en**/... | Contenido en inglés |
| es | /content/dam/.../**es**/... | Contenido en español |

## Consulta persistente de GraphQL

AEM proporciona un `_locale` Filtro GraphQL que filtra automáticamente el contenido por código de configuración regional . Por ejemplo, para consultar todas las aventuras de inglés en la [Proyecto de demostración de referencia WKND](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/create-site.html) se puede hacer con una nueva consulta persistente `wknd-shared/adventures-by-locale` definido como:

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

La variable `$locale` se usa en la variable `_locale` requiere el código de configuración regional (por ejemplo `en`, `en_us`o `de`) tal como se especifica en [AEM convención de localización de Asset Folder-Base](#assets-folder-structure).

## Ejemplo de reacción

Vamos a crear una aplicación React simple que controle el contenido de Aventura que se consulta desde AEM en función de un selector de configuración regional usando el `_locale` filtro.

When __Inglés__ se selecciona en el selector de configuración regional y, a continuación, en los fragmentos de contenido de aventura en inglés, en `/content/dam/wknd/en` cuando __Español__ está seleccionada y, a continuación, los fragmentos de contenido en español en `/content/dam/wknd/es`, etc.

![Aplicación de ejemplo React localizada](./assets/localized-content/react-example.png)

### Cree un `LocaleContext`{#locale-context}

En primer lugar, cree un [Contexto de reacción](https://reactjs.org/docs/context.html) para permitir que la configuración regional se utilice en los componentes de la aplicación React.

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### Cree un `LocaleSwitcher` React, componente{#locale-switcher}

A continuación, cree un componente React del conmutador de configuración regional que establezca el valor [El contexto de configuración regional](#locale-context) a la selección del usuario.

Este valor de configuración regional se utiliza para dirigir las consultas de GraphQL, asegurándose de que solo devuelvan contenido que coincida con la configuración regional seleccionada.

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### Consulte el contenido utilizando la variable `_locale` filter{#adventures}

El componente Adventures consulta AEM todas las aventuras por configuración regional y enumera sus títulos. Esto se logra pasando el valor de configuración regional almacenado en el contexto React a la consulta utilizando la variable `_locale` filtro.

Este método se puede ampliar a otras consultas de la aplicación, lo que garantiza que todas las consultas solo incluyan contenido especificado por la selección de configuración regional de un usuario.

La consulta con AEM se realiza en el vínculo React personalizado [getAdventuresByLocale, descrito con más detalle en la documentación de Querying AEM GraphQL](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### Defina el `App.js`{#app-js}

Por último, vincúlelo todo al ajustar la aplicación React con la variable `LanguageContext.Provider` y estableciendo el valor de configuración regional. Esto permite que los demás componentes de React: [LocaleSwitcher](#locale-switcher)y [Aventuras](#adventures) para compartir el estado de selección de la configuración regional.

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```

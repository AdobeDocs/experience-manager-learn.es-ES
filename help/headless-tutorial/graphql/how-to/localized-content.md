---
title: Uso de contenido localizado con AEM Headless
description: Aprenda a utilizar GraphQL para consultar contenido localizado en AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---

# Contenido localizado con AEM sin encabezado

AEM proporciona un [Marco de trabajo de integración de traducciones](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html?lang=es) para contenido sin encabezado, lo que permite que los fragmentos de contenido y los recursos de soporte se traduzcan fácilmente para su uso en distintas configuraciones regionales. Este es el mismo marco de trabajo que se utiliza para traducir otro contenido de AEM, como páginas, fragmentos de experiencias, Assets y Forms. Una vez que se ha traducido [contenido sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=es) y se ha publicado, está listo para que lo consuman las aplicaciones sin encabezado.

## Estructura de carpetas de Assets{#assets-folder-structure}

Asegúrese de que los fragmentos de contenido localizados en AEM sigan la [estructura de localización recomendada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html?lang=es#recommended-structure).

![Carpetas de recursos localizadas de AEM](./assets/localized-content/asset-folders.jpg)

Las carpetas de configuración regional deben ser elementos del mismo nivel y el nombre de la carpeta, en lugar del título, debe ser un [código ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) válido que represente la configuración regional del contenido incluido en la carpeta.

El código de configuración regional también es el valor utilizado para filtrar los fragmentos de contenido devueltos por la consulta de GraphQL.

| Código de configuración regional | Ruta de AEM | Configuración regional de contenido |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Contenido en alemán |
| en | /content/dam/.../**en**/... | Contenido en inglés |
| es | /content/dam/.../**es**/... | Contenido en español |

## Consulta persistente de GraphQL

AEM proporciona un filtro de GraphQL `_locale` que filtra automáticamente el contenido por código de configuración regional Por ejemplo, consultar todas las aventuras en inglés en el [proyecto del sitio WKND](https://github.com/adobe/aem-guides-wknd) se puede hacer con una nueva consulta persistente `wknd-shared/adventures-by-locale` definida como:

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

La variable `$locale` utilizada en el filtro `_locale` requiere el código de configuración regional (por ejemplo `en`, `en_us` o `de`) especificado en la [convención de localización basada en carpetas de recursos de AEM](#assets-folder-structure).

## Ejemplo de React

Vamos a crear una aplicación React simple que controle qué contenido de Adventure consultar desde AEM en función de un selector de configuración regional usando el filtro `_locale`.

Cuando se selecciona __English__ en el selector de configuración regional, se devuelven los fragmentos de contenido de aventura en inglés de `/content/dam/wknd/en`, cuando se selecciona __Spanish__, luego los fragmentos de contenido en español de `/content/dam/wknd/es`, etc., etc.

![Aplicación de ejemplo React localizada](./assets/localized-content/react-example.png)

### Crear un(a) `LocaleContext`{#locale-context}

Primero, cree un [contexto React](https://reactjs.org/docs/context.html) para permitir que se use la configuración regional en los componentes de la aplicación React.

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

### Crear un componente de React `LocaleSwitcher`{#locale-switcher}

A continuación, cree un componente React del conmutador de configuración regional que establezca el valor [LocaleContext&#39;s](#locale-context) en la selección del usuario.

Este valor de configuración regional se utiliza para dirigir las consultas de GraphQL, asegurándose de que solo devuelven contenido que coincida con la configuración regional seleccionada.

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

### Consultar contenido mediante el filtro `_locale`{#adventures}

El componente Aventuras consulta AEM todas las aventuras por configuración regional y enumera sus títulos. Esto se logra pasando el valor de configuración regional almacenado en el contexto de React a la consulta mediante el filtro `_locale`.

Este método se puede ampliar a otras consultas de la aplicación, asegurándose de que todas las consultas incluyan únicamente el contenido especificado por la configuración regional seleccionada por el usuario.

La consulta a AEM se realiza en el vínculo personalizado de React [getAdventuresByLocale, que se describe con más detalle en la sección Consulta de la documentación de AEM GraphQL](./aem-headless-sdk.md).

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

### Definir `App.js`{#app-js}

Por último, vincule todo ajustando la aplicación React con `LanguageContext.Provider` y estableciendo el valor de configuración regional. Esto permite que los demás componentes de React, [LocaleSwitcher](#locale-switcher) y [Adventures](#adventures), compartan el estado de selección de configuración regional.

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

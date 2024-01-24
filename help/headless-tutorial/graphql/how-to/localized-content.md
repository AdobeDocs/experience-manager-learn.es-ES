---
title: AEM Uso de contenido localizado con sin encabezado
description: Aprenda a utilizar GraphQL AEM para consultar el contenido localizado en las listas de contenido de su sitio.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 162
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---

# AEM Contenido localizado con sin encabezado

AEM proporciona un [Marco de integración de traducción](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) para contenido sin encabezado, lo que permite que los fragmentos de contenido y los recursos de soporte se traduzcan fácilmente para su uso en varias configuraciones regionales. AEM Este es el mismo marco de trabajo que se utiliza para traducir otro contenido, como páginas, fragmentos de experiencias, recursos y Forms. Una [se ha traducido el contenido sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=es), y publicado, está listo para su consumo por aplicaciones sin encabezado.

## Estructura de carpetas de recursos{#assets-folder-structure}

AEM Asegúrese de que los fragmentos de contenido localizados en siguen de manera la [estructura de localización recomendada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![AEM Carpetas de recursos localizadas de la](./assets/localized-content/asset-folders.jpg)

Las carpetas de configuración regional deben ser elementos del mismo nivel y el nombre de la carpeta, en lugar del título, debe ser un válido [Código ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) que representa la configuración regional del contenido contenido de la carpeta.

El código de configuración regional también es el valor utilizado para filtrar los fragmentos de contenido devueltos por la consulta de GraphQL.

| Código de configuración regional | AEM ruta de acceso de la | Configuración regional de contenido |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Contenido en alemán |
| en | /content/dam/.../**en**/... | Contenido en inglés |
| es | /content/dam/.../**es**/... | Contenido en español |

## Consulta persistente de GraphQL

AEM proporciona un `_locale` Filtro de GraphQL que filtra automáticamente el contenido por código de configuración regional Por ejemplo, consultar todas las aventuras en inglés en la [Proyecto del sitio WKND](https://github.com/adobe/aem-guides-wknd) se puede hacer con una nueva consulta persistente `wknd-shared/adventures-by-locale` definido como:

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

El `$locale` utilizada en la variable `_locale` Este filtro requiere el código de configuración regional (por ejemplo, `en`, `en_us`, o `de`) según se especifica en [AEM convención de localización basada en carpetas de recursos](#assets-folder-structure).

## Ejemplo de React

AEM Vamos a crear una aplicación React simple que controle desde qué contenido de Adventure se va a realizar la consulta en función de un selector de configuración regional usando la variable `_locale` filtro.

Cuándo __Inglés__ se selecciona en el selector de configuración regional y, a continuación, Fragmentos de contenido de aventura en inglés en `/content/dam/wknd/en` se devuelven, cuando __Español__ se selecciona y luego Fragmentos de contenido en español en `/content/dam/wknd/es`, etc., etc.

![Aplicación de ejemplo React localizada](./assets/localized-content/react-example.png)

### Crear un `LocaleContext`{#locale-context}

En primer lugar, cree un [Contexto de React](https://reactjs.org/docs/context.html) para permitir que la configuración regional se utilice en los componentes de la aplicación React.

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

### Crear un `LocaleSwitcher` Componente React{#locale-switcher}

A continuación, cree un componente React del conmutador de configuración regional que establezca en [El contexto local](#locale-context) a la selección del usuario.

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

### Consulte el contenido mediante `_locale` filter{#adventures}

AEM El componente Aventuras consulta las de todas las aventuras por configuración regional y enumera sus títulos. Esto se logra pasando el valor de configuración regional almacenado en el contexto de React a la consulta utilizando `_locale` filtro.

Este método se puede ampliar a otras consultas de la aplicación, asegurándose de que todas las consultas incluyan únicamente el contenido especificado por la configuración regional seleccionada por el usuario.

AEM La consulta contra la se realiza en el vínculo React personalizado [AEM getAdventuresByLocale, descrito con más detalle en la documentación de Consulta de GraphQL](./aem-headless-sdk.md).

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

Por último, enlácelo todo envolviendo la aplicación React con el `LanguageContext.Provider` y establecer el valor de configuración regional. Esto permite a los demás componentes de React, [LocaleSwitcher](#locale-switcher), y [Aventuras](#adventures) para compartir el estado de selección de la configuración regional.

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

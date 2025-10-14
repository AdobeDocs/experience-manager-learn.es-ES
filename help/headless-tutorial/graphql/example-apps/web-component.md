---
title: 'Componente web/JS: ejemplo para AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación Web Component/JS muestra cómo consultar contenido mediante las API GraphQL de AEM utilizando consultas persistentes.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 129
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 3%

---

# Componente web

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin encabezado de Adobe Experience Manager (AEM). Esta aplicación de componente web muestra cómo consultar contenido mediante las API de GraphQL de AEM utilizando consultas persistentes y representando una parte de la interfaz de usuario, todo ello realizado utilizando código JavaScript puro.

![Componente web con AEM Headless](./assets/web-component/web-component.png)

Ver el [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Requisitos de AEM

El componente web funciona con las siguientes opciones de implementación de AEM.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=es)
+ Configuración local mediante [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)
   + Requiere [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&fulltext=Oracle%7E+JDK%7E+11%7E&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=14) (si se conecta a AEM 6.5 local o AEM SDK)

Esta aplicación de ejemplo depende de [basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip) que se va a instalar y de que se hayan implementado las [configuraciones de implementación](../deployment/web-component.md) necesarias.


>[!IMPORTANT]
>
>El componente web está diseñado para conectarse a un entorno __AEM Publish__, pero puede obtener contenido de AEM Author si se proporciona autenticación en el archivo [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) del componente web.

## Cómo usar

1. Clonar el repositorio `adobe/aem-guides-wknd-graphql`:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Vaya al subdirectorio `web-component`.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Edite el archivo `.../src/person.js` para incluir los detalles de conexión de AEM:

   En el objeto `aemHeadlessService`, actualice `aemHost` para que apunte a su servicio de publicación de AEM.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Si se conecta a un servicio de AEM Author, en el objeto `aemCredentials`, proporcione las credenciales de usuario locales de AEM.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Abra un terminal y ejecute los comandos de `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Una nueva ventana del explorador abre la página estática de HTML que incrusta el componente web en [http://localhost:8080](http://localhost:8080).
1. El componente web _Información de persona_ se muestra en la página web.

## El código

A continuación se muestra un resumen de cómo se crea el componente web, cómo se conecta a AEM sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se encuentra en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Etiqueta de HTML de componente web

Se agrega un componente web reutilizable (también conocido como elemento personalizado) `<person-info>` a la página de HTML `../src/assets/aem-headless.html`. Admite los atributos `host` y `query-param-value` para controlar el comportamiento del componente. El valor del atributo `host` invalida el valor `aemHost` del objeto `aemHeadlessService` en `person.js`, y `query-param-value` se usa para seleccionar a la persona que se va a procesar.

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementación de componentes web

`person.js` define la funcionalidad del componente web y a continuación se muestran los aspectos destacados de la misma.

#### Implementación del elemento PersonInfo

El objeto de clase del elemento personalizado `<person-info>` define la funcionalidad mediante los métodos de ciclo de vida `connectedCallback()`, adjuntando una raíz central, recuperando la consulta persistente de GraphQL y la manipulación DOM para crear la estructura DOM central interna del elemento personalizado.

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src',
            host + (person.profilePicture._dynamicUrl || person.profilePicture._path));
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Registrar el elemento `<person-info>`

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Uso compartido de recursos de origen cruzado (CORS)

Este componente web se basa en una configuración CORS basada en AEM que se ejecuta en el entorno AEM de destino y supone que la página host se ejecuta en `http://localhost:8080` en modo de desarrollo y a continuación se muestra un ejemplo de configuración CORS OSGi para el servicio AEM Author local.

Revise [configuraciones de implementación](../deployment/web-component.md) para el servicio de AEM correspondiente.

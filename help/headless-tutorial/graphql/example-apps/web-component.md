---
title: 'Componente web/JS: ejemplo AEM sin encabezado'
description: Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación de componente web/JS muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 6%

---


# Componente web

Las aplicaciones de ejemplo son una buena manera de explorar las capacidades sin objetivos de Adobe Experience Manager (AEM). Esta aplicación de componente web muestra cómo consultar contenido mediante las API de GraphQL AEM mediante consultas persistentes y procesar una parte de la interfaz de usuario, realizada mediante código JavaScript puro.

![Componente web con AEM sin encabezado](./assets/web-component/web-component.png)

Consulte la [código fuente en GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Requisitos previos {#prerequisites}

Las siguientes herramientas deben instalarse localmente:

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (si se conecta al SDK local AEM 6.5 o AEM)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM requisitos

El componente web funciona con las siguientes opciones de implementación AEM.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Configuración local mediante [el SDK de AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es)
+ [Inicio rápido de AEM 6.5 SP13+](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es?lang=en#install-local-aem-instances)

Todas las implementaciones requieren la `tutorial-solution-content.zip` de la variable [Archivos de solución](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) para instalar y ser necesario [Configuraciones de implementación](../deployment/web-component.md) se realizan.


>[!IMPORTANT]
>
>El componente web está diseñado para conectarse a un __AEM Publish__ , sin embargo, puede obtener contenido de AEM Author si la autenticación se proporciona en el [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) archivo.

## Utilización

1. Clonar el `adobe/aem-guides-wknd-graphql` repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Vaya a `web-component` subdirectorio.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Edite el `.../src/person.js` para incluir los detalles de conexión de AEM:

   En el `aemHeadlessService` , actualice la variable `aemHost` para que apunte a su servicio de AEM Publish.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Si se conecta a un servicio de AEM Author, en la sección `aemCredentials` , proporcione las credenciales de usuario AEM locales.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Abra un terminal y ejecute los comandos desde `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Una nueva ventana del explorador abre la página del HTML estático que incrusta el componente web en [http://localhost:8080](http://localhost:8080).
1. La variable _Información de persona_ El componente web se muestra en la página web.

## El código

A continuación se muestra un resumen de cómo se crea el componente web, cómo se conecta a AEM sin encabezado para recuperar contenido mediante consultas persistentes de GraphQL y cómo se presentan esos datos. El código completo se puede encontrar en [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Etiqueta del HTML de componentes web

Un componente web reutilizable (también conocido como elemento personalizado) `<person-info>` se agrega a la variable `../src/assets/aem-headless.html` página del HTML. Admite `host` y `query-param-value` para dirigir el comportamiento del componente. La variable `host` anulaciones de valores de atributo `aemHost` valor de `aemHeadlessService` en `person.js`y `query-param-value` se utiliza para seleccionar la persona que se va a procesar.

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implementación de componentes web

La variable `person.js` define la funcionalidad del componente web y a continuación se indican los aspectos destacados de la misma.

#### Implementación de elementos PersonInfo

La variable `<person-info>` el objeto de clase del elemento personalizado define la funcionalidad empleando la variable `connectedCallback()` métodos de ciclo de vida, adjuntar una raíz de sombra, recuperar la consulta persistente de GraphQL y la manipulación DOM para crear la estructura DOM interna del elemento de sombra personalizado.

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
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Registre el `<person-info>` element

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Uso compartido de recursos de origen diverso (CORS)

Este componente web se basa en una configuración CORS basada en AEM que se ejecuta en el entorno de AEM de destino y asume que la página host se ejecuta en `http://localhost:8080` en el modo de desarrollo y a continuación se muestra una configuración OSGi CORS de muestra para el servicio local AEM Author.

Consulte [configuraciones de implementación](../deployment/web-component.md) para el servicio AEM correspondiente.

![Configuración CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)

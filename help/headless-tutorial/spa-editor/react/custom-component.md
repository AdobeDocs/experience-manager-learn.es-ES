---
title: Crear un componente meteorológico personalizado | AEM SPA Introducción al Editor de y React
description: AEM SPA Obtenga información sobre cómo crear un componente meteorológico personalizado para utilizarlo con el Editor de tiempo de la. Aprenda a desarrollar cuadros de diálogo de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado. Se utilizan los componentes Open Weather API y React Open Weather.
feature: SPA Editor
version: Cloud Service
jira: KT-5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 82466e0e-b573-440d-b806-920f3585b638
duration: 323
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1105'
ht-degree: 0%

---

# Crear un componente meteorológico personalizado {#custom-component}

AEM SPA Obtenga información sobre cómo crear un componente meteorológico personalizado para utilizarlo con el Editor de tiempo de la. Aprenda a desarrollar cuadros de diálogo de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado. Se usan la [API de clima abierto](https://openweathermap.org) y el [componente React de clima abierto](https://www.npmjs.com/package/react-open-weather).

## Objetivo

1. AEM Comprenda el papel de los modelos Sling en la manipulación de la API del modelo JSON proporcionada por los usuarios de la interfaz de usuario de.
2. AEM Obtenga información sobre cómo crear nuevos cuadros de diálogo de componentes de.
3. AEM SPA Aprenda a crear un componente **personalizado** de la aplicación que sea compatible con el marco de trabajo del editor de.

## Qué va a generar

Se crea un componente meteorológico simple. SPA Los autores de contenido pueden añadir este componente a la. AEM Mediante un cuadro de diálogo de, los autores pueden establecer la ubicación del tiempo que se mostrará.  AEM AEM SPA La implementación de este componente ilustra los pasos necesarios para crear un componente de red de nueva creación que sea compatible con el marco de trabajo del Editor de.

![Configurar el componente de tiempo abierto](assets/custom-component/enter-dialog.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). SPA AEM AEM Este capítulo es una continuación del capítulo [Navegación y enrutamiento](navigation-routing.md); sin embargo, para continuar, todo lo que necesita es un proyecto de habilitado para el uso de la local implementado en una instancia de la aplicación.

### Abrir clave de API meteorológica

Se necesita una clave de API de [Tiempo abierto](https://openweathermap.org/) para seguir junto con el tutorial. [El registro es gratis](https://home.openweathermap.org/users/sign_up) para una cantidad limitada de llamadas API.

## AEM Definición del componente de

AEM Un componente se define como un nodo y propiedades. En el proyecto, estos nodos y propiedades se representan como archivos XML en el módulo `ui.apps`. AEM A continuación, cree el componente de en el módulo `ui.apps`.

>[!NOTE]
>
> AEM Un repaso rápido de los [conceptos básicos de los componentes de la puede ser útil](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. En el IDE que elija, abra la carpeta `ui.apps`.
2. Vaya a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` y cree una nueva carpeta llamada `open-weather`.
3. Cree un nuevo archivo con el nombre `.content.xml` debajo de la carpeta `open-weather`. Rellene `open-weather/.content.xml` con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Crear definición de componente personalizado](assets/custom-component/aem-custom-component-definition.png)

   AEM `jcr:primaryType="cq:Component"`: identifica que este nodo es un componente de la.

   `jcr:title` es el valor que se muestra a los autores de contenido y `componentGroup` determina la agrupación de componentes en la interfaz de usuario de creación.

4. Debajo de la carpeta `custom-component`, cree otra carpeta denominada `_cq_dialog`.
5. Debajo de la carpeta `_cq_dialog` cree un nuevo archivo denominado `.content.xml` y rellénelo con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![Definición de componente personalizado](assets/custom-component/dialog-custom-component-defintion.png)

   El archivo XML anterior genera un cuadro de diálogo muy sencillo para `Weather Component`. La parte crítica del archivo son los nodos internos `<label>`, `<lat>` y `<lon>`. Este cuadro de diálogo contiene dos `numberfield`s y un `textfield` que permiten al usuario configurar el tiempo que se mostrará.

   Se crea un modelo Sling junto a para exponer el valor de las propiedades `label`,`lat` y `long` a través del modelo JSON.

   >[!NOTE]
   >
   > Puede ver muchos más [ejemplos de cuadros de diálogo al ver las definiciones de los componentes principales](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). También puede ver campos de formulario adicionales, como `select`, `textarea`, `pathfield`, disponibles debajo de `/libs/granite/ui/components/coral/foundation/form` en [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   AEM Con un componente tradicional de la, se suele requerir un script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=es). SPA Dado que el componente se procesará mediante el script, no se necesita un script HTL.

## Creación del modelo Sling

Los modelos Sling son objetos Java antiguos comunes (&quot;POJO&quot;) impulsados por anotaciones que facilitan la asignación de datos desde el JCR a variables Java. AEM [Los modelos Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models) suelen funcionar para encapsular lógica empresarial compleja del lado del servidor para componentes de la.

SPA En el contexto del Editor de modelos de Sling, los modelos de Sling exponen el contenido de un componente a través del modelo JSON mediante una función que utiliza el [Exportador del modelo Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=es).

1. En el IDE de su elección, abra el módulo `core` en `aem-guides-wknd-spa.react/core`.
1. Cree un archivo con el nombre `OpenWeatherModel.java` en `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Rellene `OpenWeatherModel.java` con lo siguiente:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
       public String getLabel();
       public double getLat();
       public double getLon();
   }
   ```

   Esta es la interfaz de Java para nuestro componente. SPA Para que nuestro modelo Sling sea compatible con el marco de trabajo del Editor de la, debe ampliar la clase `ComponentExporter`.

1. Cree una carpeta denominada `impl` debajo de `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Cree un archivo de nombre `OpenWeatherModelImpl.java` por debajo de `impl` y rellénelo con lo siguiente:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   La variable estática `RESOURCE_TYPE` debe apuntar a la ruta de acceso en `ui.apps` del componente. SPA `getExportedType()` se usa para asignar las propiedades JSON al componente de la a través de `MapTo`. `@ValueMapValue` es una anotación que lee la propiedad jcr guardada por el cuadro de diálogo.

## SPA Actualización del estado de la

AEM A continuación, actualice el código React para incluir el componente [React Open Weather](https://www.npmjs.com/package/react-open-weather) y haga que se asigne al componente de la aplicación de tiempo abierto creado en los pasos anteriores.

1. Instale el componente React Open Weather como una dependencia **npm**:

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. Cree una nueva carpeta denominada `OpenWeather` en `ui.frontend/src/components/OpenWeather`.
1. Agregue un archivo de nombre `OpenWeather.js` y rellénelo con lo siguiente:

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if (OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. Actualizar `import-components.js` a las `ui.frontend/src/components/import-components.js` para incluir el componente `OpenWeather`:

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. AEM Implemente todas las actualizaciones en un entorno de local desde la raíz del directorio del proyecto con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Actualizar la directiva de plantilla

AEM SPA A continuación, navegue hasta la para comprobar las actualizaciones y permitir que se agregue el componente `OpenWeather` al elemento de la lista de elementos que se van a.

1. Compruebe el registro del nuevo modelo Sling navegando hasta [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   Debería ver las dos líneas anteriores que indican que `OpenWeatherModelImpl` está asociado con el componente `wknd-spa-react/components/open-weather` y que está registrado a través del Exportador de modelos Sling.

1. SPA Vaya a la plantilla de página de la página de en [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Actualice la directiva del contenedor de diseño para agregar el nuevo `Open Weather` como componente permitido:

   ![Actualizar directiva de contenedor de diseño](assets/custom-component/custom-component-allowed.png)

   Guarde los cambios en la directiva y observe `Open Weather` como un componente permitido:

   ![Componente personalizado como componente permitido](assets/custom-component/custom-component-allowed-layout-container.png)

## Crear el componente Tiempo abierto

AEM SPA A continuación, cree el componente `Open Weather` mediante el Editor de.

1. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
1. En el modo `Edit`, agregue `Open Weather` a `Layout Container`:

   ![Insertar nuevo componente](assets/custom-component/insert-custom-component.png)

1. Abra el cuadro de diálogo del componente e introduzca **Etiqueta**, **Latitud** y **Longitud**. Por ejemplo **San Diego**, **32.7157** y **-117.1611**. Los números del hemisferio occidental y del hemisferio sur se representan como números negativos con la API de clima abierto

   ![Configurar el componente de tiempo abierto](assets/custom-component/enter-dialog.png)

   Este es el cuadro de diálogo que se creó en función del archivo XML anteriormente en el capítulo.

1. Guarde los cambios. Observe que ahora se muestra el tiempo en **San Diego**:

   ![Componente meteorológico actualizado](assets/custom-component/weather-updated.png)

1. Vea el modelo JSON navegando a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Buscar `wknd-spa-react/components/open-weather`:

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   El modelo Sling genera los valores JSON. Estos valores JSON se pasan al componente React como props.

## Enhorabuena. {#congratulations}

AEM SPA ¡Enhorabuena! Ha aprendido a crear un componente de personalizado para utilizarlo con el Editor de segmentos de la aplicación También ha aprendido cómo los cuadros de diálogo, las propiedades JCR y los modelos Sling interactúan para generar el modelo JSON.

### Siguientes pasos {#next-steps}

AEM AEM SPA [Ampliar un componente principal](extend-component.md): aprenda a ampliar un componente principal de la existente para utilizarlo con el Editor de la. AEM SPA Comprender cómo añadir propiedades y contenido a un componente existente es una técnica potente para ampliar las capacidades de una implementación de Editor de segmentos de tiempo de ejecución de la aplicación de un editor de segmentos de tiempo de ejecución de la aplicación de un editor de tiempo de ejecución de la.

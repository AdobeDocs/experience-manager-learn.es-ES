---
title: Personalización de la capa de datos del cliente de Adobe con componentes AEM
description: Obtenga información sobre cómo personalizar la capa de datos del cliente de Adobe con contenido de componentes de AEM personalizados. Aprenda a utilizar las API proporcionadas por AEM componentes principales para ampliar y personalizar la capa de datos.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2016'
ht-degree: 3%

---

# Personalización de la capa de datos del cliente de Adobe con componentes AEM {#customize-data-layer}

Obtenga información sobre cómo personalizar la capa de datos del cliente de Adobe con contenido de componentes de AEM personalizados. Obtenga información sobre cómo utilizar las API proporcionadas por [AEM los componentes principales para ampliar](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) y personalizar la capa de datos.

## Qué va a generar

![Capa de datos de firma](assets/adobe-client-data-layer/byline-data-layer-html.png)

En este tutorial, explorará varias opciones para ampliar la capa de datos del cliente de Adobe mediante la actualización de WKND [Componente de firma](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html). Este es un componente personalizado y las lecciones aprendidas en este tutorial se pueden aplicar a otros componentes personalizados.

### Objetivos {#objective}

1. Inserción de datos de componentes en la capa de datos mediante la ampliación de un modelo Sling y un HTL de componente
1. Utilice las utilidades de capa de datos del componente principal para reducir los esfuerzos
1. Utilice los atributos de datos del componente principal para conectarse a eventos de capa de datos existentes

## Requisitos previos {#prerequisites}

A **entorno de desarrollo local** es necesario para completar este tutorial. Las capturas de pantalla y el vídeo se capturan mediante el SDK as a Cloud Service de AEM que se ejecuta en un macOS. Los comandos y el código son independientes del sistema operativo local a menos que se indique lo contrario.

**¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).

**¿Es nuevo en AEM 6.5?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Descargar e implementar el sitio de referencia de WKND {#set-up-wknd-site}

Este tutorial amplía el componente Byline en el sitio de referencia de WKND. Clone e instale la base de código WKND en su entorno local.

1. Inicio rápido local **author** instancia de AEM que se ejecuta en [http://localhost:4502](http://localhost:4502).
1. Abra una ventana de terminal y clone la base de código WKND utilizando Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Implemente la base de código WKND en una instancia local de AEM:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 y el Service Pack más reciente, agregue la variable `classic` perfil al comando Maven:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Abra una nueva ventana del explorador e inicie sesión en AEM. Abra un **Revista** página como: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Componente de firma en la página](assets/adobe-client-data-layer/byline-component-onpage.png)

   Debería ver un ejemplo del componente Byline que se ha añadido a la página como parte de un fragmento de experiencia. Puede ver el fragmento de experiencia en [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Abra las herramientas para desarrolladores e introduzca el siguiente comando en la sección **Consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect proporciona la respuesta para ver el estado actual de la capa de datos en un sitio AEM. Debería ver información sobre la página y los componentes individuales.

   ![Respuesta de capa de datos de Adobe](assets/data-layer-state-response.png)

   Observe que el componente Byline no aparece en la capa de datos.

## Actualizar el modelo de Sling de firma {#sling-model}

Para insertar datos sobre el componente en la capa de datos, primero debemos actualizar el modelo Sling del componente. A continuación, actualice la interfaz Java de Byline y la implementación del modelo Sling para agregar un nuevo método `getData()`. Este método contendrá las propiedades que queremos insertar en la capa de datos.

1. En el IDE de su elección, abra el `aem-guides-wknd` proyecto. Vaya a la `core` módulo.
1. Abra el archivo . `Byline.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interfaz Java de Byline](assets/adobe-client-data-layer/byline-java-interface.png)

1. Agregue un nuevo método a la interfaz:

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. Abra el archivo . `BylineImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   Esta es la implementación de la variable `Byline` y se implementa como modelo de Sling.

1. Agregue las siguientes instrucciones de importación al principio del archivo:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   La variable `fasterxml.jackson` Las API se utilizan para serializar los datos que queremos exponer como JSON. La variable `ComponentUtils` de AEM componentes principales se utilizan para comprobar si la capa de datos está habilitada.

1. Añadir el método no implementado `getData()` a `BylineImple.java`:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   En el método anterior, se ha añadido una nueva `HashMap` se usa para capturar las propiedades que queremos exponer como JSON. Observe que existen métodos como `getName()` y `getOccupations()` se utilizan. `@type` representa el tipo de recurso único del componente, lo que permite a un cliente identificar fácilmente eventos o déclencheur según el tipo de componente.

   La variable `ObjectMapper` se utiliza para serializar las propiedades y devolver una cadena JSON. Esta cadena JSON se puede insertar en la capa de datos.

1. Abra una ventana de terminal. Genere e implemente solo la variable `core` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Actualizar el HTL de firma {#htl}

A continuación, actualice la variable `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL (lenguaje de plantilla de HTML) es la plantilla utilizada para representar el HTML del componente.

Un atributo de datos especial `data-cmp-data-layer` en cada componente AEM se utiliza para exponer su capa de datos.  JavaScript proporcionado por AEM componentes principales busca este atributo de datos, cuyo valor se rellena con la cadena JSON devuelta por el modelo de Sling de Byline `getData()` e inserta los valores en la capa de datos del cliente de Adobe.

1. En el IDE, abra el `aem-guides-wknd` proyecto. Vaya a la `ui.apps` módulo.
1. Abra el archivo . `byline.html` at `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![HTML de firma](assets/adobe-client-data-layer/byline-html-template.png)

1. Actualizar `byline.html` para incluir el `data-cmp-data-layer` atributo:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   El valor de `data-cmp-data-layer` se ha configurado como `"${byline.data}"` donde `byline` es el modelo de Sling actualizado anteriormente. `.data` es la notación estándar para llamar a un método Java Getter en HTL de `getData()` implementado en el ejercicio anterior.

1. Abra una ventana de terminal. Genere e implemente solo la variable `ui.apps` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Vuelva al explorador y vuelva a abrir la página con un componente de firma: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Abra las herramientas para desarrolladores e inspeccione el origen de HTML de la página para el componente Byline :

   ![Capa de datos de firma](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Debería ver que la variable `data-cmp-data-layer` se ha rellenado con la cadena JSON del modelo Sling.

1. Abra las herramientas para desarrolladores del explorador e introduzca el siguiente comando en la sección **Consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Debajo de la respuesta en `component` para encontrar la instancia de la variable `byline` se ha añadido a la capa de datos:

   ![Parte de firma de la capa de datos](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   Debería ver una entrada como la siguiente:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Observe que las propiedades expuestas son las mismas que se agregan en la variable `HashMap` en el Modelo Sling.

## Agregar un evento de clic {#click-event}

La capa de datos del cliente de Adobe está controlada por eventos y uno de los eventos más comunes para almacenar en déclencheur una acción es la `cmp:click` evento. Los componentes principales de AEM facilitan el registro de su componente con la ayuda del elemento de datos: `data-cmp-clickable`.

Los elementos en los que se puede hacer clic suelen ser un botón de llamada a acción o un vínculo de navegación. Lamentablemente, el componente Byline no tiene ninguno de estos componentes, pero lo registraremos de todas formas, ya que podría ser común para otros componentes personalizados.

1. Abra el `ui.apps` en su IDE
1. Abra el archivo . `byline.html` at `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Actualizar `byline.html` para incluir el `data-cmp-clickable` en el **name** elemento:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Abra un terminal nuevo. Genere e implemente solo la variable `ui.apps` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Vuelva al explorador y vuelva a abrir la página con el componente Byline añadido: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Para probar nuestro evento, agregaremos manualmente algo de JavaScript usando la consola del desarrollador. Consulte [Uso de la capa de datos del cliente de Adobe con AEM componentes principales](data-layer-overview.md) para ver un vídeo sobre cómo hacerlo.

1. Abra las herramientas para desarrolladores del explorador e introduzca el siguiente método en la sección **Consola**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Este método simple debe gestionar el clic del nombre del componente Byline.

1. Introduzca el método siguiente en la **Consola**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   El método anterior empuja un detector de eventos a la capa de datos para detectar la `cmp:click` y llama a la función `bylineClickHandler`.

   >[!CAUTION]
   >
   > Es importante **not** para actualizar el explorador a lo largo de este ejercicio, de lo contrario se pierde el JavaScript de la consola.

1. En el explorador, con la variable **Consola** abra, haga clic en el nombre del autor en el componente Byline :

   ![Componente de línea en el que se hizo clic](assets/adobe-client-data-layer/byline-component-clicked.png)

   Debería ver el mensaje de la consola `Byline Clicked!` y el nombre de la línea.

   La variable `cmp:click` es el más fácil de conectar. Para componentes más complejos y para rastrear otro comportamiento, es posible añadir javascript personalizado para añadir y registrar nuevos eventos. Un ejemplo bueno es el componente Carrusel, que déclencheur un `cmp:show` cada vez que se alterne una diapositiva. Consulte la [código fuente para obtener más información](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Uso de la utilidad DataLayerBuilder {#data-layer-builder}

Cuando el modelo Sling era [actualizado](#sling-model) anteriormente en el capítulo, hemos elegido crear la cadena JSON utilizando un `HashMap` y configurar cada una de las propiedades manualmente. Este método funciona bien para los componentes pequeños independientes, pero para los componentes que amplían los componentes principales AEM esto podría resultar en un montón de código adicional.

Una clase de utilidad, `DataLayerBuilder`, existe para realizar la mayor parte del trabajo pesado. Esto permite que las implementaciones amplíen solo las propiedades que desean. Actualicemos el modelo de Sling para usar el `DataLayerBuilder`.

1. Vuelva al IDE y vaya a la `core` módulo.
1. Abra el archivo . `Byline.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modifique el `getData()` método para devolver un tipo de `ComponentData`

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` es un objeto proporcionado por AEM componentes principales. Resulta en una cadena JSON, como en el ejemplo anterior, pero también realiza un gran número de trabajos adicionales.

1. Abra el archivo . `BylineImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Añada las siguientes instrucciones de importación:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Sustituya el `getData()` con el siguiente método:

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   El componente Línea vuelve a utilizar partes del componente principal de imagen para mostrar una imagen que represente al autor. En el fragmento anterior, la variable [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) se utiliza para ampliar la capa de datos del `Image` componente. Esto rellena previamente el objeto JSON con todos los datos sobre la imagen utilizada. También realiza algunas de las funciones rutinarias, como configurar la variable `@type` y el identificador único del componente. Observe que el método es realmente pequeño!

   La única propiedad extendió la variable `withTitle` que se sustituye por el valor de `getName()`.

1. Abra una ventana de terminal. Genere e implemente solo la variable `core` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Vuelva al IDE y abra el `byline.html` file under `ui.apps`
1. Actualizar el HTL para usar `byline.data.json` para rellenar la variable `data-cmp-data-layer` atributo:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Recuerde que ahora se devuelve un objeto de tipo `ComponentData`. Este objeto incluye un método de captador `getJson()` y esto se usa para rellenar la variable `data-cmp-data-layer` atributo.

1. Abra una ventana de terminal. Genere e implemente solo la variable `ui.apps` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Vuelva al explorador y vuelva a abrir la página con el componente Byline añadido: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Abra las herramientas para desarrolladores del explorador e introduzca el siguiente comando en la sección **Consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Debajo de la respuesta en `component` para encontrar la instancia de la variable `byline` componente:

   ![Capa de datos de firma actualizada](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   Debería ver una entrada como la siguiente:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   Observe que ahora hay un `image` dentro del `byline` entrada de componente. Esto tiene mucha más información sobre el recurso en DAM. Observe también que la variable `@type` y el identificador único (en este caso `byline-136073cfcb`) se han rellenado automáticamente, así como el `repo:modifyDate` que indica cuándo se modificó el componente.

## Ejemplos adicionales {#additional-examples}

1. Otro ejemplo de ampliación de la capa de datos se puede ver inspeccionando la variable `ImageList` en la base de código WKND:
   * `ImageList.java` - Interfaz de Java en el `core` módulo.
   * `ImageListImpl.java` - Modelo de Sling en la variable `core` módulo.
   * `image-list.html` - Plantilla HTL en el `ui.apps` módulo.

   >[!NOTE]
   >
   > Es un poco más difícil incluir propiedades personalizadas como `occupation` al usar la variable [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Sin embargo, si se amplía un componente principal que incluye una imagen o una página, la utilidad ahorra mucho tiempo.

   >[!NOTE]
   >
   > Si se crea una capa de datos avanzada para objetos reutilizados a lo largo de una implementación, se recomienda extraer los elementos de la capa de datos en sus propios objetos Java específicos de la capa de datos. Por ejemplo, los componentes principales de comercio tienen interfaces agregadas para `ProductData` y `CategoryData` ya que estos se pueden utilizar en muchos componentes dentro de una implementación de Commerce. Consulte [el código de la repo aem-cif-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) para obtener más información.

## Felicitaciones! {#congratulations}

Solo tiene que explorar varias formas de ampliar y personalizar la capa de datos del cliente de Adobe con componentes AEM.

## Recursos adicionales {#additional-resources}

* [Documentación de capa de datos del cliente de Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Integración de la capa de datos con los componentes principales](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Uso de la capa de datos del cliente de Adobe y la documentación de componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)

---
title: Personalización de la capa de datos del cliente de Adobe AEM con componentes de
description: Obtenga información sobre cómo personalizar la capa de datos del cliente de Adobe AEM con contenido de componentes de personalizados. AEM Aprenda a utilizar las API proporcionadas por los componentes principales de la aplicación para ampliar y personalizar la capa de datos.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
jira: KT-6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
doc-type: Tutorial
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
duration: 452
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1813'
ht-degree: 1%

---

# Personalización de la capa de datos del cliente de Adobe AEM con componentes de {#customize-data-layer}

Obtenga información sobre cómo personalizar la capa de datos del cliente de Adobe AEM con contenido de componentes de personalizados. AEM Aprenda a utilizar las API proporcionadas por [Componentes principales para ampliar ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) y personalizar la capa de datos.

## Lo que va a generar

![Capa de datos de firma](assets/adobe-client-data-layer/byline-data-layer-html.png)

En este tutorial, vamos a explorar varias opciones para ampliar la capa de datos del cliente de Adobe actualizando el componente WKND [Byline](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html). El componente _Byline_ es un **componente personalizado** y las lecciones aprendidas en este tutorial se pueden aplicar a otros componentes personalizados.

### Objetivos {#objective}

1. Inserte datos de componente en la capa de datos ampliando un modelo Sling y un componente HTL
1. Utilice las utilidades de capa de datos del componente principal para reducir el esfuerzo
1. Utilice los atributos de datos del componente principal para vincularlos a eventos de capa de datos existentes

## Requisitos previos {#prerequisites}

Se necesita un **entorno de desarrollo local** para completar este tutorial. Las capturas de pantalla y los vídeos se capturan mediante el SDK de AEM as a Cloud Service que se ejecuta en un macOS. Los comandos y el código son independientes del sistema operativo local a menos que se indique lo contrario.

**¿Es novato en el uso de AEM as a Cloud Service?** Consulte la [siguiente guía para configurar un entorno de desarrollo local con el SDK de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=es).

AEM **Nuevo a la versión 6.5 de la?** Consulte la [siguiente guía para configurar un entorno de desarrollo local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=es).

## Descargar e implementar el sitio de referencia de WKND {#set-up-wknd-site}

Este tutorial amplía el componente Byline en el sitio de referencia de WKND. Clone e instale el código WKND base en su entorno local.

1. AEM Inicie una instancia local de Quickstart **author** de la ejecución de la en [http://localhost:4502](http://localhost:4502).
1. Abra una ventana de terminal y clone el código base de WKND con Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. AEM Implemente la base de código WKND en una instancia local de, como se indica a continuación

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM Para la versión 6.5 y el Service Pack más reciente, agregue el perfil `classic` al comando Maven:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. AEM Abra una nueva ventana del explorador e inicie sesión para acceder a la. Abrir una página de **Magazine** como: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Componente de firma en la página](assets/adobe-client-data-layer/byline-component-onpage.png)

   Debería ver un ejemplo del componente Firma que se ha agregado a la página como parte de un Fragmento de experiencia. Puede ver el fragmento de experiencia en [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Abra las herramientas para desarrolladores e introduzca el siguiente comando en la **consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM Para ver el estado actual de la capa de datos en un sitio de datos, compruebe la respuesta de un sitio de datos en el que se haya realizado una inspección. Debería ver información sobre la página y los componentes individuales.

   ![Respuesta de capa de datos de Adobe](assets/data-layer-state-response.png)

   Observe que el componente Firma no aparece en la capa de datos.

## Actualización del modelo Sling de firma {#sling-model}

Para insertar datos sobre el componente en la capa de datos, primero vamos a actualizar el modelo Sling del componente. A continuación, actualice la interfaz Java™ e implementación del modelo Sling de Byline para que tenga un nuevo método `getData()`. Este método contiene las propiedades que se van a insertar en la capa de datos.

1. Abra el proyecto `aem-guides-wknd` en el IDE que desee. Vaya al módulo `core`.
1. Abra el archivo `Byline.java` en `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Interfaz Java de línea de firma](assets/adobe-client-data-layer/byline-java-interface.png)

1. Añada el método siguiente a la interfaz:

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

1. Abra el archivo `BylineImpl.java` en `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`. Es la implementación de la interfaz `Byline` y se implementa como modelo Sling.

1. Agregue las siguientes instrucciones de importación al principio del archivo:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   Las API `fasterxml.jackson` se utilizan para serializar los datos que se van a exponer como JSON. AEM Los `ComponentUtils` de los componentes principales de la se utilizan para comprobar si la capa de datos está habilitada.

1. Agregar el método no implementado `getData()` a `BylineImple.java`:

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

   En el método anterior, se usa un nuevo `HashMap` para capturar las propiedades que se van a exponer como JSON. Observe que se utilizan métodos existentes como `getName()` y `getOccupations()`. `@type` representa el tipo de recurso único del componente; permite a un cliente identificar fácilmente eventos o déclencheur en función del tipo de componente.

   `ObjectMapper` se usa para serializar las propiedades y devolver una cadena JSON. A continuación, esta cadena JSON se puede insertar en la capa de datos.

1. Abra una ventana de terminal. Genere e implemente solo el módulo `core` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## Actualizar el HTL de firma {#htl}

A continuación, actualice `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=en). HTL (lenguaje de plantilla de HTML) es la plantilla utilizada para procesar el HTML del componente.

AEM Se utiliza un atributo de datos especial `data-cmp-data-layer` en cada componente de la para exponer su capa de datos. JavaScript AEM proporcionado por los componentes principales de la busca este atributo de datos. El valor de este atributo de datos se rellena con la cadena JSON devuelta por el método `getData()` del modelo Byline Sling y se inserta en la capa de datos del cliente de Adobe.

1. Abra el proyecto `aem-guides-wknd` en el IDE. Vaya al módulo `ui.apps`.
1. Abra el archivo `byline.html` en `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![HTML de firma](assets/adobe-client-data-layer/byline-html-template.png)

1. Actualizar `byline.html` para incluir el atributo `data-cmp-data-layer`:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   El valor de `data-cmp-data-layer` se ha establecido en `"${byline.data}"`, donde `byline` es el modelo Sling actualizado anteriormente. `.data` es la notación estándar para llamar a un método Java™ Getter en HTL de `getData()` implementado en el ejercicio anterior.

1. Abra una ventana de terminal. Genere e implemente solo el módulo `ui.apps` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Vuelva al explorador y vuelva a abrir la página con un componente Firma: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Abra las herramientas para desarrolladores e inspeccione el origen de HTML de la página para el componente Firma:

   ![Capa de datos de firma](assets/adobe-client-data-layer/byline-data-layer-html.png)

   Debería ver que `data-cmp-data-layer` se ha rellenado con la cadena JSON del modelo Sling.

1. Abra las herramientas para desarrolladores del explorador e introduzca el siguiente comando en la **consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Vaya debajo de la respuesta en `component` para encontrar la instancia del componente `byline` que se ha agregado a la capa de datos:

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

   Observe que las propiedades expuestas son las mismas agregadas en `HashMap` en el modelo Sling.

## Agregar un evento de clic {#click-event}

La capa de datos del cliente de Adobe está controlada por eventos y uno de los eventos más comunes para almacenar en déclencheur una acción es el evento `cmp:click`. AEM Los componentes principales hacen que sea fácil registrar el componente con la ayuda del elemento de datos: `data-cmp-clickable`.

Los elementos en los que se puede hacer clic suelen ser un botón CTA o un vínculo de navegación. Desafortunadamente, el componente Firma no tiene ninguno de estos, pero vamos a registrarlo de todas formas, ya que esto podría ser común para otros componentes personalizados.

1. Abra el módulo `ui.apps` en su IDE
1. Abra el archivo `byline.html` en `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Actualice `byline.html` para incluir el atributo `data-cmp-clickable` en el elemento **name** de la línea de firma:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Abra un terminal nuevo. Genere e implemente solo el módulo `ui.apps` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Vuelva al explorador y vuelva a abrir la página con el componente Firma agregado: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Para probar el evento, añadiremos manualmente algunas API de JavaScript mediante la consola para desarrolladores. Consulte [Uso de la capa de datos del cliente de Adobe AEM con componentes principales de la](data-layer-overview.md) para ver un vídeo sobre cómo hacerlo.

1. Abra las herramientas para desarrolladores del explorador e introduzca el método siguiente en la **consola**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Este método simple debe controlar el clic del nombre del componente Byline.

1. Escriba el método siguiente en la **consola**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   El método anterior inserta un detector de eventos en la capa de datos para detectar el evento `cmp:click` y llama al `bylineClickHandler`.

   >[!CAUTION]
   >
   > Es importante **no** actualizar el explorador durante este ejercicio; de lo contrario, se perderá la consola JavaScript.

1. En el explorador, con la **Consola** abierta, haga clic en el nombre del autor en el componente Firma:

   ![Se hizo clic en el componente Byline](assets/adobe-client-data-layer/byline-component-clicked.png)

   Debería ver el mensaje de la consola `Byline Clicked!` y el nombre de la línea de firma.

   El evento `cmp:click` es el más fácil de vincular. Para componentes más complejos y para rastrear otros comportamientos, es posible añadir JavaScript personalizado para añadir y registrar nuevos eventos. Un buen ejemplo es el Componente de carrusel, que almacena en déclencheur un evento `cmp:show` cada vez que se cambia una diapositiva. Consulte el [código fuente para obtener más información](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js).

## Uso de la utilidad DataLayerBuilder {#data-layer-builder}

Cuando el modelo Sling se [actualizó](#sling-model) anteriormente en el capítulo, optamos por crear la cadena JSON usando un `HashMap` y configurando cada una de las propiedades manualmente. AEM Este método funciona bien para componentes únicos pequeños, pero para los componentes que amplían los componentes principales esto podría dar como resultado mucho código adicional.

Existe una clase de utilidad, `DataLayerBuilder`, para realizar la mayor parte del trabajo pesado. Esto permite que las implementaciones amplíen únicamente las propiedades que desean. Actualicemos el modelo Sling para utilizar `DataLayerBuilder`.

1. Vuelva al IDE y vaya al módulo `core`.
1. Abra el archivo `Byline.java` en `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. Modifique el método `getData()` para devolver un tipo de `ComponentData`

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

   AEM `ComponentData` es un objeto proporcionado por los componentes principales de la. Da como resultado una cadena JSON, igual que en el ejemplo anterior, pero también realiza mucho trabajo adicional.

1. Abra el archivo `BylineImpl.java` en `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Añada las siguientes instrucciones de importación:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Reemplace el método `getData()` con lo siguiente:

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

   El componente Firma vuelve a utilizar partes del componente principal de imagen para mostrar una imagen que representa al autor. En el fragmento anterior, [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) se usa para ampliar la capa de datos del componente `Image`. Esto rellena previamente el objeto JSON con todos los datos sobre la imagen utilizada. También realiza algunas de las funciones rutinarias, como configurar `@type` y el identificador único del componente. Observe que el método es pequeño.

   La única propiedad extendió `withTitle`, que se reemplazó con el valor de `getName()`.

1. Abra una ventana de terminal. Genere e implemente solo el módulo `core` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Vuelva al IDE y abra el archivo `byline.html` en `ui.apps`
1. Actualice HTL para usar `byline.data.json` para rellenar el atributo `data-cmp-data-layer`:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   Recuerde que ahora se devuelve un objeto de tipo `ComponentData`. Este objeto incluye un método de captador `getJson()` y se usa para rellenar el atributo `data-cmp-data-layer`.

1. Abra una ventana de terminal. Genere e implemente solo el módulo `ui.apps` con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Vuelva al explorador y vuelva a abrir la página con el componente Firma agregado: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Abra las herramientas para desarrolladores del explorador e introduzca el siguiente comando en la **consola**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Vaya debajo de la respuesta en `component` para encontrar la instancia del componente `byline`:

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

   Observe que ahora hay un objeto `image` dentro de la entrada de componente `byline`. Tiene mucha más información sobre el recurso en DAM. Observe también que `@type` y el identificador único (en este caso `byline-136073cfcb`) se han rellenado automáticamente, y el `repo:modifyDate` que indica cuándo se modificó el componente.

## Ejemplos adicionales {#additional-examples}

1. Otro ejemplo de ampliación de la capa de datos se puede ver inspeccionando el componente `ImageList` en la base de código WKND:
   * `ImageList.java`: interfaz Java en el módulo `core`.
   * `ImageListImpl.java`: modelo Sling en el módulo `core`.
   * `image-list.html` - Plantilla HTL en el módulo `ui.apps`.

   >[!NOTE]
   >
   > Es un poco más difícil incluir propiedades personalizadas como `occupation` al usar [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Sin embargo, si amplía un componente principal que incluye una imagen o una página, la utilidad ahorra mucho tiempo.

   >[!NOTE]
   >
   > Si se crea una capa de datos avanzada para objetos reutilizados a lo largo de una implementación, se recomienda extraer los elementos de la capa de datos en sus propios objetos Java™ específicos de la capa de datos. Por ejemplo, los componentes principales de Commerce han agregado interfaces para `ProductData` y `CategoryData`, ya que estas se pueden usar en muchos componentes de una implementación de Commerce. Revise [el código en el repositorio aem-cif-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) para obtener más detalles.

## Enhorabuena. {#congratulations}

Acaba de explorar algunas formas de ampliar y personalizar la capa de datos del cliente de Adobe AEM con componentes de la interfaz de usuario de la interfaz de usuario de la interfaz de usuario de.

## Recursos adicionales {#additional-resources}

* [Documentación de la capa de datos del cliente de Adobe](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Integración de la capa de datos con los componentes principales](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [Uso de la capa de datos del cliente de Adobe y la documentación de componentes principales](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=es)

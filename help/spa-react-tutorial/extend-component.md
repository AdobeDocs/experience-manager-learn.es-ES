---
title: Ampliación de un componente | Introducción al Editor de SPA de AEM y Reacción
description: Obtenga información sobre cómo ampliar un componente principal existente para utilizarlo con el Editor de SPA de AEM. El comprender cómo agregar propiedades y contenido a un componente existente es una técnica eficaz para expandir las capacidades de una implementación del Editor de SPA de AEM. Aprenda a utilizar el patrón de delegación para ampliar los modelos Sling y las características de Sling Resource Merger.
sub-product: sitios
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5879
thumbnail: 5879-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '1969'
ht-degree: 2%

---


# Ampliación de un componente principal {#extend-component}

Obtenga información sobre cómo ampliar un componente principal existente para utilizarlo con el Editor de SPA de AEM. El comprender cómo ampliar un componente existente es una técnica eficaz para personalizar y ampliar las capacidades de una implementación del Editor de SPA de AEM.

## Objetivo

1. Amplíe un componente principal existente con propiedades y contenido adicionales.
2. Comprender los aspectos básicos de la herencia de componentes con el uso de `sling:resourceSuperType`.
3. Obtenga información sobre cómo aprovechar el patrón [de](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) delegación para modelos Sling para reutilizar la lógica y la funcionalidad existentes.

## Qué va a generar

En este capítulo se creará un nuevo `Card` componente. El `Card` componente ampliará el componente [principal de](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/components/image.html) imagen agregando campos de contenido adicionales como Título y un botón Llamada a acción para realizar la función de teaser para otro contenido dentro del SPA.

![Creación final del componente de tarjeta](assets/extend-component/final-authoring-card.png)

>[!NOTE]
>
> En una implementación real puede ser más apropiado simplemente usar el componente [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/teaser.html) Teaser y luego ampliar el componente [principal de](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/components/image.html) imagen para crear un `Card` componente según los requisitos del proyecto. Siempre se recomienda utilizar los componentes [principales](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html) directamente cuando sea posible.

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un entorno [de desarrollo](overview.md#local-dev-environment)local.

### Obtener el código

1. Descargue el punto de partida de este tutorial a través de Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/extend-component-start
   ```

2. Implemente el código base en una instancia de AEM local mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si utiliza [AEM 6.x](overview.md#compatibility) , agregue el `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale el paquete terminado para el sitio [de referencia](https://github.com/adobe/aem-guides-wknd/releases/latest)WKND tradicional. Las imágenes proporcionadas por el sitio [de referencia](https://github.com/adobe/aem-guides-wknd/releases/latest) WKND se reutilizarán en el WKND SPA. El paquete se puede instalar mediante [AEM administrador](http://localhost:4502/crx/packmgr/index.jsp)de paquetes.

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) o desproteger el código localmente cambiando a la rama `React/extend-component-solution`.

## Implementación inicial de Inspect Card

El código de inicio de capítulo ha proporcionado un componente de tarjeta inicial. Inspect es el punto de partida para la implementación de tarjetas.

1. En el IDE de su elección, abra el `ui.apps` módulo.
2. Vaya al archivo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/card` y realice la vista del mismo `.content.xml` .

   ![Inicio de definición de AEM del componente de tarjeta](assets/extend-component/aem-card-cmp-start-definition.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Card"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   La propiedad `sling:resourceSuperType` señala que `wknd-spa-react/components/image` indica que el `Card` componente heredará toda la funcionalidad del componente de imagen WKND SPA.

3. Inspect el archivo `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/image/.content.xml`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Image"
       sling:resourceSuperType="core/wcm/components/image/v2/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Observe que los `sling:resourceSuperType` puntos a `core/wcm/components/image/v2/image`. Esto indica que el componente de imagen WKND SPA hereda toda la funcionalidad de la imagen del componente principal.

   También conocida como la herencia de recursos Sling del patrón [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#proxy-component-pattern) proxy es un potente patrón de diseño que permite a los componentes secundarios heredar la funcionalidad y extender/anular el comportamiento cuando lo desee. La herencia de Sling admite varios niveles de herencia, por lo que en última instancia el nuevo `Card` componente hereda la funcionalidad de la imagen del componente principal.

   Muchos equipos de desarrollo se esfuerzan por ser D.R.Y. (no se repita). La herencia de Sling permite esto con AEM.

4. Debajo de la `card` carpeta, abra el archivo `_cq_dialog/.content.xml`.

   Este archivo es la definición del cuadro de diálogo de componentes del `Card` componente. Si se utiliza la herencia Sling, es posible utilizar las funciones de la fusión [de recursos](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/sling-resource-merger.html) Sling para anular o ampliar partes del cuadro de diálogo. En este ejemplo, se ha agregado una nueva ficha al cuadro de diálogo para capturar datos adicionales de un autor para rellenar el componente de tarjeta.

   Propiedades como `sling:orderBefore` permitir que un desarrollador elija dónde insertar nuevas fichas o campos de formulario. En este caso, la `Text` ficha se insertará antes de la `asset` ficha. Para aprovechar al máximo la fusión de recursos de Sling, es importante conocer la estructura de nodos de cuadro de diálogo original para el cuadro de diálogo [del componente](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_dialog/.content.xml)Imagen.

5. Debajo de la `card` carpeta, abra el archivo `_cq_editConfig.xml`. Este archivo dicta el comportamiento de arrastrar y soltar en la IU de creación de AEM. Al ampliar el componente Imagen, es importante que el tipo de recurso coincida con el propio componente. Revise el `<parameters>` nodo:

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/card"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La mayoría de los componentes no requieren un `cq:editConfig`, la imagen y los descendientes secundarios del componente Imagen son excepciones.

6. En el conmutador IDE al `ui.frontend` módulo, vaya a `ui.frontend/src/components/Card`:

   ![Reaccionar Inicio de componentes](assets/extend-component/react-card-component-start.png)

7. Inspect el archivo `Card.js`.

   El componente ya se ha encontrado para asignarse al componente AEM `Card` mediante la `MapTo` función estándar.

   ```js
   MapTo('wknd-spa-react/components/card')(Card, CardEditConfig);
   ```

8. Inspect el método `get imageContent()`:

   ```js
    get imageContent() {
       return (
           <div className="Card__image">
               <Image {...this.props} />
           </div>)
   }
   ```

   En este ejemplo hemos elegido reutilizar el componente React Image existente `Image` pasando simplemente el `this.props` componente desde el `Card` . Más adelante en el tutorial se implementará el `get bodyContent()` método para mostrar un título, una fecha y un botón de llamada a acción.

## Actualizar la directiva de plantilla

Con esta implementación inicial `Card` revise la funcionalidad en el Editor de SPA de AEM. Para ver el `Card` componente inicial se necesita una actualización de la directiva Plantilla.

1. Implemente el código de inicio en una instancia local de AEM, si aún no lo ha hecho:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Vaya a la plantilla de página de SPA en [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. Actualice la directiva del Contenedor de diseño para agregar el nuevo `Card` componente como un componente permitido:

   ![Actualizar directiva de Contenedor de diseño](assets/extend-component/card-component-allowed.png)

   Guarde los cambios en la directiva y observe el `Card` componente como un componente permitido:

   ![Componente de tarjeta como componente permitido](assets/extend-component/card-component-allowed-layout-container.png)

## Autor del componente de tarjeta inicial

A continuación, cree el `Card` componente con el Editor de SPA de AEM.

1. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
2. En `Edit` modo, agregue el `Card` componente a la `Layout Container`:

   ![Insertar nuevo componente](assets/extend-component/insert-card-component.png)

3. Drag and drop an image from the Asset finder onto the `Card` component:

   ![Añadir imagen](assets/extend-component/card-add-image.png)

4. Abra el cuadro de diálogo del `Card` componente y observe la adición de una ficha de **texto** .
5. Introduzca los siguientes valores en la ficha **Texto** :

   ![Ficha Componente de texto](assets/extend-component/card-component-text.png)

   **Ruta** de la tarjeta: elija una página debajo de la página principal de SPA.

   **Texto** de llamada a acción: &quot;Más información&quot;

   **Título** de la tarjeta: dejar en blanco

   **Obtener título de una página** vinculada: marque la casilla de verificación para indicar verdadero.

6. Actualice la ficha Metadatos **del** recurso para agregar valores para Texto **** alternativo y **Rótulo**.

   Actualmente no aparecen cambios adicionales después de actualizar el cuadro de diálogo. Para exponer los nuevos campos al componente React, debemos actualizar el modelo de Sling para el `Card` componente.

7. Abra una nueva ficha y vaya a [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd-spa-react/us/en/home/jcr%3Acontent/root/responsivegrid/card). Inspect los nodos de contenido debajo `/content/wknd-spa-react/us/en/home/jcr:content/root/responsivegrid` para buscar el contenido del `Card` componente.

   ![Propiedades del componente CRXDE-Lite](assets/extend-component/crxde-lite-properties.png)

   Observe que las propiedades `cardPath`, `ctaText`, `titleFromPage` son persistentes en el cuadro de diálogo.

## Actualizar modelo Sling de tarjeta

Para exponer finalmente los valores del cuadro de diálogo del componente al componente Reaccionar, debemos actualizar el modelo Sling que rellena el JSON para el `Card` componente. También tenemos la oportunidad de implementar dos elementos de lógica empresarial:

* Si `titleFromPage` es **true**, devuelve el título de la página especificada; de lo contrario, `cardPath` devuelve el valor de `cardTitle` textfield.
* Devuelve la última fecha de modificación de la página especificada por `cardPath`.

Vuelva al IDE de su elección y abra el `core` módulo.

1. Open the file `Card.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/Card.java`.

   Observe que la `Card` interfaz actualmente se extiende `com.adobe.cq.wcm.core.components.models.Image` y por lo tanto hereda todos los métodos de la `Image` interfaz. La `Image` interfaz ya amplía la `ComponentExporter` interfaz, lo que permite exportar el modelo Sling como JSON y asignarlo al editor de SPA. Por lo tanto, no es necesario ampliar explícitamente `ComponentExporter` la interfaz como lo hicimos en el capítulo [Componente](custom-component.md)personalizado.

2. Añada los siguientes métodos a la interfaz:

   ```java
   @ProviderType
   public interface Card extends Image {
   
       /***
       * The URL to populate the CTA button as part of the card.
       * The link should be based on the cardPath property that points to a page.
       * @return String URL
       */
       public String getCtaLinkURL();
   
       /***
       * The text to display on the CTA button of the card.
       * @return String CTA text
       */
       public String getCtaText();
   
   
   
       /***
       * The date to be displayed as part of the card.
       * This is based on the last modified date of the page specified by the cardPath
       * @return
       */
       public Calendar getCardLastModified();
   
   
       /**
       * Return the title of the page specified by cardPath if `titleFromPage` is set to true.
       * Otherwise return the value of `cardTitle`
       * @return
       */
       public String getCardTitle();
   }
   ```

   Estos métodos se exponen mediante la API de modelo JSON y se pasan al componente React.

3. Abra `CardImpl.java`. Esta es la implementación de la `Card.java` interfaz. Esta implementación ya se ha estropeado parcialmente para acelerar el tutorial.  Observe el uso de las anotaciones `@Model` y `@Exporter` para garantizar que el modelo Sling pueda serializarse como JSON a través del exportador del modelo Sling.

   `CardImpl.java` también utiliza el patrón [Delegación para los modelos](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Sling para evitar reescribir toda la lógica del componente principal Imagen.

4. Observe las líneas siguientes:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   La anotación anterior creará una instancia de un objeto Image denominado `image` en función de la `sling:resourceSuperType` herencia del `Card` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Entonces es posible simplemente utilizar el `image` objeto para implementar los métodos definidos por la `Image` interfaz, sin tener que escribir la lógica nosotros mismos. Esta técnica se utiliza para `getSrc()`, `getAlt()` y `getTitle()`.

5. A continuación, implemente el `initModel()` método para iniciar una variable privada `cardPage` en función del valor de `cardPath`

   ```java
   @PostConstruct
   public void initModel() {
       if(StringUtils.isNotBlank(cardPath) && pageManager != null) {
           cardPage = pageManager.getPage(this.cardPath);
       }
   }
   ```

   Siempre `@PostConstruct initModel()` se llamará al modelo Sling cuando se inicialice, por lo tanto es una buena oportunidad para inicializar objetos que puedan ser utilizados por otros métodos del modelo. El `pageManager` es uno de varios objetos [globales con respaldo de](https://docs.adobe.com/content/help/en/experience-manager-htl/using/htl/global-objects.html#java-backed-objects) Java disponibles para los modelos de Sling mediante la `@ScriptVariable` anotación. El método [getPage](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/PageManager.html#getPage-java.lang.String-) toma una ruta y devuelve un objeto [Page](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/ref/javadoc/com/day/cq/wcm/api/Page.html) AEM o nulo si la ruta no apunta a una página válida.

   Esto inicializará la `cardPage` variable, que será utilizada por los otros nuevos métodos para devolver datos sobre la página vinculada subyacente.

6. Revise las variables globales ya asignadas a las propiedades JCR guardadas en el cuadro de diálogo de creación. La `@ValueMapValue` anotación se utiliza para realizar automáticamente la asignación.

   ```java
   @ValueMapValue
   private String cardPath;
   
   @ValueMapValue
   private String ctaText;
   
   @ValueMapValue
   private boolean titleFromPage;
   
   @ValueMapValue
   private String cardTitle;
   ```

   Estas variables se utilizarán para implementar los métodos adicionales para la `Card.java` interfaz.

7. Implementar los métodos adicionales definidos en la `Card.java` interfaz:

   ```java
   @Override
   public String getCtaLinkURL() {
       if(cardPage != null) {
           return cardPage.getPath() + ".html";
       }
       return null;
   }
   
   @Override
   public String getCtaText() {
       return ctaText;
   }
   
   @Override
   public Calendar getCardLastModified() {
      if(cardPage != null) {
          return cardPage.getLastModified();
      }
      return null;
   }
   
   @Override
   public String getCardTitle() {
       if(titleFromPage) {
           return cardPage != null ? cardPage.getTitle() : null;
       }
       return cardTitle;
   }
   ```

   >[!NOTE]
   >
   > Puede realizar la vista de [CardImpl.java aquí](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CardImpl.java).

8. Abra una ventana de terminal e implemente solo las actualizaciones del `core` módulo mediante el perfil Maven `autoInstallBundle` desde el `core` directorio.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

   Si utiliza [AEM 6.x](overview.md#compatibility) , agregue el `classic` perfil.

9. Vista de la respuesta del modelo JSON en: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) y busque la `wknd-spa-react/components/card`:

   ```json
   "card": {
       "ctaText": "Read More",
       "cardTitle": "Page 1",
       "title": "Woman chillaxing with river views in Australian bushland",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/card.coreimg.jpeg/1595190732886/adobestock-216674449.jpeg",
       "alt": "Female sitting on a large rock relaxing in afternoon dappled light the Australian bushland with views over the river",
       "cardLastModified": 1591360492414,
       "ctaLinkURL": "/content/wknd-spa-react/us/en/home/page-1.html",
       ":type": "wknd-spa-react/components/card"
   }
   ```

   Observe que el modelo JSON se actualiza con pares de clave/valor adicionales después de actualizar los métodos en el modelo `CardImpl` Sling.

## Actualizar componente de reacción

Ahora que el modelo JSON se rellena con nuevas propiedades para `ctaLinkURL`, `ctaText``cardTitle` y `cardLastModified` podemos actualizar el componente React para que se muestren.

1. Vuelva al IDE y abra el `ui.frontend` módulo. Opcionalmente, puede realizar un inicio en el servidor de desarrollo de webpack desde una nueva ventana de terminal para ver los cambios en tiempo real:

   ```shell
   $ cd ui.frontend
   $ npm install
   $ npm start
   ```

2. Abrir `Card.js` en `ui.frontend/src/components/Card/Card.js`.
3. Añada el método `get ctaButton()` para procesar la llamada a acción:

   ```js
   import {Link} from "react-router-dom";
   ...
   
   export default class Card extends Component {
   
       get ctaButton() {
           if(this.props && this.props.ctaLinkURL && this.props.ctaText) {
               return (
                   <div className="Card__action-container">
                       <Link to={this.props.ctaLinkURL} title={this.props.title}
                           className="Card__action-link">
                           {this.props.ctaText}
                       </Link>
                   </div>
               );
           }
   
           return null;
       }
       ...
   }
   ```

4. Añada un método para `get lastModifiedDisplayDate()` transformar `this.props.cardLastModified` en una cadena localizada que represente la fecha.

   ```js
   export default class Card extends Component {
       ...
       get lastModifiedDisplayDate() {
           const lastModifiedDate = this.props.cardLastModified ? new Date(this.props.cardLastModified) : null;
   
           if (lastModifiedDate) {
               return lastModifiedDate.toLocaleDateString();
           }
           return null;
       }
       ...
   }
   ```

5. Actualice el `get bodyContent()` para mostrar `this.props.cardTitle` y utilizar los métodos creados en los pasos anteriores:

   ```js
   export default class Card extends Component {
       ...
       get bodyContent() {
          return (<div class="Card__content">
                       <h2 class="Card__title"> {this.props.cardTitle}
                           <span class="Card__lastmod">
                               {this.lastModifiedDisplayDate}
                           </span>
                       </h2>
                       {this.ctaButton}
               </div>);
       }
       ...
   }
   ```

6. Ya se han agregado reglas de clasificación en `Card.scss` para aplicar estilo al título, la llamada a acción y la fecha de la última modificación. Para incluir estos estilos, agregue la línea siguiente a `Card.js` la parte superior del archivo:

   ```diff
     import {MapTo} from '@adobe/aem-react-editable-components';
   
   + require('./Card.scss');
   
     export const CardEditConfig = {
   ```

   >[!NOTE]
   >
   > Puede realizar la vista del código del componente de la tarjeta [React aquí](https://github.com/adobe/aem-guides-wknd-spa/blob/React/extend-component-solution/ui.frontend/src/components/Card/Card.js).

7. Implemente los cambios completos en AEM desde la raíz del proyecto mediante Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

8. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html) para ver el componente actualizado:

   ![Componente de tarjeta actualizado en AEM](assets/extend-component/updated-card-in-aem.png)

9. Debería poder volver a crear el contenido existente para crear una página similar a la siguiente:

   ![Creación final del componente de tarjeta](assets/extend-component/final-authoring-card.png)

## Felicitaciones! {#congratulations}

Enhorabuena, ha aprendido a ampliar un componente de AEM mediante el modelo JSON y cómo funcionan los modelos de Sling y los diálogos.

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/extend-component-solution) o desproteger el código localmente cambiando a la rama `React/extend-component-solution`.

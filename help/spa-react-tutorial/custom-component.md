---
title: Crear un componente personalizado | Introducción al Editor de SPA de AEM y Reacción
description: Obtenga información sobre cómo crear un componente personalizado para utilizarlo con el Editor de SPA de AEM. Obtenga información sobre cómo desarrollar diálogos de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado.
sub-product: sitios
feature: SPA Editor
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5878
thumbnail: 5878-spa-react.jpg
translation-type: tm+mt
source-git-commit: 52748ff530e98c4ec21b84250bd73543899db4e4
workflow-type: tm+mt
source-wordcount: '1486'
ht-degree: 1%

---


# Crear un componente personalizado {#custom-component}

Obtenga información sobre cómo crear un componente personalizado para utilizarlo con el Editor de SPA de AEM. Obtenga información sobre cómo desarrollar diálogos de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado.

## Objetivo

1. Comprenda la función de los modelos Sling en la manipulación de la API de modelo JSON proporcionada por AEM.
2. Obtenga información sobre cómo crear nuevos cuadros de diálogo de componentes de AEM.
3. Aprenda a crear un componente de AEM **personalizado** que sea compatible con el marco del editor de SPA.

## Qué va a generar

Los capítulos anteriores se centraban en la elaboración de componentes de la SPA y su asignación a los componentes básicos *existentes* AEM. Este capítulo se centra en cómo crear y ampliar *nuevos* componentes de AEM y manipular el modelo JSON ofrecido por AEM.

Un `Custom Component` ejemplo sencillo ilustra los pasos necesarios para crear un componente de AEM neto-nuevo.

![Mensaje mostrado en Todas las mayúsculas](assets/custom-component/message-displayed.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un entorno [de desarrollo](overview.md#local-dev-environment)local.

### Obtener el código

1. Descargue el punto de partida de este tutorial a través de Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/custom-component-start
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

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) o desproteger el código localmente cambiando a la rama `React/custom-component-solution`.

## Definir el componente AEM

Un componente AEM se define como un nodo y propiedades. En el proyecto, estos nodos y propiedades se representan como archivos XML en el `ui.apps` módulo. A continuación, cree el componente AEM en el `ui.apps` módulo.

>[!NOTE]
>
> Una rápida actualización de los [conceptos básicos de AEM componentes puede resultar útil](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html).

1. En el IDE de su elección, abra la `ui.apps` carpeta.
2. Vaya a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` y cree una nueva carpeta denominada `custom-component`.
3. Create a new file named `.content.xml` beneath the `custom-component` folder. Rellene el `custom-component/.content.xml` formulario con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Crear definición de componente personalizado](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identifica que este nodo será un componente AEM.

   `jcr:title` es el valor que se mostrará a los autores de contenido y `componentGroup` determina la agrupación de componentes en la interfaz de usuario de creación.

4. Debajo de la `custom-component` carpeta, cree otra carpeta llamada `_cq_dialog`.
5. Debajo de la `_cq_dialog` carpeta, cree un nuevo archivo con el nombre `.content.xml` y rellénelo con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   ![Definición del componente personalizado](assets/custom-component/dialog-custom-component-defintion.png)

   El archivo XML anterior genera un cuadro de diálogo muy sencillo para el `Custom Component`. La parte crítica del archivo es el `<message>` nodo interior. Este cuadro de diálogo contendrá un `textfield` nombre sencillo `Message` y persistirá el valor del campo de texto en una propiedad denominada `message`.

   Se creará un modelo Sling junto a la exposición del valor de la `message` propiedad mediante el modelo JSON.

   >[!NOTE]
   >
   > Puede vista de muchos más [ejemplos de diálogos consultando las definiciones](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)de los componentes principales. También puede vista campos de formulario adicionales, como `select`, `textarea``pathfield`, disponibles `/libs/granite/ui/components/coral/foundation/form` en [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Con un componente AEM tradicional, se suele requerir un script [HTL](https://docs.adobe.com/content/help/es-ES/experience-manager-htl/using/overview.html) . Dado que SPA procesará el componente, no se necesita ningún script HTL.

## Creación del modelo Sling

Los modelos de Sling son Java &quot;POJO&quot; (objetos Java antiguos sin formato) impulsados por anotaciones que facilitan la asignación de datos de las variables JCR a Java. [Los modelos](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) Sling suelen funcionar para encapsular la compleja lógica empresarial del lado del servidor para AEM componentes.

En el contexto del Editor de SPA, los modelos de Sling exponen el contenido de un componente a través del modelo JSON mediante una función que utiliza el Exportador [del modelo de](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)Sling.

1. En el IDE de su elección, abra el `core` módulo. `CustomComponent.java` y ya `CustomComponentImpl.java` se han creado y se han encontrado como parte del código de inicio de capítulo.

   >[!NOTE]
   >
   > Si utiliza Visual Studio Code IDE, puede resultar útil instalar [extensiones para Java](https://code.visualstudio.com/docs/java/extensions).

2. Abra la interfaz de Java `CustomComponent.java` en `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/CustomComponent.java`:

   ![Interfaz CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Esta es la interfaz de Java que implementará el Modelo Sling.

3. Actualice `CustomComponent.java` para que se extienda la `ComponentExporter` interfaz:

   ```java
   package com.adobe.aem.guides.wknd.spa.react.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   La implementación de la `ComponentExporter` interfaz es un requisito para que el modelo Sling sea recogido automáticamente por la API del modelo JSON.

   La `CustomComponent` interfaz incluye un único método de captador `getMessage()`. Este es el método que muestra el valor del cuadro de diálogo de creación a través del modelo JSON. En el modelo JSON solo `()` se exportarán métodos de captador públicos con parámetros vacíos.

4. Abrir `CustomComponentImpl.java` en `core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java`.

   Esta es la implementación de la `CustomComponent` interfaz. La `@Model` anotación identifica la clase Java como modelo Sling. La `@Exporter` anotación permite serializar y exportar la clase Java a través del exportador del modelo Sling.

5. Actualice la variable estática `RESOURCE_TYPE` para que señale al componente de AEM `wknd-spa-react/components/custom-component` creado en el ejercicio anterior.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-react/components/custom-component";
   ```

   El tipo de recurso del componente es lo que enlazará el modelo Sling con el componente AEM y, en última instancia, se asignará al componente React.

6. Añada el `getExportedType()` método a la `CustomComponentImpl` clase para devolver el tipo de recurso de componente:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Este método es necesario al implementar la `ComponentExporter` interfaz y expone el tipo de recurso que permite la asignación al componente React.

7. Actualice el `getMessage()` método para devolver el valor de la propiedad que `message` persiste en el cuadro de diálogo de creación. Utilice la `@ValueMap` anotación es asignar el valor JCR `message` a una variable Java:

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   Se agrega alguna &quot;lógica empresarial&quot; adicional para devolver el valor String del mensaje con todas las letras mayúsculas. Esto nos permitirá ver la diferencia entre el valor sin procesar almacenado por el cuadro de diálogo de creación y el valor expuesto por el modelo de Sling.

   >[!NOTE]
   >
   > Puede realizar la vista de CustomComponentImpl.java [finalizada aquí](https://github.com/adobe/aem-guides-wknd-spa/blob/React/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/react/core/models/impl/CustomComponentImpl.java).

## Actualizar el componente React

Ya se ha creado el código de Reacción para el componente personalizado. A continuación, realice algunas actualizaciones para asignar el componente React al componente AEM.

1. En el `ui.frontend` módulo, abra el archivo `ui.frontend/src/components/Custom/Custom.js`.
2. Observe la `{this.props.message}` variable como parte del `render()` método:

   ```js
   return (
           <div class="CustomComponent">
               <h2 class="CustomComponent__message">{this.props.message}</h2>
           </div>
       );
   ```

   Se espera que el valor en mayúsculas transformado del modelo de Sling se asigne a esta `message` propiedad.

3. Importe el `MapTo` objeto desde el SDK de JS del Editor de SPA de AEM y úselo para asignarlo al componente AEM:

   ```diff
   + import {MapTo} from '@adobe/aem-react-editable-components';
   
    ...
    export default class Custom extends Component {
        ...
    }
   
   + MapTo('wknd-spa-react/components/custom-component')(Custom, CustomEditConfig);
   ```

4. Implemente todas las actualizaciones en un entorno de AEM local desde la raíz del directorio del proyecto, utilizando sus conocimientos Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Actualizar la directiva de plantilla

A continuación, vaya a AEM para comprobar las actualizaciones y permitir que se `Custom Component` agreguen al SPA.

1. Compruebe el registro del nuevo modelo de Sling navegando a [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl - wknd-spa-react/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.react.core.models.impl.CustomComponentImpl exports 'wknd-spa-react/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Debe ver las dos líneas anteriores que indican que el `CustomComponentImpl` componente está asociado al `wknd-spa-react/components/custom-component` componente y que está registrado a través del exportador del modelo de Sling.

2. Vaya a la plantilla de página de SPA en [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
3. Actualice la directiva del Contenedor de diseño para agregar el nuevo `Custom Component` como componente permitido:

   ![Actualizar directiva de Contenedor de diseño](assets/custom-component/custom-component-allowed.png)

   Guarde los cambios en la directiva y observe el `Custom Component` componente como permitido:

   ![Componente personalizado como componente permitido](assets/custom-component/custom-component-allowed-layout-container.png)

## Creación del componente personalizado

A continuación, cree el `Custom Component` con el Editor de SPA de AEM.

1. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
2. En `Edit` modo, agregue el `Custom Component` a la `Layout Container`:

   ![Insertar nuevo componente](assets/custom-component/insert-custom-component.png)

3. Abra el cuadro de diálogo del componente e introduzca un mensaje que contenga algunas letras minúsculas.

   ![Configuración del componente personalizado](assets/custom-component/enter-dialog-message.png)

   Es el cuadro de diálogo que se creó en función del archivo XML anteriormente en el capítulo.

4. Guarde los cambios. Observe que el mensaje mostrado está en todas las mayúsculas.

   ![Mensaje mostrado en Todas las mayúsculas](assets/custom-component/message-displayed.png)

5. Para vista del modelo JSON, vaya a [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Buscar `wknd-spa-react/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-react/components/custom-component"
   }
   ```

   Tenga en cuenta que el valor JSON se establece en todas las letras mayúsculas según la lógica agregada al modelo Sling.

## Felicitaciones! {#congratulations}

Enhorabuena, ha aprendido a crear un componente de AEM personalizado para utilizarlo con el Editor de SPA. También ha aprendido cómo interactúan los diálogos, las propiedades JCR y los modelos Sling para obtener el modelo JSON.

Puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/custom-component-solution) o extraer el código localmente cambiando a la rama `React/custom-component-solution`.

### Próximos pasos {#next-steps}

[Ampliar un componente](extend-component.md) principal: Conozca cómo ampliar un componente principal existente para utilizarlo con el Editor de SPA de AEM. El comprender cómo agregar propiedades y contenido a un componente existente es una técnica eficaz para expandir las capacidades de una implementación del Editor de SPA de AEM.

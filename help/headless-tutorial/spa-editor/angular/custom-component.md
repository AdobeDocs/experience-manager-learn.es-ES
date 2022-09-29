---
title: Crear un componente personalizado | Introducción al Editor de SPA de AEM y al Angular
description: Obtenga información sobre cómo crear un componente personalizado para utilizarlo con el AEM SPA Editor. Aprenda a desarrollar cuadros de diálogo de autor y modelos de Sling para ampliar el modelo JSON y rellenar un componente personalizado.
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 3%

---

# Crear un componente personalizado {#custom-component}

Obtenga información sobre cómo crear un componente personalizado para utilizarlo con el AEM SPA Editor. Aprenda a desarrollar cuadros de diálogo de autor y modelos de Sling para ampliar el modelo JSON y rellenar un componente personalizado.

## Objetivo

1. Comprenda el papel de los modelos Sling en la manipulación de la API del modelo JSON proporcionada por AEM.
2. Obtenga información sobre cómo crear cuadros de diálogo de componentes de AEM.
3. Aprenda a crear un **custom** AEM componente compatible con el marco del editor de SPA.

## Qué va a generar

El objetivo de los capítulos anteriores era elaborar SPA componentes y asignarlos a *existente* AEM componentes principales. Este capítulo se centra en cómo crear y ampliar *new* AEM componentes y manipule el modelo JSON servido por AEM.

Una sencilla `Custom Component` ilustra los pasos necesarios para crear un componente de AEM nuevo.

![Mensaje mostrado en Todas las mayúsculas](assets/custom-component/message-displayed.png)

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtención del código

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Implemente el código base en una instancia de AEM local mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si usa [AEM 6.x](overview.md#compatibility) añada la variable `classic` perfil:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale el paquete terminado para el [Sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Las imágenes proporcionadas por [Sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) se reutilizan en la SPA WKND. El paquete se puede instalar mediante [Administrador de paquetes AEM](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager instalar wknd.all](./assets/map-components/package-manager-wknd-all.png)

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o extraer el código localmente cambiando a la rama `Angular/custom-component-solution`.

## Definir el componente AEM

Un componente AEM se define como un nodo y propiedades. En el proyecto, estos nodos y propiedades se representan como archivos XML en la variable `ui.apps` módulo. A continuación, cree el componente AEM en la `ui.apps` módulo.

>[!NOTE]
>
> Un rápido repaso sobre el [los conceptos básicos de AEM componentes pueden resultar útiles](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Abra el `ui.apps` en el IDE de su elección.
2. Vaya a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` y crear una carpeta con el nombre `custom-component`.
3. Crear un archivo con el nombre `.content.xml` debajo del `custom-component` carpeta. Rellene el `custom-component/.content.xml` con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Crear definición de componente personalizado](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` : identifica que este nodo es un componente AEM.

   `jcr:title` es el valor que se muestra a los autores de contenido y la variable `componentGroup` determina la agrupación de componentes en la IU de creación.

4. Debajo de la variable `custom-component` carpeta, cree otra carpeta denominada `_cq_dialog`.
5. Debajo de la variable `_cq_dialog` carpeta crear un archivo con el nombre `.content.xml` y rellénelo de la siguiente manera:

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

   ![Definición de componente personalizado](assets/custom-component/dialog-custom-component-defintion.png)

   El archivo XML anterior genera un cuadro de diálogo sencillo para la variable `Custom Component`. La parte crítica del archivo es el `<message>` nodo . Este cuadro de diálogo contiene `textfield` named `Message` y conservan el valor del campo de texto en una propiedad denominada `message`.

   Se crea un modelo de Sling junto a para exponer el valor de la variable `message` a través del modelo JSON.

   >[!NOTE]
   >
   > Puede ver mucho más [ejemplos de cuadros de diálogo consultando las definiciones de los componentes principales](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). También puede ver campos de formulario adicionales, como `select`, `textarea`, `pathfield`, disponible debajo de `/libs/granite/ui/components/coral/foundation/form` en [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Con un componente AEM tradicional, una [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) normalmente se requiere una secuencia de comandos. Dado que la SPA procesa el componente, no se necesita ningún script HTL.

## Creación del modelo Sling

Los modelos Sling son &quot;POJO&quot; Java™ impulsados por anotaciones (objetos Java™ antiguos comunes) que facilitan la asignación de datos de las variables JCR a Java™. [Modelos de Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) normalmente funcionan para encapsular una lógica empresarial compleja del lado del servidor para AEM componentes.

En el contexto del Editor de SPA, los modelos de Sling exponen el contenido de un componente a través del modelo JSON a través de una función que utiliza el [Exportador de modelo Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=es).

1. En el IDE de su elección, abra el `core` módulo. `CustomComponent.java` y `CustomComponentImpl.java` ya se han creado y rellenado como parte del código de inicio de capítulo.

   >[!NOTE]
   >
   > Si utiliza Visual Studio Code IDE, puede ser útil instalar [extensiones para Java™](https://code.visualstudio.com/docs/java/extensions).

2. Abra la interfaz de Java™ `CustomComponent.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![Interfaz CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Esta es la interfaz de Java™ implementada por el modelo Sling.

3. Actualizar `CustomComponent.java` para que se extienda el `ComponentExporter` interfaz:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   Implementación de `ComponentExporter` es un requisito para que la API del modelo JSON recopile automáticamente el modelo Sling.

   La variable `CustomComponent` la interfaz incluye un único método de captador `getMessage()`. Este es el método que expone el valor del cuadro de diálogo de autor a través del modelo JSON. Solo métodos get con parámetros vacíos `()` se exportan en el modelo JSON.

4. Apertura `CustomComponentImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Esta es la implementación de la variable `CustomComponent` interfaz. La variable `@Model` la anotación identifica la clase Java™ como modelo de Sling. La variable `@Exporter` la anotación permite serializar y exportar la clase Java™ a través del exportador del modelo Sling.

5. Actualizar la variable estática `RESOURCE_TYPE` para señalar al componente AEM `wknd-spa-angular/components/custom-component` creada en el ejercicio anterior.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   El tipo de recurso del componente es el que enlaza el modelo Sling al componente AEM y, en última instancia, se asigna al componente Angular.

6. Agregue la variable `getExportedType()` para `CustomComponentImpl` para devolver el tipo de recurso de componente:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Este método es necesario al implementar la variable `ComponentExporter` y expone el tipo de recurso que permite la asignación al componente de Angular.

7. Actualice el `getMessage()` método para devolver el valor de la variable `message` propiedad que persiste en el cuadro de diálogo de autor. Utilice la variable `@ValueMap` la anotación está asignada al valor JCR `message` a una variable de Java™:

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

   Se agrega algo de &quot;lógica empresarial&quot; adicional para devolver el valor del mensaje en mayúsculas. Esto nos permite ver la diferencia entre el valor sin procesar almacenado por el cuadro de diálogo de creación y el valor expuesto por el modelo Sling.

   >[!NOTE]
   Puede ver la [ha finalizado CustomComponentImpl.java aquí](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Actualizar el componente Angular

El código de Angular para el componente personalizado ya se ha creado. A continuación, realice algunas actualizaciones para asignar el componente de Angular al componente de AEM.

1. En el `ui.frontend` abra el archivo `ui.frontend/src/app/components/custom/custom.component.ts`
2. Observe el `@Input() message: string;` línea. Se espera que el valor en mayúsculas transformado se asigne a esta variable.
3. Importe el `MapTo` del SDK de JS del Editor SPA de AEM y utilícelo para asignarlo al componente AEM:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Apertura `cutom.component.html` y observe que el valor de `{{message}}` se muestra en el lado de un `<h2>` etiqueta.
5. Apertura `custom.component.css` y agregue la siguiente regla:

   ```css
   :host-context {
       display: block;
   }
   ```

   Para que el marcador de posición del Editor de AEM se muestre correctamente cuando el componente está vacío, la variable `:host-context` u otro `<div>` necesita configurarse como `display: block;`.

6. Implemente las actualizaciones en un entorno de AEM local desde la raíz del directorio del proyecto, con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Actualizar la directiva de plantilla

A continuación, vaya a AEM para comprobar las actualizaciones y permitir que `Custom Component` para agregarlo al SPA.

1. Verifique el registro del nuevo modelo Sling navegando hasta [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Debería ver las dos líneas anteriores que indican la variable `CustomComponentImpl` está asociado con la variable `wknd-spa-angular/components/custom-component` y que está registrado a través del Sling Model Exporter.

2. Vaya a la plantilla de página SPA en [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Actualice la política del contenedor de diseño para agregar el nuevo `Custom Component` como componente permitido:

   ![Actualizar directiva de contenedor de diseño](assets/custom-component/custom-component-allowed.png)

   Guarde los cambios en la directiva y observe la `Custom Component` como componente permitido:

   ![Componente personalizado como componente permitido](assets/custom-component/custom-component-allowed-layout-container.png)

## Creación del componente personalizado

A continuación, cree el `Custom Component` uso del AEM SPA Editor.

1. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. En `Edit` , añada la variable `Custom Component` a `Layout Container`:

   ![Insertar nuevo componente](assets/custom-component/insert-custom-component.png)

3. Abra el cuadro de diálogo del componente e introduzca un mensaje que contenga letras minúsculas.

   ![Configurar el componente personalizado](assets/custom-component/enter-dialog-message.png)

   Este es el cuadro de diálogo que se creó en función del archivo XML anteriormente en el capítulo.

4. Guarde los cambios. Observe que el mensaje mostrado está en mayúsculas.

   ![Mensaje mostrado en Todas las mayúsculas](assets/custom-component/message-displayed.png)

5. Para ver el modelo JSON, vaya a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Buscar `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Observe que el valor JSON se establece en todas las mayúsculas basándose en la lógica añadida al modelo Sling.

## Felicitaciones! {#congratulations}

Felicitaciones, ha aprendido a crear un componente de AEM personalizado y a cómo funcionan los modelos y diálogos de Sling con el modelo JSON.

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o extraer el código localmente cambiando a la rama `Angular/custom-component-solution`.

### Siguientes pasos {#next-steps}

[Ampliar un componente principal](extend-component.md) - Obtenga información sobre cómo ampliar un componente principal existente para utilizarlo con el AEM SPA Editor. Comprender cómo añadir propiedades y contenido a un componente existente es una técnica eficaz para expandir las capacidades de una implementación AEM Editor SPA.

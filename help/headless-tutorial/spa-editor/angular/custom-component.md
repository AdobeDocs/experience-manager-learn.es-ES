---
title: Crear un componente personalizado | AEM SPA Introducción al Editor y al Angular de la
description: AEM SPA Obtenga información sobre cómo crear un componente personalizado para utilizarlo con el Editor de. Aprenda a desarrollar cuadros de diálogo de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado.
feature: SPA Editor
version: Cloud Service
jira: KT-5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
duration: 308
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# Crear un componente personalizado {#custom-component}

AEM SPA Obtenga información sobre cómo crear un componente personalizado para utilizarlo con el Editor de. Aprenda a desarrollar cuadros de diálogo de autor y modelos Sling para ampliar el modelo JSON y rellenar un componente personalizado.

## Objetivo

1. AEM Comprenda el papel de los modelos Sling en la manipulación de la API del modelo JSON proporcionada por los usuarios de la interfaz de usuario de.
2. AEM Obtenga información sobre cómo crear cuadros de diálogo de componentes de.
3. AEM SPA Aprenda a crear un componente **personalizado** de la aplicación que sea compatible con el marco de trabajo del editor de.

## Qué va a generar

SPA AEM El enfoque de los capítulos anteriores era el desarrollo de componentes de y su asignación a *componentes principales existentes*. AEM AEM Este capítulo se centra en cómo crear y ampliar *nuevos* componentes de y en cómo manipular el modelo JSON proporcionado por los usuarios de la aplicación de código de tiempo de la aplicación de código de tiempo de la aplicación de código de tiempo.

AEM Un `Custom Component` simple ilustra los pasos necesarios para crear un nuevo componente de red de la red de la red (net-new) de la red de la red de la red.

![Mensaje mostrado en Todo en mayúsculas](assets/custom-component/message-displayed.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Obtener el código

1. Descargue el punto de partida para este tutorial mediante Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. AEM Implemente el código base en una instancia de local mediante Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM Si usa [6.x](overview.md#compatibility), agregue el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Instale el paquete terminado para el [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) tradicional. SPA Las imágenes proporcionadas por el [sitio de referencia WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) se reutilizarán en el WKND de la. AEM El paquete se puede instalar usando el [Administrador de paquetes de](http://localhost:4502/crx/packmgr/index.jsp).

   ![Administrador de paquetes instala wknd.all](./assets/map-components/package-manager-wknd-all.png)

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o desprotegerlo localmente cambiando a la rama `Angular/custom-component-solution`.

## AEM Definición del componente de

AEM Un componente se define como un nodo y propiedades. En el proyecto, estos nodos y propiedades se representan como archivos XML en el módulo `ui.apps`. AEM A continuación, cree el componente de en el módulo `ui.apps`.

>[!NOTE]
>
> AEM Un repaso rápido de los [conceptos básicos de los componentes de la puede ser útil](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Abra la carpeta `ui.apps` en el IDE que desee.
2. Vaya a `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` y cree una carpeta llamada `custom-component`.
3. Cree un archivo con el nombre `.content.xml` debajo de la carpeta `custom-component`. Rellene `custom-component/.content.xml` con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Crear definición de componente personalizado](assets/custom-component/aem-custom-component-definition.png)

   AEM `jcr:primaryType="cq:Component"`: identifica que este nodo es un componente de la.

   `jcr:title` es el valor que se muestra a los autores de contenido y `componentGroup` determina la agrupación de componentes en la interfaz de usuario de creación.

4. Debajo de la carpeta `custom-component`, cree otra carpeta denominada `_cq_dialog`.
5. Debajo de la carpeta `_cq_dialog` cree un archivo denominado `.content.xml` y rellénelo con lo siguiente:

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

   El archivo XML anterior genera un cuadro de diálogo sencillo para `Custom Component`. La parte crítica del archivo es el nodo `<message>` interno. Este cuadro de diálogo contiene un `textfield` simple denominado `Message` y persiste el valor del campo de texto en una propiedad denominada `message`.

   Se crea un modelo Sling junto a para exponer el valor de la propiedad `message` a través del modelo JSON.

   >[!NOTE]
   >
   > Puede ver muchos más [ejemplos de cuadros de diálogo al ver las definiciones de los componentes principales](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). También puede ver campos de formulario adicionales, como `select`, `textarea`, `pathfield`, disponibles debajo de `/libs/granite/ui/components/coral/foundation/form` en [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   AEM Con un componente tradicional de la, se suele requerir un script [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=es). SPA Dado que el procesa el componente, no se necesita ningún script HTL.

## Creación del modelo Sling

Los modelos Sling son objetos Java™ &quot;POJO&quot; (Plain Old Java™ Objects) impulsados por anotaciones que facilitan la asignación de datos de las variables JCR a Java™. AEM [Los modelos Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) suelen funcionar para encapsular lógica empresarial compleja del lado del servidor para componentes de la.

SPA En el contexto del Editor de modelos de Sling, los modelos de Sling exponen el contenido de un componente a través del modelo JSON mediante una función que utiliza el [Exportador del modelo Sling](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html?lang=es).

1. En el IDE de su elección, abra el módulo `core`. `CustomComponent.java` y `CustomComponentImpl.java` ya se han creado y tachado como parte del código de inicio del capítulo.

   >[!NOTE]
   >
   > Si utiliza el IDE de código de Visual Studio, puede resultar útil instalar [extensiones para Java™](https://code.visualstudio.com/docs/java/extensions).

2. Abra la interfaz de Java™ `CustomComponent.java` en `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![Interfaz CustomComponent.java](assets/custom-component/custom-component-interface.png)

   Esta es la interfaz Java™ que implementa el modelo Sling.

3. Actualizar `CustomComponent.java` para que extienda la interfaz `ComponentExporter`:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   La implementación de la interfaz `ComponentExporter` es un requisito para que la API del modelo JSON recoja automáticamente el modelo Sling.

   La interfaz `CustomComponent` incluye un solo método de captador `getMessage()`. Este es el método que expone el valor del cuadro de diálogo de autor a través del modelo JSON. En el modelo JSON solo se exportan los métodos de captador con parámetros vacíos `()`.

4. Abra `CustomComponentImpl.java` a las `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Esta es la implementación de la interfaz `CustomComponent`. La anotación `@Model` identifica la clase Java™ como modelo Sling. La anotación `@Exporter` permite serializar y exportar la clase Java™ a través del Exportador de modelos Sling.

5. AEM Actualice la variable estática `RESOURCE_TYPE` para que apunte al componente de la `wknd-spa-angular/components/custom-component` creado en el ejercicio anterior.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   AEM El tipo de recurso del componente es el que enlaza el modelo Sling al componente de y, en última instancia, se asigna al componente de Angular.

6. Agregue el método `getExportedType()` a la clase `CustomComponentImpl` para devolver el tipo de recurso de componente:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Este método es necesario al implementar la interfaz `ComponentExporter` y expone el tipo de recurso que permite la asignación al componente de Angular.

7. Actualice el método `getMessage()` para devolver el valor de la propiedad `message` conservada por el cuadro de diálogo de autor. Usar la anotación `@ValueMap` es asignar el valor JCR `message` a una variable Java™:

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

   Se agrega alguna &quot;lógica empresarial&quot; adicional para devolver el valor del mensaje como mayúsculas. Esto nos permite ver la diferencia entre el valor sin procesar almacenado por el cuadro de diálogo del autor y el valor expuesto por el modelo Sling.

   >[!NOTE]
   >
   > Puede ver [CustomComponentImpl.java finalizado aquí](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## Actualización del componente Angular

Ya se ha creado el código de Angular del componente personalizado. A continuación, realice algunas actualizaciones para asignar el componente de Angular AEM al componente de.

1. En el módulo `ui.frontend`, abra el archivo `ui.frontend/src/app/components/custom/custom.component.ts`
2. Observe la línea `@Input() message: string;`. Se espera que el valor en mayúsculas transformado se asigne a esta variable.
3. AEM SPA AEM Importe el objeto `MapTo` desde el SDK de JS de Editor de y utilícelo para asignarlo al componente de:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Abra `cutom.component.html` y observe que el valor de `{{message}}` se muestra en una etiqueta `<h2>`.
5. Abra `custom.component.css` y agregue la regla siguiente:

   ```css
   :host-context {
       display: block;
   }
   ```

   AEM Para que el marcador de posición del Editor de la se muestre correctamente cuando el componente está vacío, `:host-context` u otro `<div>` debe establecerse en `display: block;`.

6. AEM Implemente las actualizaciones en un entorno de trabajo local desde la raíz del directorio del proyecto con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Actualizar la directiva de plantilla

AEM SPA A continuación, navegue hasta la para comprobar las actualizaciones y permitir que se agregue `Custom Component` a la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos de la lista de elementos.

1. Compruebe el registro del nuevo modelo Sling navegando hasta [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   Debería ver las dos líneas anteriores que indican que `CustomComponentImpl` está asociado con el componente `wknd-spa-angular/components/custom-component` y que está registrado a través del Exportador de modelos Sling.

2. SPA Vaya a la plantilla de página de la página de en [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Actualice la directiva del contenedor de diseño para agregar el nuevo `Custom Component` como componente permitido:

   ![Actualizar directiva de contenedor de diseño](assets/custom-component/custom-component-allowed.png)

   Guarde los cambios en la directiva y observe `Custom Component` como un componente permitido:

   ![Componente personalizado como componente permitido](assets/custom-component/custom-component-allowed-layout-container.png)

## Crear el componente personalizado

AEM SPA A continuación, cree el `Custom Component` mediante el Editor de de trabajo.

1. Vaya a [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. En el modo `Edit`, agregue `Custom Component` a `Layout Container`:

   ![Insertar nuevo componente](assets/custom-component/insert-custom-component.png)

3. Abra el cuadro de diálogo del componente e introduzca un mensaje que contenga letras minúsculas.

   ![Configurar el componente personalizado](assets/custom-component/enter-dialog-message.png)

   Este es el cuadro de diálogo que se creó en función del archivo XML anteriormente en el capítulo.

4. Guarde los cambios. Observe que el mensaje que se muestra está en mayúsculas.

   ![Mensaje mostrado en Todo en mayúsculas](assets/custom-component/message-displayed.png)

5. Vea el modelo JSON navegando a [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Buscar `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   Observe que el valor JSON se establece en mayúsculas en función de la lógica agregada al modelo Sling.

## Enhorabuena. {#congratulations}

AEM ¡Enhorabuena! Ha aprendido a crear un componente de personalizado y cómo los modelos Sling y los cuadros de diálogo funcionan con el modelo JSON.

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) o desprotegerlo localmente cambiando a la rama `Angular/custom-component-solution`.

### Siguientes pasos {#next-steps}

AEM SPA [Ampliar un componente principal](extend-component.md): aprenda a ampliar un componente principal existente para utilizarlo con el Editor de. AEM SPA Comprender cómo añadir propiedades y contenido a un componente existente es una técnica potente para ampliar las capacidades de una implementación de Editor de segmentos de tiempo de ejecución de la aplicación de un editor de segmentos de tiempo de ejecución de la aplicación de un editor de tiempo de ejecución de la.

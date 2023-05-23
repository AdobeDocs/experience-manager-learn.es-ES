---
title: Ampliación de un componente principal AEM SPA | Introducción al Editor de y React
description: AEM SPA Obtenga información sobre cómo ampliar el modelo JSON para un componente principal existente que se utilizará con el Editor de. AEM SPA Comprender cómo añadir propiedades y contenido a un componente existente es una técnica potente para ampliar las capacidades de una implementación de Editor de segmentos de tiempo de ejecución de la aplicación de un editor de segmentos de tiempo de ejecución de la aplicación de un editor de tiempo de ejecución de la. Aprenda a utilizar el patrón de delegación para ampliar los modelos Sling y las funciones de la fusión de recursos de Sling.
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 2%

---

# Ampliación de un componente principal {#extend-component}

AEM SPA Obtenga información sobre cómo ampliar un componente principal existente para utilizarlo con el Editor de. AEM SPA Comprender cómo ampliar un componente existente es una técnica potente para personalizar y ampliar las capacidades de una implementación de Editor de de trabajo.

## Objetivo

1. Ampliar un componente principal existente con propiedades y contenido adicionales.
2. Comprenda los conceptos básicos de la herencia de componentes con el uso de `sling:resourceSuperType`.
3. Aprenda a aprovechar las [Patrón de delegación](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para que los modelos Sling reutilicen la lógica y la funcionalidad existentes.

## Qué va a generar

Este capítulo ilustra el código adicional necesario para añadir una propiedad adicional a un estándar `Image` para cumplir los requisitos de un nuevo componente `Banner` componente. El `Banner` El componente contiene las mismas propiedades que el estándar `Image` incluye una propiedad adicional para que los usuarios rellenen el componente **Texto del titular**.

![Componente de banner creado final](assets/extend-component/final-author-banner-component.png)

## Requisitos previos

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment). AEM SPA En este punto del tutorial se da por hecho que los usuarios tienen una comprensión sólida de la función de editor de contenido (Editor de contenido) de la.

## Herencia con el supertipo de recursos de Sling {#sling-resource-super-type}

Para ampliar un componente existente, establezca una propiedad denominada `sling:resourceSuperType` en la definición del componente.  `sling:resourceSuperType`es un [propiedad](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) AEM que se puede establecer en la definición de un componente de la que apunta a otro componente. Esto establece explícitamente que el componente heredará toda la funcionalidad del componente identificado como `sling:resourceSuperType`.

Si queremos ampliar la `Image` componente en `wknd-spa-react/components/image` necesitamos actualizar el código en la `ui.apps` módulo.

1. Cree una nueva carpeta debajo de `ui.apps` módulo para `banner` en `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. Debajo `banner` crear una Definición de componente (`.content.xml`) como la siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Esto establece `wknd-spa-react/components/banner` para heredar toda la funcionalidad de `wknd-spa-react/components/image`.

## cq:editConfig {#cq-edit-config}

El `_cq_editConfig.xml` AEM El archivo dicta el comportamiento de arrastrar y soltar en la interfaz de usuario de creación de la. Al ampliar el componente de imagen, es importante que el tipo de recurso coincida con el propio componente.

1. En el `ui.apps` módulo cree otro archivo debajo de `banner` nombrado `_cq_editConfig.xml`.
1. Rellenar `_cq_editConfig.xml` con el siguiente XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. El aspecto único del archivo es el `<parameters>` nodo que establece resourceType en `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   La mayoría de los componentes no requieren un `_cq_editConfig`. Los componentes de imagen y los descendientes son la excepción.

## Ampliación del cuadro de diálogo {#extend-dialog}

Nuestro `Banner` requiere un campo de texto adicional en el cuadro de diálogo para capturar el componente `bannerText`. Como estamos utilizando la herencia de Sling, podemos utilizar las funciones del [Fusión de recursos de Sling](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=es) para anular o ampliar partes del diálogo. En este ejemplo, se ha añadido una nueva pestaña al cuadro de diálogo para capturar datos adicionales de un autor para rellenar el componente de tarjeta.

1. En el `ui.apps` módulo, debajo de `banner` carpeta, cree una carpeta llamada `_cq_dialog`.
1. Debajo `_cq_dialog` crear un archivo de definición de Dialog `.content.xml`. Rellénelo con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   La definición XML anterior creará una nueva pestaña denominada **Texto** y pídalo *antes* el existente **Recurso** pestaña. Contendrá un solo campo **Texto del titular**.

1. El cuadro de diálogo tendrá el siguiente aspecto:

   ![Cuadro de diálogo final del titular](assets/extend-component/banner-dialog.png)

   Observe que no tuvimos que definir las pestañas para **Recurso** o **Metadatos**. Se heredan a través de `sling:resourceSuperType` propiedad.

   SPA Antes de poder previsualizar el cuadro de diálogo, es necesario implementar el Componente de y el `MapTo` función.

## SPA Implementar componente de {#implement-spa-component}

SPA SPA Para utilizar el componente Banner con el Editor de, se debe crear un nuevo componente de la que se asigne a `wknd-spa-react/components/banner`. Esto se hace en el `ui.frontend` módulo.

1. En el `ui.frontend` módulo crear una nueva carpeta para `Banner` en `ui.frontend/src/components/Banner`.
1. Cree un nuevo archivo llamado `Banner.js` debajo de `Banner` carpeta. Rellénelo con lo siguiente:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if (BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   SPA AEM Este componente se asigna al componente de la `wknd-spa-react/components/banner` creado anteriormente.

1. Actualizar `import-components.js` en `ui.frontend/src/components/import-components.js` para incluir el nuevo `Banner` SPA Componente de la:

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. AEM En este punto, el proyecto se puede implementar para que se ejecute y el cuadro de diálogo se pueda probar. Implemente el proyecto con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. SPA Actualice la directiva de la plantilla de la plantilla de la plantilla de la plantilla de datos para agregar `Banner` componente como un **componente permitido**.

1. SPA Navegue hasta una página de y añada `Banner` SPA Componente a una de las páginas de la:

   ![Añadir componente de titular](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > El cuadro de diálogo le permite guardar un valor para **Texto del titular** SPA sin embargo, este valor no se refleja en el componente de la. Para habilitarlo, necesitamos extender el modelo Sling para el componente.

## Añadir interfaz de Java {#java-interface}

Para exponer finalmente los valores del cuadro de diálogo del componente al componente React, debemos actualizar el modelo Sling que rellena el JSON para el `Banner` componente. Esto se hace en el `core` SPA que contiene todo el código Java de nuestro proyecto de.

En primer lugar, crearemos una nueva interfaz Java para `Banner` que amplía el `Image` Interfaz de Java.

1. En el `core` módulo cree un nuevo archivo con el nombre `BannerModel.java` en `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Rellenar `BannerModel.java` con lo siguiente:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   Esto heredará todos los métodos del componente principal `Image` y añada un nuevo método `getBannerText()`.

## Implementar el modelo Sling {#sling-model}

A continuación, implemente el modelo Sling para `BannerModel` interfaz.

1. En el `core` módulo cree un nuevo archivo con el nombre `BannerModelImpl.java` en `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. Rellenar `BannerModelImpl.java` con lo siguiente:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   }
   ```

   Observe el uso del `@Model` y `@Exporter` anotaciones para garantizar que el modelo Sling pueda serializarse como JSON mediante el exportador de modelos Sling.

   `BannerModelImpl.java` utiliza el [Patrón de delegación para modelos Sling](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para evitar volver a escribir toda la lógica desde el componente principal de imagen.

1. Revise las líneas siguientes:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   La anotación anterior creará una instancia de un objeto Image denominado `image` basado en el `sling:resourceSuperType` herencia de la `Banner` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   A continuación, es posible utilizar simplemente la variable `image` objeto para implementar métodos definidos por `Image` interfaz, sin tener que escribir la lógica nosotros mismos. Esta técnica se utiliza para `getSrc()`, `getAlt()` y `getTitle()`.

1. Abra una ventana de terminal e implemente solo las actualizaciones de `core` usando el módulo de Maven `autoInstallBundle` perfil de la `core` directorio.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## En resumen {#put-together}

1. AEM SPA Vuelva a la página de inicio y abra la página de la página que tiene la variable Volver a la página `Banner` componente.
1. Actualice el `Banner` componente que se incluirá **Texto del titular**:

   ![Texto del titular](assets/extend-component/banner-text-dialog.png)

1. Rellene el componente con una imagen:

   ![Cuadro de diálogo Agregar imagen al titular](assets/extend-component/banner-dialog-image.png)

   Guarde las actualizaciones del cuadro de diálogo.

1. Ahora debería ver el valor representado de **Texto del titular**:

![Texto del titular mostrado](assets/extend-component/banner-text-displayed.png)

1. Vea la respuesta del modelo JSON en: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) y busque el `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   Observe que el modelo JSON se actualiza con pares clave/valor adicionales después de implementar el modelo Sling en `BannerModelImpl.java`.

## Enhorabuena. {#congratulations}

AEM ¡Enhorabuena! Ha aprendido a ampliar un componente de mediante y cómo funcionan los modelos Sling y los cuadros de diálogo con el modelo JSON.

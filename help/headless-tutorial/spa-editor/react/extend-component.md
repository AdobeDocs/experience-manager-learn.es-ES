---
title: Ampliar un componente principal | Introducción al AEM SPA Editor y React
description: Obtenga información sobre cómo ampliar el modelo JSON para un componente principal existente para utilizarlo con el AEM SPA Editor. Comprender cómo añadir propiedades y contenido a un componente existente es una técnica eficaz para expandir las capacidades de una implementación AEM Editor SPA. Aprenda a utilizar el patrón de delegación para ampliar los modelos Sling y las funciones de fusión de recursos Sling.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 2%

---

# Ampliar un componente principal {#extend-component}

Obtenga información sobre cómo ampliar un componente principal existente para utilizarlo con el AEM SPA Editor. Explicación de cómo ampliar un componente existente es una técnica eficaz para personalizar y expandir las capacidades de una implementación AEM Editor SPA.

## Objetivo

1. Amplíe un componente principal existente con propiedades y contenido adicionales.
2. Comprender los conceptos básicos de la herencia de componentes con el uso de `sling:resourceSuperType`.
3. Aprenda a aprovechar el [Patrón de delegación](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para que los modelos Sling reutilicen la lógica y funcionalidad existentes.

## Qué va a generar

Este capítulo ilustra el código adicional necesario para añadir una propiedad adicional a un estándar `Image` para cumplir los requisitos de una nueva `Banner` componente. La variable `Banner` el componente contiene todas las mismas propiedades que el estándar `Image` pero incluye una propiedad adicional para que los usuarios rellenen el **Texto del banner**.

![Componente de banner creado definitivamente](assets/extend-component/final-author-banner-component.png)

## Requisitos previos

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment). Se asume en este punto en el tutorial en el que los usuarios tienen una comprensión sólida de la función AEM SPA Editor.

## Herencia con Sling Resource Super Type {#sling-resource-super-type}

Ampliación de un conjunto de componentes existente con una propiedad denominada `sling:resourceSuperType` en la definición del componente.  `sling:resourceSuperType`es [property](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) que se puede establecer en una definición de componente AEM que apunte a otro componente. Esto establece explícitamente el componente para heredar toda la funcionalidad del componente identificado como el `sling:resourceSuperType`.

Si queremos ampliar el `Image` componente en `wknd-spa-react/components/image` necesitamos actualizar el código en la variable `ui.apps` módulo.

1. Cree una nueva carpeta debajo de `ui.apps` módulo para `banner` at `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. Bajo `banner` crear una definición de componente (`.content.xml`) como el siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Este conjunto `wknd-spa-react/components/banner` para heredar toda la funcionalidad de `wknd-spa-react/components/image`.

## cq:editConfig {#cq-edit-config}

La variable `_cq_editConfig.xml` dicta el comportamiento de arrastrar y soltar en la interfaz de usuario de creación de AEM. Al ampliar el componente Imagen, es importante que el tipo de recurso coincida con el propio componente.

1. En el `ui.apps` crear otro archivo debajo de `banner` named `_cq_editConfig.xml`.
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

## Ampliar el cuadro de diálogo {#extend-dialog}

Nuestra `Banner` requiere un campo de texto adicional en el cuadro de diálogo para capturar el `bannerText`. Como se utiliza la herencia de Sling, podemos usar las características de la variable [Fusión de recursos de Sling](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=es) para anular o ampliar partes del cuadro de diálogo. En este ejemplo, se ha añadido una nueva pestaña al cuadro de diálogo para capturar datos adicionales de un autor y rellenar el componente de tarjeta.

1. En el `ui.apps` módulo, debajo de `banner` carpeta, crear una carpeta con el nombre `_cq_dialog`.
1. Bajo `_cq_dialog` crear un archivo de definición de cuadro de diálogo `.content.xml`. Rellene con lo siguiente:

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

   La definición XML anterior creará una nueva ficha llamada **Texto** y ordenarlo *before* el **Activo** pestaña . Contiene un solo campo **Texto del banner**.

1. El cuadro de diálogo tendrá un aspecto similar al siguiente:

   ![Cuadro de diálogo final del banner](assets/extend-component/banner-dialog.png)

   Tenga en cuenta que no fue necesario definir las pestañas para **Activo** o **Metadatos**. Se heredan mediante la variable `sling:resourceSuperType` propiedad.

   Para poder obtener una vista previa del cuadro de diálogo, debemos implementar el componente SPA y la variable `MapTo` función.

## Implementar SPA componente {#implement-spa-component}

Para utilizar el componente Titular con el Editor de SPA, se debe crear un nuevo componente de SPA que se asignará a `wknd-spa-react/components/banner`. Esto se hace en la variable `ui.frontend` módulo.

1. En el `ui.frontend` crear una carpeta nueva para `Banner` at `ui.frontend/src/components/Banner`.
1. Cree un nuevo archivo con el nombre `Banner.js` debajo del `Banner` carpeta. Rellene con lo siguiente:

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

   Este componente SPA se asigna al componente AEM `wknd-spa-react/components/banner` creado anteriormente.

1. Actualizar `import-components.js` at `ui.frontend/src/components/import-components.js` para incluir el nuevo `Banner` SPA componente:

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. En este punto, el proyecto se puede implementar en AEM y el diálogo se puede probar. Implemente el proyecto con sus habilidades con Maven:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Actualice la política de la plantilla de SPA para agregar la variable `Banner` componente como **componente permitido**.

1. Vaya a una página SPA y añada la variable `Banner` a una de las páginas SPA:

   ![Agregar componente de banner](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > El cuadro de diálogo le permite guardar un valor para **Texto del banner** pero este valor no se refleja en el componente SPA. Para activarlo, es necesario ampliar el modelo Sling para el componente.

## Añadir interfaz Java {#java-interface}

Para exponer finalmente los valores del cuadro de diálogo del componente al componente React , debemos actualizar el modelo de Sling que rellena el JSON para el `Banner` componente. Esto se hace en la variable `core` que contiene todo el código Java para nuestro proyecto SPA.

Primero crearemos una nueva interfaz Java para `Banner` que amplía el `Image` Interfaz de Java.

1. En el `core` crear un nuevo archivo denominado `BannerModel.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
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

   Esto heredará todos los métodos del componente principal `Image` interfaz y agregar un nuevo método `getBannerText()`.

## Implementar modelo de Sling {#sling-model}

A continuación, implemente el modelo de Sling para la variable `BannerModel` interfaz.

1. En el `core` crear un nuevo archivo denominado `BannerModelImpl.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

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

   Observe el uso de la variable `@Model` y `@Exporter` anotaciones para garantizar que el modelo de Sling se pueda serializar como JSON a través del exportador de modelos Sling.

   `BannerModelImpl.java` utiliza la variable [Patrón de delegación para modelos Sling](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) para evitar reescribir toda la lógica del componente principal de la imagen.

1. Revise las siguientes líneas:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   La anotación anterior creará una instancia de un objeto de imagen denominado `image` en función de la variable `sling:resourceSuperType` herencia de `Banner` componente.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Por lo tanto, es posible utilizar simplemente la variable `image` objeto para implementar métodos definidos por el `Image` , sin tener que escribir la lógica nosotros mismos. Esta técnica se utiliza para `getSrc()`, `getAlt()` y `getTitle()`.

1. Abra una ventana de terminal e implemente solo las actualizaciones de `core` módulo con Maven `autoInstallBundle` perfil de `core` directorio.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## En resumen {#put-together}

1. Vuelva a AEM y abra la página de SPA que tiene la variable `Banner` componente.
1. Actualice el `Banner` componente que incluir **Texto del banner**:

   ![Texto del banner](assets/extend-component/banner-text-dialog.png)

1. Rellene el componente con una imagen:

   ![Agregar imagen al cuadro de diálogo del banner](assets/extend-component/banner-dialog-image.png)

   Guarde las actualizaciones del cuadro de diálogo.

1. Ahora debería ver el valor procesado de **Texto del banner**:

![Texto del banner mostrado](assets/extend-component/banner-text-displayed.png)

1. Vea la respuesta del modelo JSON en: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) y busque la variable `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   Observe que el modelo JSON se actualiza con pares clave/valor adicionales después de implementar el modelo Sling en `BannerModelImpl.java`.

## Felicitaciones! {#congratulations}

Felicidades, ha aprendido a ampliar un componente AEM utilizando y cómo funcionan los modelos y diálogos de Sling con el modelo JSON.

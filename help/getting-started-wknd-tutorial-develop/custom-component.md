---
title: Componente personalizado
description: Abarca la creación de extremo a extremo de un componente de byline personalizado que muestra contenido creado. Incluye el desarrollo de un modelo de Sling para encapsular la lógica empresarial a fin de completar el componente de byline y el correspondiente HTL para procesar el componente.
sub-product: sitios
feature: sling-models
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3961'
ht-degree: 0%

---


# Componente personalizado {#custom-component}

Este tutorial trata la creación de extremo a extremo de un componente de firma AEM personalizado que muestra el contenido creado en un cuadro de diálogo y explora el desarrollo de un modelo Sling para encapsular la lógica empresarial que rellena el HTL del componente.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

Consulte el código de línea base que el tutorial genera:

1. Consulte la rama `tutorial/custom-component-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Implemente código base en una instancia de AEM local con sus conocimientos Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede realizar la vista del código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) o extraer el código localmente cambiando a la rama `tutorial/custom-component-solution`.

## Objetivo

1. Comprender cómo crear un componente de AEM personalizado
1. Aprenda a encapsular la lógica empresarial con modelos Sling
1. Comprender cómo utilizar un modelo Sling desde una secuencia de comandos HTL

## Qué va a generar {#byline-component}

En esta parte del tutorial de WKND, se crea un componente de firma que se utilizará para mostrar información de autor sobre el colaborador de un artículo.

![ejemplo de componente byline](assets/custom-component/byline-design.png)

*Componente de firma*

La implementación del componente Firma incluye un cuadro de diálogo que recopila el contenido de la línea y un modelo de línea personalizado que recupera el de la línea:

* Nombre
* Imagen
* Ocupaciones

## Crear componente de firma {#create-byline-component}

En primer lugar, cree la estructura del nodo Componente de firma y defina un cuadro de diálogo. Representa el componente en AEM y define implícitamente el tipo de recurso del componente según su ubicación en el JCR.

El cuadro de diálogo expone la interfaz que pueden proporcionar los autores de contenido. Para esta implementación, se aprovechará el componente **Image** del componente principal de WCM AEM para administrar la creación y representación de la imagen de Byline, de modo que se establecerá como el `sling:resourceSuperType` del componente.

### Crear definición de componente {#create-component-definition}

1. En el módulo **ui.apps**, navegue hasta `/apps/wknd/components` y cree una nueva carpeta denominada `byline`.
1. Debajo de la carpeta `byline`, agregue un nuevo archivo denominado `.content.xml`

   ![cuadro de diálogo para crear nodo](assets/custom-component/byline-node-creation.png)

1. Rellene el archivo `.content.xml` con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   El archivo XML anterior proporciona la definición del componente, incluido el título, la descripción y el grupo. El `sling:resourceSuperType` apunta a `core/wcm/components/image/v2/image`, que es el [Componente de imagen principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html).

### Crear la secuencia de comandos HTML {#create-the-htl-script}

1. Debajo de la carpeta `byline`, agregue un nuevo archivo `byline.html`, que es responsable de la presentación HTML del componente. Es importante asignar un nombre al archivo igual que a la carpeta, ya que se convierte en la secuencia de comandos predeterminada que Sling utilizará para representar este tipo de recurso.

1. Añada el siguiente código a `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` se  [volverá a examinar más adelante](#byline-htl), una vez creado el modelo de Sling. El estado actual del archivo HTML permite que el componente se muestre en estado vacío, en el Editor de páginas de AEM Sites cuando se arrastra y se suelta en la página.

### Crear la definición de cuadro de diálogo {#create-the-dialog-definition}

A continuación, defina un cuadro de diálogo para el componente Firma con los campos siguientes:

* **Nombre**: campo de texto que indica el nombre del colaborador.
* **Imagen**: una referencia a la biografía del colaborador.
* **Ocupaciones**: lista de las ocupaciones atribuidas al contribuyente. Las ocupaciones deben ordenarse alfabéticamente en orden ascendente (a a z).

1. Debajo de la carpeta `byline`, cree una nueva carpeta con el nombre `_cq_dialog`.
1. Debajo de `byline/_cq_dialog` agregue un nuevo archivo llamado `.content.xml`. Es la definición XML del cuadro de diálogo. Añada el siguiente XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   Estas definiciones de nodo de cuadro de diálogo utilizan el [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) para controlar qué fichas de diálogo se heredan del componente `sling:resourceSuperType`, en este caso el componente de imagen **Core Components&#39;**.

   ![cuadro de diálogo completado para la línea de ayuda](assets/custom-component/byline-dialog-created.png)

### Crear el cuadro de diálogo Directiva {#create-the-policy-dialog}

Siguiendo el mismo método que con la creación del cuadro de diálogo, cree un cuadro de diálogo de directiva (anteriormente conocido como cuadro de diálogo de diseño) para ocultar los campos no deseados en la configuración de directiva heredada del componente Imagen de componentes principales.

1. Debajo de la carpeta `byline`, cree una nueva carpeta con el nombre `_cq_design_dialog`.
1. Debajo de `byline/_cq_design_dialog` cree un nuevo archivo con el nombre `.content.xml`. Actualice el archivo con lo siguiente: con el siguiente XML. Es más fácil abrir `.content.xml` y copiar/pegar el XML siguiente en él.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   La base para el **cuadro de diálogo de directivas** XML anterior se obtuvo del [componente de imagen de componentes principales](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Al igual que en la configuración de Dialogue, [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) se utiliza para ocultar los campos irrelevantes que, de lo contrario, se heredan de `sling:resourceSuperType`, como lo ven las definiciones de nodos con la propiedad `sling:hideResource="{Boolean}true"`.

### Implementar el código {#deploy-the-code}

1. Implemente la base de código actualizada en una instancia de AEM local utilizando sus conocimientos de Maven:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Añadir el componente en una página {#add-the-component-to-a-page}

Para mantener las cosas simples y centradas en AEM desarrollo de componentes, agregaremos el componente Firma en su estado actual a una página de artículo para verificar que la definición de nodo `cq:Component` está implementada y es correcta, AEM reconoce la nueva definición de componente y el cuadro de diálogo del componente funciona para la creación.

### Añadir una imagen en el AEM Assets

En primer lugar, cargue una toma de cabeza de muestra en AEM Assets para utilizarla para rellenar la imagen en el componente Firma.

1. Vaya a la carpeta LOS Skateparks de AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Cargue la captura de cabeza para **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** en la carpeta.

   ![Se cargó un tiro de cabeza](assets/custom-component/stacey-roswell-headshot-assets.png)

### Cree el componente {#author-the-component}

A continuación, agregue el componente Firma a una página de AEM. Dado que hemos agregado el componente Firma al grupo de componentes **Proyecto de sitios WKND - Contenido** mediante la definición `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml`, está disponible automáticamente para cualquier **Contenedor** cuya **Política** permite el grupo de componentes **Proyecto de sitios WKND - Contenido**, que es el Contenedor Diseño de la página de la página de artículo ...

1. Vaya al artículo de LA Skatepark en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Desde la barra lateral izquierda, arrastre y suelte un **componente de firma** en **inferior** del Contenedor Diseño de la página de artículos abierta.

   ![agregar componente de línea a la página](assets/custom-component/add-to-page.png)

1. Asegúrese de que la **barra lateral izquierda está abierta** y visible, y de que **Buscador de recursos** está seleccionado.

   ![buscador de recursos abierto](assets/custom-component/open-asset-finder.png)

1. Seleccione el **marcador de posición del componente de firma**, que a su vez muestra la barra de acciones y toque el icono **llave inglesa** para abrir el cuadro de diálogo.

   ![barra de acciones de componente](assets/custom-component/action-bar.png)

1. Con el cuadro de diálogo abierto y la primera ficha (Recurso) activa, abra la barra lateral izquierda y, desde el buscador de recursos, arrastre una imagen a la zona desplegable Imagen. Busque &quot;stacey&quot; para encontrar la imagen biográfica de Stacey Roswells en el paquete de contenido ui.ui de WKND.

   ![agregar imagen al cuadro de diálogo](assets/custom-component/add-image.png)

1. Después de agregar una imagen, haga clic en la ficha **Propiedades** para introducir el **Nombre** y **Ocupaciones**.

   Al introducir las ocupaciones, introdúzcalas en **orden alfabético inverso** para que la lógica empresarial alfabética que implementaremos en el modelo de Sling sea fácilmente evidente.

   Toque el botón **Listo** en la parte inferior derecha para guardar los cambios.

   ![rellenado de propiedades del componente byline](assets/custom-component/add-properties.png)

   Los autores de AEM configuran y crean componentes a través de los cuadros de diálogo. En este momento, en el desarrollo del componente Firma, se incluyen los diálogos para recopilar los datos, aunque aún no se ha agregado la lógica para procesar el contenido creado. Por lo tanto, solo aparece el marcador de posición.

1. Después de guardar el cuadro de diálogo, vaya a [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) y revise cómo se almacena el contenido del componente en el nodo de contenido del componente de byline, en la página AEM.

   Busque el nodo de contenido del componente Firma debajo de la página Parques de máscara LA, por ejemplo `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Observe que los nombres de propiedad `name`, `occupations` y `fileReference` se almacenan en el nodo **byline**.

   Además, observe que el `sling:resourceType` nodo está establecido en `wknd/components/content/byline`, que es lo que enlaza este nodo de contenido a la implementación del componente Byline.

   ![propiedades de byline en CRXDE](assets/custom-component/byline-properties-crxde.png)

## Crear modelo Sling de firma {#create-sling-model}

A continuación, crearemos un modelo Sling para que actúe como modelo de datos y hospede la lógica comercial del componente Byline.

Los modelos de Sling son objetos Java &quot;POJO&quot; (Plain Old Java Objects) impulsados por anotaciones que facilitan la asignación de datos de JCR a variables Java y proporcionan otras variedades cuando se desarrollan en el contexto de AEM.

### Revisar las dependencias de los menúes {#maven-dependency}

El modelo Sling de Byline se basará en varias API de Java proporcionadas por AEM. Estas API están disponibles a través del `dependencies` enumerado en el archivo POM del módulo `core`. El proyecto utilizado para este tutorial se ha creado para AEM como Cloud Service. Sin embargo, es único en que es compatible con AEM 6.5/6.4. Por lo tanto, se incluyen las dependencias Cloud Service y AEM 6.x.

1. Abra el archivo `pom.xml` debajo de `<src>/aem-guides-wknd/core/pom.xml`.
1. Busque la dependencia para `aem-sdk-api` - **AEM como Cloud Service solamente**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   El [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#building-for-the-sdk) contiene todas las API de Java públicas expuestas por AEM. El `aem-sdk-api` se utiliza de forma predeterminada al crear este proyecto. La versión se mantiene en el reactor principal situado en la raíz del proyecto en `aem-guides-wknd/pom.xml`.

1. Busque la dependencia para `uber-jar` - **AEM sólo 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   El `uber-jar` sólo se incluye cuando se invoca el perfil `classic`, es decir, `mvn clean install -PautoInstallSinglePackage -Pclassic`. Nuevamente, esto es exclusivo de este proyecto. En un proyecto real, generado a partir del Arquetipo de Proyecto AEM, el `uber-jar` será el valor predeterminado si la versión de AEM especificada es 6.5 o 6.4.

   El [suber-jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contiene todas las API de Java públicas expuestas por AEM 6.x. La versión se mantiene en el reactor principal ubicado en la raíz del proyecto `aem-guides-wknd/pom.xml`.

1. Busque la dependencia para `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Todas las API de Java públicas expuestas por AEM componentes principales. AEM componentes principales es un proyecto que se mantiene fuera de AEM y, por lo tanto, tiene un ciclo de lanzamiento independiente. Por este motivo es una dependencia que debe incluirse por separado y **no** se incluye con `uber-jar` o `aem-sdk-api`.

   Al igual que la biberón, la versión de esta dependencia se mantiene en el archivo pom del reactor principal ubicado en `aem-guides-wknd/pom.xml`.

   Más adelante en este tutorial se utilizará la clase Imagen de componente principal para mostrar la imagen en el componente Firma. Es necesario contar con la dependencia del componente principal para construir y compilar nuestro modelo Sling.

### Interfaz de firma {#byline-interface}

Cree una interfaz pública de Java para el Firme. `Byline.java` define los métodos públicos necesarios para conducir el script  `byline.html` HTL.

1. Dentro del módulo `aem-guides-wknd.core` debajo de `core/src/main/java/com/adobe/aem/guides/wknd/core/models`, cree un nuevo archivo denominado `Byline.java`

   ![crear interfaz de línea de ganancia](assets/custom-component/create-byline-interface.png)

1. Actualice `Byline.java` con los siguientes métodos:

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   Los dos primeros métodos exponen los valores para las ocupaciones **name** y **occupations** para el componente Byline.

   El método `isEmpty()` se utiliza para determinar si el componente tiene contenido para procesar o si está a la espera de ser configurado.

   Observe que no hay ningún método para la imagen; [veremos por qué es más tarde](#tackling-the-image-problem).

### Implementación de firma {#byline-implementation}

`BylineImpl.java` es la implementación del modelo de Sling que implementa la  `Byline.java` interfaz definida anteriormente. El código completo para `BylineImpl.java` se encuentra en la parte inferior de esta sección.

1. Cree una nueva carpeta con el nombre `impl` debajo de `core/src/main/java/com/adobe/aem/guides/core/models`.
1. En la carpeta `impl`, cree un nuevo archivo `BylineImpl.java`.

   ![Archivo Impl Firme](assets/custom-component/byline-impl-file.png)

1. Abra `BylineImpl.java`. Especifique que implementa la interfaz `Byline`. Utilice las funciones de autocompletar del IDE o actualice manualmente el archivo para incluir los métodos necesarios para implementar la interfaz `Byline`:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. Añada las anotaciones del modelo de Sling actualizando `BylineImpl.java` con las anotaciones de nivel de clase siguientes. Esta `@Model(..)`anotación es lo que convierte la clase en un modelo Sling.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
       ...
   }
   ```

   Veamos esta anotación y sus parámetros:

   * La anotación `@Model` registra BylineImpl como modelo Sling cuando se implementa en AEM.
   * El parámetro `adaptables` especifica que la solicitud puede adaptar este modelo.
   * El parámetro `adapters` permite que la clase de implementación se registre en la interfaz Byline. Esto permite que la secuencia de comandos HTL llame al Modelo Sling a través de la interfaz (en lugar del impl directamente). [Puede encontrar más detalles sobre los adaptadores aquí](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * El `resourceType` señala el tipo de recurso de componente Firma (creado anteriormente) y ayuda a resolver el modelo correcto si hay varias implementaciones. [Aquí](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130) encontrará más información sobre la asociación de una clase de modelo con un tipo de recurso.

### Implementación de los métodos del modelo Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

El primer método que trataremos es `getName()`, que simplemente devuelve el valor almacenado en el nodo de contenido JCR del byline bajo la propiedad `name`.

Para ello, la anotación `@ValueMapValue` Modelo de sling se utiliza para insertar el valor en un campo Java mediante el ValueMap del recurso de la solicitud.


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

Dado que la propiedad JCR comparte el mismo nombre que el campo Java (ambos son &quot;name&quot;), `@ValueMapValue` resuelve automáticamente esta asociación e inyecta el valor de la propiedad en el campo Java.

#### getOccupations() {#implementing-get-occupations}

El siguiente método para implementar es `getOccupations()`. Este método recopila todas las ocupaciones almacenadas en la propiedad JCR `occupations` y devuelve una colección ordenada (alfabéticamente) de ellas.

Utilizando la misma técnica explorada en `getName()`, el valor de la propiedad se puede inyectar en el campo del Modelo Sling.

Una vez que los valores de la propiedad JCR están disponibles en el modelo Sling mediante el campo Java insertado `occupations`, la lógica de la organización puede aplicarse en el método `getOccupations()`.


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

El último método público es `isEmpty()`, que determina cuándo el componente debe considerarse &quot;suficientemente creado&quot; para representarse.

Para este componente, tenemos requisitos comerciales que indican que los tres campos, nombre, imagen y ocupaciones deben rellenarse *antes de* que se pueda procesar el componente.


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### Abordar el &quot;problema de la imagen&quot; {#tackling-the-image-problem}

Comprobar el nombre y las condiciones de ocupación son triviales (y Apache Commons Lang3 proporciona la siempre práctica clase [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html)), sin embargo, no está claro cómo se puede validar la **presencia de la imagen**, ya que el componente de Imagen de componentes principales se utiliza para mostrar la imagen.

Hay dos maneras de enfrentarlo:

Compruebe si la propiedad `fileReference` JCR se resuelve en un recurso. ** ORConvierta este recurso en un modelo de Sling de imagen de componente principal y asegúrese de que el  `getSrc()` método no está vacío.

Optaremos por el enfoque **segundo**. El primer enfoque es probablemente suficiente, pero en este tutorial se utilizará el último para permitirnos explorar otras características de los modelos Sling.

1. Cree un método privado que obtenga la imagen. Este método se deja en privado porque no necesitamos exponer el objeto Image en el propio HTL y su único uso para conducir `isEmpty().`

   El siguiente método privado para `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Como se ha señalado anteriormente, existen dos métodos más para obtener el **Modelo de sondeo de imagen**:

   El primero utiliza la anotación `@Self` para adaptar automáticamente la solicitud actual a la `Image.class` del componente principal

   ```java
   @Self
   private Image image;
   ```

   El segundo utiliza el servicio OSGi [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html), que es un servicio muy práctico, y nos ayuda a crear modelos Sling de otros tipos en el código Java.

   Optaremos por el segundo enfoque.

   >[!NOTE]
   >
   >En una implementación en el mundo real, es preferible el enfoque &quot;Uno&quot;, que utiliza `@Self`, ya que es la solución más simple y elegante. En este tutorial vamos a utilizar el segundo enfoque, ya que nos requiere explorar más facetas de los modelos Sling que son extremadamente útiles son componentes más complejos!

   Dado que los modelos Sling son de Java POJO, y no los servicios OSGi, las anotaciones de inyección OSGi habituales `@Reference` **no pueden** utilizarse, en cambio los modelos Sling proporcionan una anotación especial **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** que proporciona una funcionalidad similar.

1. Actualice `BylineImpl.java` para incluir la anotación `OSGiService` para inyectar la `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   Con `ModelFactory` disponible, se puede crear un modelo de Sling de imagen de componente principal mediante:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Sin embargo, este método requiere tanto una solicitud como un recurso, que aún no están disponibles en el modelo de Sling. Para obtener estas anotaciones, se utilizan más anotaciones del modelo de Sling.

   Para obtener la solicitud actual, se puede utilizar la anotación **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** para insertar el `adaptable` (que se define en el `@Model(..)` como `SlingHttpServletRequest.class`, en un campo de clase Java.

1. Añada la anotación **@Self** para obtener la solicitud **SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Recuerde que el uso de `@Self Image image` para inyectar el Modelo de sondeo de imagen de componente principal era una opción anterior: la anotación `@Self` intenta inyectar el objeto adaptable (en nuestro caso, una SlingHttpServletRequest) y adaptarse al tipo de campo de anotación. Dado que el modelo de Sling de imágenes de componentes principales se puede adaptar desde objetos SlingHttpServletRequest, funcionaría y es menos código que nuestro enfoque más exploratorio.

   Ahora hemos insertado las variables necesarias para crear una instancia de nuestro modelo de imagen mediante la API ModelFactory. Utilizaremos la anotación **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** de Sling Model para obtener este objeto después de que se cree una instancia de Sling Model.

   `@PostConstruct` es increíblemente útil y actúa con una capacidad similar a la de un constructor; sin embargo, se invoca después de crear una instancia de la clase y de insertar todos los campos Java anotados. Mientras que otras anotaciones del Modelo de sling anotan campos de clase Java (variables), `@PostConstruct` anota un método de parámetro void, zero, llamado generalmente `init()` (pero se puede denominar cualquier cosa).

1. Añadir el método **@PostConstruct**:

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   Recuerde que los modelos Sling son **NO** Servicios OSGi, por lo que es seguro mantener el estado de la clase. A menudo `@PostConstruct` deriva y configura el estado de la clase del Modelo Sling para su uso posterior, similar a lo que hace un constructor sin formato.

   Tenga en cuenta que si el método `@PostConstruct` emite una excepción, el modelo de Sling no creará una instancia (será nulo).

1. **getImage() ahora se** puede actualizar para devolver simplemente el objeto de imagen.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Volvamos a `isEmpty()` y terminemos la implementación:

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   Tenga en cuenta que las llamadas múltiples a `getImage()` no son problemáticas ya que devuelve la variable de clase `image` inicializada y no invoca `modelFactory.getModelFromWrappedRequest(...)`, que no es demasiado costosa, pero vale la pena evitar llamar innecesariamente.

1. El `BylineImpl.java` final debe tener el siguiente aspecto:


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## Firma HTL {#byline-htl}

En el módulo `ui.apps`, abra `/apps/wknd/components/byline/byline.html` que hemos creado en la configuración anterior del componente AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Analicemos lo que hace este script HTL hasta ahora:

* El `placeholderTemplate` apunta al marcador de posición Componentes principales, que se muestra cuando el componente no está completamente configurado. Esto se representa en el Editor de páginas de AEM Sites como un cuadro con el título del componente, tal como se define arriba en la propiedad `cq:Component` `jcr:title`.

* El `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carga el `placeholderTemplate` definido arriba y pasa un valor booleano (actualmente codificado como `false`) a la plantilla de marcador de posición. Cuando `isEmpty` es true, la plantilla de marcador de posición procesa el cuadro gris, de lo contrario no procesa nada.

### Actualizar HTL de firma

1. Actualice **byline.html** con la siguiente estructura de HTML esquelético:

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!-- Include the Core Components Image Component -->
           </div>
           <h2 class="cmp-byline__name"><!-- Include the name --></h2>
           <p class="cmp-byline__occupations"><!-- Include the occupations --></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Tenga en cuenta que las clases CSS siguen la [convención de nombres de BEM](https://getbem.com/naming/). Aunque el uso de convenciones BEM no es obligatorio, se recomienda utilizar BEM en clases CSS de componentes principales y, por lo general, se obtienen reglas CSS limpias y legibles.

### Creación de instancias de objetos del modelo Sling en HTL {#instantiating-sling-model-objects-in-htl}

La [sentencia de bloque de uso](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) se utiliza para crear instancias de objetos del Modelo de Sling en la secuencia de comandos HTL y asignarla a una variable HTL.

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utiliza la interfaz Byline (com.adobe.aem.guide.wknd.models.Byline) implementada por BylineImpl y adapta a ella la SlingHttpServletRequest actual, y el resultado se almacena en una variable HTL name byline (  `data-sly-use.<variable-name>`).

1. Actualice el `div` externo para hacer referencia al modelo de sling **Byline** por su interfaz pública:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Acceso a los métodos del modelo Sling {#accessing-sling-model-methods}

HTL toma de JSTL y utiliza la misma abreviación de los nombres de métodos de captador de Java.

Por ejemplo, invocar el método `getName()` del Modelo de limitación puede abreviarse como `byline.name`, de manera similar en lugar de `byline.isEmpty`, se puede abreviar como `byline.empty`. El uso de nombres de método completos, `byline.getName` o `byline.isEmpty`, también funciona. Tenga en cuenta que `()` nunca se utilizan para invocar métodos en HTL (similar a JSTL).

Los métodos Java que requieren un parámetro **no pueden** utilizarse en HTL. Esto es por diseño para mantener la lógica en HTL simple.

1. El nombre de firma se puede agregar al componente invocando el método `getName()` en el Modelo de firma de firma o en HTL: `${byline.name}`.

   Actualice la etiqueta `h2`:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Uso de las opciones de Expresión HTML {#using-htl-expression-options}

[Las ](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) opciones de Expresiones HTL actúan como modificadores del contenido en HTL y van desde el formato de fecha hasta la traducción en i18n. También se pueden utilizar expresiones para unir listas o conjuntos de valores, lo que es lo que se necesita para mostrar las ocupaciones en un formato delimitado por comas.

Las expresiones se agregan mediante el operador `@` en la expresión HTL.

1. Para unirse a la lista de ocupaciones con &quot;, &quot;, se utiliza el siguiente código:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Visualización condicional del marcador de posición {#conditionally-displaying-the-placeholder}

La mayoría de las secuencias de comandos HTL para componentes AEM aprovechan el **paradigma de marcador de posición** para proporcionar una indicación visual a los autores **indicando que un componente se ha creado incorrectamente y no se mostrará en AEM Publish**. La convención para impulsar esta decisión es implementar un método en el modelo Sling del componente, en nuestro caso: `Byline.isEmpty()`.

`isEmpty()` se invoca en el Modelo de desplazamiento en línea y el resultado (o más bien su negativo, a través del  `!` operador) se guarda en una variable HTL denominada  `hasContent`:

1. Actualice el `div` externo para guardar una variable HTL denominada `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Tenga en cuenta que el uso de `data-sly-test`, el bloque HTL `test` es interesante en el sentido de que ambos establecen una variable HTL Y procesa/no representa el elemento HTML en el que está, en base a si el resultado de la expresión HTL es veraz o no. Si es &quot;true&quot;, el elemento HTML se procesa, de lo contrario no se procesa.

   Esta variable HTL `hasContent` ahora se puede volver a utilizar para mostrar u ocultar condicionalmente el marcador de posición.

1. Actualice la llamada condicional a `placeholderTemplate` en la parte inferior del archivo con lo siguiente:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Mostrar la imagen con los componentes principales {#using-the-core-components-image}

La secuencia de comandos HTL para `byline.html` ya está casi completa y solo falta la imagen.

Ya que utilizamos `sling:resourceSuperType` el componente Imagen de componentes principales para crear la imagen, también podemos utilizar el componente Imagen de componente principal para representar la imagen.

Para ello, es necesario incluir el recurso de byline actual, pero forzar el tipo de recurso del componente Imagen de componentes principales mediante el tipo de recurso `core/wcm/components/image/v2/image`. Este es un patrón potente para la reutilización de componentes. Para ello, se utiliza el bloque `data-sly-resource` de HTL.

1. Reemplace `div` por una clase de `cmp-byline__image` con lo siguiente:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Este `data-sly-resource`, incluía el recurso actual a través de la ruta relativa `'.'` y fuerza la inclusión del recurso actual (o el recurso de contenido de línea directa) con el tipo de recurso de `core/wcm/components/image/v2/image`.

   El tipo de recurso Componente principal se utiliza directamente, y no a través de un proxy, ya que se trata de un uso en secuencia de comandos y nunca se ha mantenido en el contenido.

2. Se completó `byline.html` a continuación:

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Implemente la base de código en una instancia de AEM local. Dado que se han realizado cambios importantes en los archivos POM, realice una compilación completa de Maven a partir del directorio raíz del proyecto.

   ```shell
   $ cd aem-guides-wknd/
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si se implementa en AEM 6.5/6.4, invoque el perfil `classic`:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

### Revisión del componente sin estilo Firma {#reviewing-the-unstyled-byline-component}

1. Después de implementar la actualización, navegue a la página [Ultimate Guide to LA Skateparks ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) o donde haya agregado el componente Byline anteriormente en el capítulo.

1. La **imagen**, **nombre** y **ocupaciones** ahora aparece y tenemos un componente Byline sin estilo pero en funcionamiento.

   ![componente de línea sin estilo](assets/custom-component/unstyled.png)

### Revisión del registro del modelo Sling {#reviewing-the-sling-model-registration}

La [vista de estado de modelos Sling de la consola web de AEM](http://localhost:4502/system/console/status-slingmodels) muestra todos los modelos Sling registrados en AEM. El modelo de firma puede validarse como instalado y reconocido mediante la revisión de esta lista.

Si **BylineImpl** no se muestra en esta lista, es probable que haya un problema con las anotaciones del modelo de Sling o que el modelo de Sling no se haya agregado al paquete de modelos Sling registrado (com.adobe.aem.guías.wknd.core.models) en el proyecto principal.

![Modelo Sling de firma registrado](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## Estilos de firma {#byline-styles}

El componente Firma debe tener un estilo que se ajuste al diseño creativo del componente Firma. Esto se logrará mediante el uso de SCSS, que AEM proporciona soporte a través del subproyecto **ui.front** Maven.

### Añadir un estilo predeterminado

Añada estilos predeterminados para el componente Firma. En el proyecto **ui.front** en `/src/main/webpack/components`:

1. Cree un nuevo archivo denominado `_byline.scss`.

   ![explorador de proyectos de byline](assets/custom-component/byline-style-project-explorer.png)

1. Añada el CSS de implementaciones de firma (escrito como SCSS) en el `default.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. Revise `main.scss` en `ui.frontend/src/main/webpack/site/main.scss`:

   ```scss
   @import 'variables';
   @import 'wkndicons';
   @import 'base';
   @import '../components/**/*.scss';
   @import './styles/*.scss';
   ```

   `main.scss` es el punto de entrada principal para los estilos incluidos en el  `ui.frontend` módulo. La expresión regular `'../components/**/*.scss'` incluirá todos los archivos de la carpeta `components/`.

1. Cree e implemente el proyecto completo para AEM:

   ```shell
   $ cd aem-guides-wknd/
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si utiliza AEM 6.4/6.5, agregue el perfil `-Pclassic`.

   >[!TIP]
   >
   >Es posible que tenga que borrar la caché del navegador para asegurarse de que no se proporciona una CSS antigua y actualizar la página con el componente Firma para obtener el estilo completo.

## Colocarlo en conjunto {#putting-it-together}

A continuación, se muestra el aspecto que debería tener el componente Firma con estilo y completamente creado en la página de AEM.

![componente byline terminado](assets/custom-component/final-byline-component.png)

## Felicitaciones! {#congratulations}

¡Enhorabuena, acaba de crear un componente personalizado desde cero con Adobe Experience Manager!

### Próximos pasos {#next-steps}

Continúe aprendiendo sobre AEM desarrollo de componentes explorando cómo escribir pruebas JUnit para el código Java de Byline para garantizar que todo se desarrolle correctamente y que la lógica empresarial implementada sea correcta y completa.

* [Escritura de pruebas unitarias o AEM componentes](unit-testing.md)

Vista el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código de forma local en la plataforma Git `tutorial/custom-component-solution`.

1. Clona el repositorio [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Consulte la rama `tutorial/custom-component-solution`

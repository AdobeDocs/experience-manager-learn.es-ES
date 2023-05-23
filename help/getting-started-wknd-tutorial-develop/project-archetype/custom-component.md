---
title: Componente personalizado
description: Abarca la creación de extremo a extremo de un componente de firma personalizado que muestra el contenido creado. Incluye el desarrollo de un modelo Sling para encapsular la lógica empresarial y rellenar el componente de firma y el HTL correspondiente para procesar el componente.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: 434f56e143bc0f969723de48abd26d49a308af9b
workflow-type: tm+mt
source-wordcount: '4061'
ht-degree: 1%

---

# Componente personalizado {#custom-component}

Este tutorial trata la creación completa de un formulario personalizado `Byline` AEM Componente que muestra el contenido creado en un cuadro de diálogo y explora el desarrollo de un modelo Sling para encapsular la lógica empresarial que rellena el HTL del componente.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment).

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la `tutorial/custom-component-start` bifurcar desde [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. AEM Implemente una base de código en una instancia de local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM Si se utiliza la versión 6.5 o 6.4, se debe anexar la variable `classic` de perfil a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) o compruebe el código localmente cambiando a la rama `tutorial/custom-component-solution`.

## Objetivo

1. AEM Obtenga información sobre cómo crear un componente de personalizado
1. Aprenda a encapsular la lógica empresarial con los modelos Sling
1. Obtenga información sobre cómo utilizar un modelo Sling desde un script HTL

## Lo que va a generar {#what-build}

En esta parte del tutorial de WKND, se crea un componente Byline que se utiliza para mostrar información creada acerca del colaborador de un artículo.

![ejemplo de componente de firma](assets/custom-component/byline-design.png)

*Componente Firma*

La implementación del componente Firma incluye un cuadro de diálogo que recopila el contenido de la firma y un Modelo Sling personalizado que recupera los detalles como:

* Nombre
* Imagen
* Ocupaciones

## Crear componente Firma {#create-byline-component}

En primer lugar, cree la estructura de nodos Componente de firma y defina un cuadro de diálogo. AEM Representa el componente en el que se define de forma implícita el tipo de recurso del componente y lo define de manera implícita mediante su ubicación en el JCR.

El cuadro de diálogo expone la interfaz que los autores de contenido pueden proporcionar. AEM Para esta implementación, el componente principal de WCM de la **Imagen** se utiliza para gestionar la creación y el procesamiento de la imagen del divisor, por lo que debe configurarse como de este componente `sling:resourceSuperType`.

### Crear definición del componente {#create-component-definition}

1. En el **ui.apps** módulo, vaya a `/apps/wknd/components` y cree una carpeta denominada `byline`.
1. Dentro de `byline` carpeta, agregue un archivo llamado `.content.xml`

   ![diálogo para crear nodo](assets/custom-component/byline-node-creation.png)

1. Rellene el `.content.xml` archivo con lo siguiente:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   El archivo XML anterior proporciona la definición del componente, incluido el título, la descripción y el grupo. El `sling:resourceSuperType` apunta a `core/wcm/components/image/v2/image`, que es el [Componente Imagen principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=es).

### Creación del script HTL {#create-the-htl-script}

1. Dentro de `byline` carpeta, añadir un archivo `byline.html`, que es responsable de la presentación del HTML del componente. Es importante asignar el mismo nombre al archivo que a la carpeta, ya que se convierte en el script predeterminado que utiliza Sling para procesar este tipo de recurso.

1. Añada el siguiente código a `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

El `byline.html` es [se volvió a visitar más tarde](#byline-htl), una vez que se haya creado el modelo Sling. El estado actual del archivo HTL permite que el componente se muestre en estado vacío, en el Editor de páginas de AEM Sites cuando se arrastra y se coloca en la página.

### Creación de la definición de Diálogo {#create-the-dialog-definition}

A continuación, defina un cuadro de diálogo para el componente Firma con los siguientes campos:

* **Nombre**: un campo de texto con el nombre del colaborador.
* **Imagen**: una referencia a la biografía del colaborador.
* **Ocupaciones**: una lista de ocupaciones atribuidas al colaborador. Las ocupaciones deben ordenarse alfabéticamente en orden ascendente (de la a a la z).

1. Dentro de `byline` carpeta, cree una carpeta llamada `_cq_dialog`.
1. Dentro de `byline/_cq_dialog`, agregue un archivo con el nombre `.content.xml`. Esta es la definición XML del cuadro de diálogo. Agregue el siguiente XML:

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

   Estas definiciones de nodos de diálogo utilizan el [Fusión de recursos de Sling](https://sling.apache.org/documentation/bundles/resource-merger.html) para controlar qué pestañas de diálogo se heredan del `sling:resourceSuperType` componente, en este caso la variable **Componente de imagen de los componentes principales**.

   ![cuadro de diálogo completado para firma](assets/custom-component/byline-dialog-created.png)

### Creación del cuadro de diálogo Política {#create-the-policy-dialog}

Siguiendo el mismo enfoque que con la creación del cuadro de diálogo, cree un cuadro de diálogo de directiva (anteriormente conocido como cuadro de diálogo de diseño) para ocultar campos no deseados en la configuración de directiva heredada del componente de imagen de los componentes principales.

1. Dentro de `byline` carpeta, cree una carpeta llamada `_cq_design_dialog`.
1. Dentro de `byline/_cq_design_dialog`, cree un archivo llamado `.content.xml`. Actualice el archivo con lo siguiente: con el siguiente XML. Es más fácil abrir el `.content.xml` y copie/pegue el XML siguiente en él.

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

   La base de lo anterior **Cuadro de diálogo Política** XML se obtuvo del [Componente Imagen de componentes principales](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Como en la configuración de Diálogo, [Fusión de recursos de Sling](https://sling.apache.org/documentation/bundles/resource-merger.html) se utiliza para ocultar campos irrelevantes que de otra manera se heredan de `sling:resourceSuperType`, tal como lo ven las definiciones de nodo con `sling:hideResource="{Boolean}true"` propiedad.

### Implementación del código {#deploy-the-code}

1. Sincronización de los cambios en `ui.apps` con su IDE o con sus habilidades con Maven.

   ![AEM Exportar a componente de firma de servidor de](assets/custom-component/export-byline-component-aem.png)

## Añadir el componente a una página {#add-the-component-to-a-page}

AEM Para que las cosas sean sencillas y se centren en el desarrollo de componentes de la, vamos a agregar el componente Firma en su estado actual a una página de artículo para verificar la `cq:Component` la definición del nodo es correcta. AEM También para verificar que el usuario reconoce la nueva definición del componente y que el cuadro de diálogo del componente funciona durante la creación.

### Añadir una imagen a AEM Assets

En primer lugar, cargue una captura de cabeza de muestra en AEM Assets para utilizarla para rellenar la imagen en el componente Firma.

1. Vaya a la carpeta LA Skateparks de AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Cargue la toma de cabeza para  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** a la carpeta.

   ![Captura de encabezado cargada en AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Crear el componente {#author-the-component}

AEM A continuación, añada el componente Firma a una página de en la. Debido a que el componente Firma se agrega al **Proyecto de sitios WKND: contenido** Grupo de componentes, a través de `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` definición, está disponible automáticamente para cualquier **Contenedor** cuya **Política** permite el **Proyecto de sitios WKND: contenido** grupo de componentes. Por lo tanto, está disponible en el contenedor de diseño de la página de artículo

1. Vaya al artículo de LA Skatepark en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. En la barra lateral izquierda, arrastre y suelte una **Componente Firma** en a **bottom** del contenedor de diseño de la página de artículo abierta.

   ![añadir componente de firma a la página](assets/custom-component/add-to-page.png)

1. Asegúrese de que la barra lateral izquierda esté abierta **y visible, y la variable** Se ha seleccionado Buscador ** recursos.

1. Seleccione el **Marcador de posición del componente Firma**, que a su vez muestra la barra de acciones y pulsa el botón **llave** para abrir el cuadro de diálogo.

1. Con el cuadro de diálogo abierto y la primera pestaña (Recurso) activa, abra la barra lateral izquierda y, desde el buscador de recursos, arrastre una imagen a la zona desplegable Imagen. Busque &quot;stacey&quot; para encontrar la biografía de Stacey Roswells proporcionada en el paquete ui.content de WKND.

   ![añadir imagen al cuadro de diálogo](assets/custom-component/add-image.png)

1. Después de añadir una imagen, haga clic en **Propiedades** para introducir la variable **Nombre** y **Ocupaciones**.

   Al introducir ocupaciones, introdúzcalas en **alfabético inverso** ordene para que se verifique la lógica empresarial de alfabetización implementada en el modelo Sling.

   Pulse el botón **Listo** en la parte inferior derecha para guardar los cambios.

   ![rellenar propiedades del componente de firma](assets/custom-component/add-properties.png)

   AEM Los autores de la configuran y crean componentes a través de los cuadros de diálogo. En este punto, en el desarrollo del componente Firma, se incluyen los cuadros de diálogo para recopilar los datos, pero aún no se ha añadido la lógica para procesar el contenido creado. Por lo tanto, solo se muestra el marcador de posición.

1. Después de guardar el cuadro de diálogo, vaya a [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) AEM y revise cómo se almacena el contenido del componente en el nodo de contenido del componente Firma en la página de la página de.

   Busque el nodo de contenido del componente Firma debajo de la página Parques de patinaje LA, por ejemplo `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Observe los nombres de las propiedades `name`, `occupations`, y `fileReference` se almacenan en **nodo de firma**.

   Además, observe la `sling:resourceType` del nodo se ha definido en `wknd/components/content/byline` que es lo que vincula este nodo de contenido a la implementación del componente Firma.

   ![propiedades de firma en CRXDE](assets/custom-component/byline-properties-crxde.png)

## Crear modelo Sling de firma {#create-sling-model}

A continuación, vamos a crear un Modelo Sling para que actúe como modelo de datos y aloje la lógica empresarial del componente Firma.

AEM Los modelos Sling son objetos Java™ POJO (Java™ antiguos sin formato) impulsados por anotaciones que facilitan la asignación de datos desde el JCR a las variables Java™ y proporcionan eficacia al desarrollarse en el contexto de la.

### Revisar dependencias de Maven {#maven-dependency}

AEM El modelo Sling de firma se basa en varias API de Java™ proporcionadas por el usuario de. Estas API están disponibles a través de la `dependencies` enumerados en la `core` archivo POM del módulo. AEM El proyecto utilizado para este tutorial se ha creado para el as a Cloud Service de la. AEM Sin embargo, es único, ya que es compatible con versiones anteriores, con la versión 6.5/6.4, que es compatible con la versión anterior de la aplicación. Por lo tanto, se incluyen ambas dependencias para Cloud Service AEM y 6.x.

1. Abra el `pom.xml` archivo debajo `<src>/aem-guides-wknd/core/pom.xml`.
1. Buscar la dependencia de `aem-sdk-api` - **AEM Solo as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   El [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) AEM contiene todas las API de Java™ públicas expuestas por los usuarios de la aplicación. El `aem-sdk-api` se utiliza de forma predeterminada al crear este proyecto. La versión se mantiene en el pom del reactor principal desde la raíz del proyecto en `aem-guides-wknd/pom.xml`.

1. Busque la dependencia para `uber-jar` - **AEM Solo 6.5/6.4 de la**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   El `uber-jar` solo se incluye cuando la variable `classic` se invoca el perfil, es decir, `mvn clean install -PautoInstallSinglePackage -Pclassic`. De nuevo, esto es exclusivo de este proyecto. AEM En un proyecto del mundo real, generado a partir del archivo del proyecto de, escriba el `uber-jar` AEM es la versión predeterminada si la versión de la versión especificada es 6.5 o 6.4.

   El [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) AEM contiene todas las API de Java™ públicas expuestas por la versión 6.x de la aplicación de. La versión se mantiene en el pom del reactor principal desde la raíz del proyecto `aem-guides-wknd/pom.xml`.

1. Buscar la dependencia de `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   AEM Estas son las API públicas de Java™ completas expuestas por los componentes principales de la. AEM AEM Los componentes principales son un proyecto que se mantiene fuera de la aplicación y, por lo tanto, tiene un ciclo de lanzamiento independiente. Por este motivo, es una dependencia que debe incluirse por separado y es **no** incluido con el `uber-jar` o `aem-sdk-api`.

   Al igual que uber-jar, la versión de esta dependencia se mantiene en el archivo pom del reactor principal de `aem-guides-wknd/pom.xml`.

   Más adelante en este tutorial, la clase de imagen del componente principal se utiliza para mostrar la imagen en el componente Firma. Es necesario tener la dependencia del componente principal para generar y compilar el modelo Sling.

### Interfaz de firma {#byline-interface}

Cree una interfaz Java™ pública para el Firma. El `Byline.java` define los métodos públicos necesarios para dirigir el `byline.html` Script HTL.

1. En el interior, la `core` dentro del módulo `core/src/main/java/com/adobe/aem/guides/wknd/core/models` carpeta cree un archivo llamado `Byline.java`

   ![crear interfaz de firma](assets/custom-component/create-byline-interface.png)

1. Actualizar `Byline.java` con los métodos siguientes:

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

   Los dos primeros métodos exponen los valores de **name** y **oficios** para el componente Firma.

   El `isEmpty()` se utiliza para determinar si el componente tiene contenido para procesar o si está esperando a configurarse.

   Observe que no hay ningún método para la imagen; [esto se revisa más tarde](#tackling-the-image-problem).

1. Los paquetes Java™ que contienen clases Java™ públicas, en este caso un modelo Sling, deben versiones con el  `package-info.java` archivo.

   Desde el paquete Java™ de la fuente WKND `com.adobe.aem.guides.wknd.core.models` declara la versión de `1.0.0`, y se están agregando una interfaz pública y métodos que no provoquen interrupciones, la versión debe aumentarse a `1.1.0`. Abra el archivo en `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` y actualizar `@Version("1.0.0")` hasta `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

Cada vez que se realicen cambios en los archivos de este paquete, la variable [la versión del paquete debe ajustarse semánticamente](https://semver.org/). Si no, el proyecto de Maven [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) detecta una versión de paquete no válida y rompe la compilación. Afortunadamente, en caso de error, el complemento Maven informa de la versión del paquete Java™ no válida y de la versión que debería ser. Actualice el `@Version("...")` declaración en el paquete Java™ que infringe `package-info.java` a la versión recomendada por el complemento para corregirla.

### Implementación de firma {#byline-implementation}

El `BylineImpl.java` es la implementación del modelo Sling que implementa el `Byline.java` interfaz definida anteriormente. El código completo de `BylineImpl.java` se puede encontrar en la parte inferior de esta sección.

1. Cree una carpeta llamada `impl` debajo `core/src/main/java/com/adobe/aem/guides/core/models`.
1. En el `impl` carpeta, crear un archivo `BylineImpl.java`.

   ![Archivo Impl De Firma](assets/custom-component/byline-impl-file.png)

1. Abra `BylineImpl.java`. Especifique que implementa la variable `Byline` interfaz. Utilice las características de autocompletar del IDE o actualice manualmente el archivo para incluir los métodos necesarios para implementar el `Byline` interfaz:

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

1. Añadir las anotaciones del modelo Sling al actualizar `BylineImpl.java` con las siguientes anotaciones de nivel de clase. Esta `@Model(..)`La anotación es lo que convierte la clase en un modelo Sling.

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
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   Revisemos esta anotación y sus parámetros:

   * El `@Model` AEM La anotación registra BylineImpl como un modelo Sling cuando se implementa en el entorno de trabajo de la interfaz de usuario de.
   * El `adaptables` especifica que la solicitud puede adaptar este modelo.
   * El `adapters` permite registrar la clase de implementación en la interfaz Byline. Esto permite que el script HTL llame al modelo Sling a través de la interfaz (en lugar de la implementación directamente). [Encontrará más detalles sobre adaptadores aquí](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * El `resourceType` apunta al tipo de recurso del componente Firma (creado anteriormente) y ayuda a resolver el modelo correcto si hay varias implementaciones. [Encontrará más detalles sobre la asociación de una clase de modelo con un tipo de recurso aquí](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementación de los métodos del modelo Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

El primer método que se implementa es `getName()`, simplemente devuelve el valor almacenado en el nodo de contenido JCR de la línea bajo la propiedad `name`.

Para ello, la variable `@ValueMapValue` La anotación del modelo Sling se utiliza para insertar el valor en un campo Java™ mediante el ValueMap de recursos de la solicitud.


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

Dado que la propiedad JCR comparte el nombre como campo Java™ (ambos son &quot;name&quot;), `@ValueMapValue` resuelve automáticamente esta asociación e inserta el valor de la propiedad en el campo Java™.

#### getOccupations() {#implementing-get-occupations}

El siguiente método para implementar es `getOccupations()`. Este método carga las ocupaciones almacenadas en la propiedad JCR `occupations` y devuelven una colección ordenada (alfabéticamente) de ellos.

Con la misma técnica explorada en `getName()` el valor de la propiedad se puede insertar en el campo del modelo Sling.

Una vez que los valores de propiedad JCR estén disponibles en el modelo Sling a través del campo Java™ insertado `occupations`, la lógica empresarial de ordenación se puede aplicar en la variable `getOccupations()` método.


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

El último método público es `isEmpty()` que determina cuándo el componente debe considerarse &quot;lo suficientemente creado&quot; como para procesarse.

Para este componente, el requisito empresarial es los tres campos, `name, image and occupations` debe rellenarse *antes* el componente se puede representar.


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


#### Abordando el &quot;problema de la imagen&quot; {#tackling-the-image-problem}

Comprobar el nombre y las condiciones de ocupación son triviales y el Apache Commons Lang3 proporciona lo útil [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) clase. Sin embargo, no está claro cómo **presencia de la imagen** se puede validar porque el componente de imagen de los componentes principales se utiliza para mostrar la imagen.

Hay dos maneras de abordar esto:

Compruebe si la variable `fileReference` La propiedad JCR se resuelve en un recurso. *O* Convierta este recurso en un modelo Sling de imagen de componente principal y asegúrese de que `getSrc()` El método no está vacío.

Vamos a usar el **segundo** enfoque. El primer enfoque es probablemente suficiente, pero en este tutorial se utiliza el último para permitirnos explorar otras características de los modelos Sling.

1. Cree un método privado que obtenga la imagen. Este método se deja privado porque no es necesario exponer el objeto Image en el propio HTL y solo se utiliza para dirigir `isEmpty().`

   Añada el siguiente método privado para `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Como se ha indicado anteriormente, existen dos enfoques más para obtener la **Modelo Sling de imagen**:

   El primero utiliza el `@Self` para adaptar automáticamente la solicitud actual a la configuración del componente principal. `Image.class`

   El segundo usa el [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) El servicio OSGi, que es un servicio práctico y nos ayuda a crear modelos Sling de otro tipo en código Java™.

   Usemos el segundo enfoque.

   >[!NOTE]
   >
   >En una implementación del mundo real, enfoque &quot;Uno&quot;, utilizando `@Self` se prefiere ya que es la solución más simple y elegante. En este tutorial se utiliza el segundo enfoque, ya que requiere explorar más facetas de los modelos Sling que son útiles en componentes más complejos.

   Dado que los modelos Sling son Java™ POJO y no OSGi Services, las anotaciones de inyección OSGi habituales `@Reference` **no puede** se utilicen, en su lugar los modelos Sling proporcionan un **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** anotación que proporciona una funcionalidad similar.

1. Actualizar `BylineImpl.java` para incluir el `OSGiService` anotación para insertar el `ModelFactory`:

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

   Con el `ModelFactory` disponible, se puede crear un modelo de Sling de imagen de componente principal con:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Sin embargo, este método requiere una solicitud y un recurso, que aún no están disponibles en el modelo Sling. Para obtener estos, se utilizan más anotaciones del Modelo Sling.

   Para obtener la solicitud actual, **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** anotación se puede utilizar para insertar el `adaptable` (que se define en la `@Model(..)` as `SlingHttpServletRequest.class`, en un campo de clase Java™.

1. Añada el **@Self** anotación para obtener el **Solicitud SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Recuerde, usar `@Self Image image` para insertar el modelo de Sling de imagen del componente principal era una opción anterior: `@Self` La anotación intenta insertar el objeto adaptable (en este caso un SlingHttpServletRequest) y adaptarse al tipo de campo de anotación. Dado que el modelo Sling de imagen del componente principal se puede adaptar desde los objetos SlingHttpServletRequest, esto habría funcionado y es menos código que más exploratorio `modelFactory` enfoque.

   Ahora se insertan las variables necesarias para crear una instancia del modelo de imagen mediante la API ModelFactory. Usemos el modelo de Sling **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** anotación para obtener este objeto después de que el modelo Sling cree una instancia.

   `@PostConstruct` es increíblemente útil y actúa en una capacidad similar a la de un constructor; sin embargo, se invoca después de crear una instancia de la clase y de insertar todos los campos Java™ anotados. Mientras que otras anotaciones del modelo Sling anotan campos de clase Java™ (variables), `@PostConstruct` anota un método de parámetro void, zero, normalmente denominado `init()` (pero se puede nombrar cualquier cosa).

1. Añadir **@PostConstruct** método:

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

   Recuerde, los modelos Sling son **NO** Servicios OSGi, por lo que es seguro mantener el estado de clase. Frecuentemente `@PostConstruct` deriva y configura el estado de clase del modelo Sling para su uso posterior, de forma similar a lo que hace un constructor sin formato.

   Si la variable `@PostConstruct` El método genera una excepción, el modelo Sling no se crea una instancia y es nulo.

1. **getImage()** ahora se puede actualizar para devolver simplemente el objeto de imagen.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Vamos a volver a `isEmpty()` y finalice la implementación:

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

   Anote varias llamadas a `getImage()` no presenta problemas ya que devuelve el `image` variable de clase y no invoca `modelFactory.getModelFromWrappedRequest(...)` que no es demasiado costoso, pero vale la pena evitar llamar innecesariamente.

1. La final `BylineImpl.java` debería tener un aspecto similar al siguiente:


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
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
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

En el `ui.apps` módulo, abrir `/apps/wknd/components/byline/byline.html` AEM que se creó en la configuración anterior del componente de la.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Revisemos lo que hace esta secuencia de comandos HTL hasta ahora:

* El `placeholderTemplate` señala al marcador de posición de los componentes principales, que se muestra cuando el componente no está completamente configurado. Esto se representa en el Editor de páginas de AEM Sites como un cuadro con el título del componente, tal como se ha definido anteriormente en la `cq:Component`de  `jcr:title` propiedad.

* El `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carga el `placeholderTemplate` definido anteriormente y pasa un valor booleano (actualmente codificado en `false`) en la plantilla de marcador de posición. Cuándo `isEmpty` Si es true, la plantilla de marcador de posición procesa el cuadro gris; de lo contrario, no procesa nada.

### Actualizar firma HTL

1. Actualizar **byline.html** con la siguiente estructura de HTML esquelético:

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Tenga en cuenta que las clases CSS siguen el [Convenciones de nomenclatura de BEM](https://getbem.com/naming/). Aunque el uso de convenciones de BEM no es obligatorio, se recomienda utilizar BEM porque se utiliza en clases CSS de componentes principales y, por lo general, genera reglas CSS limpias y legibles.

### Creación de instancias de objetos del modelo Sling en HTL {#instantiating-sling-model-objects-in-htl}

El [Usar instrucción de bloque](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) se utiliza para crear una instancia de los objetos del modelo Sling en la secuencia de comandos HTL y asignarlos a una variable HTL.

El `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utiliza la interfaz Byline (com.adobe.aem.guides.wknd.models.Byline) implementada por BylineImpl y adapta a ella la SlingHttpServletRequest actual, y el resultado se almacena en una línea de nombre de variable HTL ( `data-sly-use.<variable-name>`).

1. Actualizar el externo `div` para hacer referencia al **Firma** Modelo Sling por su interfaz pública:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Acceder a métodos del modelo Sling {#accessing-sling-model-methods}

HTL toma prestado de JSTL y utiliza el mismo acortamiento de los nombres de los métodos de captador Java™.

Por ejemplo, invocar el modelo Sling de firma `getName()` El método se puede abreviar como `byline.name`, de forma similar en lugar de `byline.isEmpty`, se puede abreviar como `byline.empty`. Uso de nombres de método completos, `byline.getName` o `byline.isEmpty`, también funciona. Tenga en cuenta `()` nunca se utilizan para invocar métodos en HTL (similar a JSTL).

Métodos Java™ que requieren un parámetro **no puede** se utilizará en HTL. Esto es por diseño para mantener la lógica en HTL simple.

1. El nombre de la línea de firma se puede añadir al componente invocando la variable `getName()` en el modelo Byline Sling o en HTL: `${byline.name}`.

   Actualice el `h2` etiqueta:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Uso de opciones de expresión HTL {#using-htl-expression-options}

[Opciones de expresiones HTL](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) actúan como modificadores del contenido en HTL y van desde el formato de fecha a la traducción i18n. Las expresiones también se pueden utilizar para unir listas o matrices de valores, que son lo que se necesita para mostrar las ocupaciones en un formato delimitado por comas.

Las expresiones se añaden mediante la variable `@` en la expresión HTL.

1. Para unirse a la lista de ocupaciones con &quot;, &quot;, se utiliza el siguiente código:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Visualización condicional del marcador de posición {#conditionally-displaying-the-placeholder}

AEM La mayoría de los scripts HTL para componentes de utilizan el **paradigma de marcador de posición** para proporcionar una pista visual a los autores **que indica que un componente se ha creado incorrectamente y no se muestra en AEM Publish**. La convención para dirigir esta decisión es implementar un método en el modelo Sling de respaldo del componente, en este caso: `Byline.isEmpty()`.

El `isEmpty()` El método se invoca en el modelo Sling de firma y el resultado (o mejor dicho, es negativo, a través de la variable `!` operador) se guarda en una variable HTL llamada `hasContent`:

1. Actualizar el externo `div` para guardar una variable HTL llamada `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Observe el uso de `data-sly-test`, el HTL `test` El bloque de es clave, establece una variable HTL y procesa/no procesa el elemento HTML en el que está. Se basa en el resultado de la evaluación de expresiones HTL. Si es &quot;true&quot;, el elemento HTML se procesa; de lo contrario, no se procesa.

   Esta variable de HTL `hasContent` ahora se puede reutilizar para mostrar u ocultar condicionalmente el marcador de posición.

1. Actualice la llamada condicional a `placeholderTemplate` al final del archivo con lo siguiente:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Mostrar la imagen mediante los componentes principales {#using-the-core-components-image}

El script HTL para `byline.html` ahora está casi completa y solo falta la imagen.

Como el `sling:resourceSuperType` señala al componente de imagen del componente principal para crear la imagen; el componente de imagen del componente principal se puede utilizar para procesar la imagen.

Para ello, vamos a incluir el recurso de firma actual, pero forzar el tipo de recurso del componente de imagen del componente principal, utilizando el tipo de recurso `core/wcm/components/image/v2/image`. Se trata de un patrón potente para la reutilización de componentes. Para esto, HTL es `data-sly-resource` se utiliza el bloque de.

1. Reemplace el `div` con una clase de `cmp-byline__image` con lo siguiente:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Esta `data-sly-resource`, incluye el recurso actual mediante la ruta relativa `'.'`y fuerza la inclusión del recurso actual (o el recurso de contenido de firma) con el tipo de recurso de `core/wcm/components/image/v2/image`.

   El tipo de recurso del componente principal se utiliza directamente, y no a través de un proxy, porque es un uso dentro del script y nunca persiste en el contenido.

2. Completado `byline.html` a continuación:

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

3. AEM Implemente el código base en una instancia de local. Desde que se realizaron cambios en `core` y `ui.apps` es necesario implementar ambos módulos.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   AEM Para implementar en la versión 6.5/6.4 de, invoque el `classic` perfil:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > También puede generar todo el proyecto desde la raíz utilizando el perfil de Maven `autoInstallSinglePackage` pero esto puede sobrescribir los cambios de contenido en la página. Esto se debe a que `ui.content/src/main/content/META-INF/vault/filter.xml` AEM se ha modificado para el código de inicio del tutorial con el fin de sobrescribir claramente el contenido existente de la. En un escenario real, esto no es un problema.

### Revisión del componente Firma sin estilo {#reviewing-the-unstyled-byline-component}

1. Después de implementar la actualización, vaya a [Guía definitiva de LA Skateparks ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) , o donde haya añadido el componente Firma anteriormente en el capítulo.

1. El **imagen**, **name**, y **oficios** ahora aparecen y aparece un componente sin estilo, pero funcionando como Byline.

   ![componente de firma sin estilo](assets/custom-component/unstyled.png)

### Revisión del registro del modelo Sling {#reviewing-the-sling-model-registration}

El [AEM Vista de estado de los modelos Sling de la consola web](http://localhost:4502/system/console/status-slingmodels) AEM muestra todos los modelos Sling registrados en la lista de modelos de. El modelo Sling de firma puede validarse como instalado y reconocido revisando esta lista.

Si la variable **BylineImpl** no se muestra en esta lista, probablemente sea un problema con las anotaciones del modelo Sling o el modelo no se agregó al paquete correcto (`com.adobe.aem.guides.wknd.core.models`) en el proyecto principal.

![Modelo Sling de firma registrado](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## Estilos de firma {#byline-styles}

Para alinear el componente Firma con el diseño creativo proporcionado, vamos a aplicarle estilo. Esto se logra utilizando el archivo SCSS y actualizando el archivo en la **ui.frontend** módulo.

### Añadir un estilo predeterminado

Agregue estilos predeterminados para el componente Firma.

1. Vuelva al IDE y al **ui.frontend** proyecto en `/src/main/webpack/components`:
1. Cree un archivo con el nombre `_byline.scss`.

   ![firma del explorador del proyecto](assets/custom-component/byline-style-project-explorer.png)

1. Añada el CSS de implementaciones de firma (escrito como SCSS) en `_byline.scss`:

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

1. Abra un terminal y navegue hasta el `ui.frontend` módulo.
1. Inicie el `watch` procese con el siguiente comando npm:

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Vuelva al explorador y vaya a. [Artículo de LA SkateParks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Debería ver los estilos actualizados del componente.

   ![componente de firma finalizada](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Es posible que tenga que borrar la caché del explorador para asegurarse de que no se proporciona CSS obsoleto y actualizar la página con el componente Firma para obtener el estilo completo.

## Enhorabuena. {#congratulations}

¡Ha creado un componente personalizado desde cero con Adobe Experience Manager!

### Pasos siguientes {#next-steps}

AEM Continúe aprendiendo sobre el desarrollo de componentes de explorando cómo escribir pruebas JUnit para el código Java™ de Byline para garantizar que todo se desarrolle correctamente y que la lógica empresarial implementada sea correcta y completa.

* [AEM Escribir pruebas unitarias o componentes de la unidad de](unit-testing.md)

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/custom-component-solution`.

1. Clonar el [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repositorio.
1. Consulte la `tutorial/custom-component-solution` ramificación

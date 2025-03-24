---
title: Componente personalizado
description: Abarca la creación de extremo a extremo de un componente de firma personalizado que muestra el contenido creado. Incluye el desarrollo de un modelo Sling para encapsular la lógica empresarial y rellenar el componente de firma y el HTL correspondiente para procesar el componente.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
duration: 1039
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '3869'
ht-degree: 0%

---

# Componente personalizado {#custom-component}

Este tutorial cubre la creación de extremo a extremo de un componente AEM `Byline` personalizado que muestra el contenido creado en un cuadro de diálogo, y explora el desarrollo de un modelo Sling para encapsular la lógica empresarial que rellena el HTL del componente.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la rama `tutorial/custom-component-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. Implemente una base de código en una instancia de AEM local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) o desprotegerlo localmente cambiando a la rama `tutorial/custom-component-solution`.

## Objetivo

1. Obtenga información sobre cómo crear un componente de AEM personalizado
1. Aprenda a encapsular la lógica empresarial con los modelos Sling
1. Obtenga información sobre cómo utilizar un modelo Sling desde un script HTL

## Lo que va a generar {#what-build}

En esta parte del tutorial de WKND, se crea un componente Byline que se utiliza para mostrar información creada acerca del colaborador de un artículo.

![ejemplo de componente de firma](assets/custom-component/byline-design.png)

*Componente de firma*

La implementación del componente Firma incluye un cuadro de diálogo que recopila el contenido de la firma y un Modelo Sling personalizado que recupera los detalles como:

* Nombre
* Imagen
* Ocupaciones

## Crear componente Firma {#create-byline-component}

En primer lugar, cree la estructura de nodos Componente de firma y defina un cuadro de diálogo. Representa el componente en AEM y define implícitamente el tipo de recurso del componente por su ubicación en el JCR.

El cuadro de diálogo expone la interfaz que los autores de contenido pueden proporcionar. Para esta implementación, el componente principal **Image** del WCM de AEM se usa para administrar la creación y el procesamiento de la imagen del Byline, por lo que debe establecerse como `sling:resourceSuperType` de este componente.

### Crear definición del componente {#create-component-definition}

1. En el módulo **ui.apps**, vaya a `/apps/wknd/components` y cree una carpeta llamada `byline`.
1. Dentro de la carpeta `byline`, agregue un archivo con el nombre `.content.xml`

   ![diálogo para crear nodo](assets/custom-component/byline-node-creation.png)

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

   El archivo XML anterior proporciona la definición del componente, incluido el título, la descripción y el grupo. `sling:resourceSuperType` señala a `core/wcm/components/image/v2/image`, que es el [componente de imagen principal](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=es).

### Creación del script HTL {#create-the-htl-script}

1. Dentro de la carpeta `byline`, agregue un archivo `byline.html`, que es responsable de la presentación HTML del componente. Es importante asignar el mismo nombre al archivo que a la carpeta, ya que se convierte en el script predeterminado que utiliza Sling para procesar este tipo de recurso.

1. Agregue el siguiente código a `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` se [vuelve a visitar más tarde](#byline-htl), una vez que se crea el modelo Sling. El estado actual del archivo HTL permite que el componente se muestre en estado vacío, en el Editor de páginas de AEM Sites cuando se arrastra y se coloca en la página.

### Creación de la definición de Diálogo {#create-the-dialog-definition}

A continuación, defina un cuadro de diálogo para el componente Firma con los siguientes campos:

* **Nombre**: un campo de texto contiene el nombre del colaborador.
* **Imagen**: una referencia a la bioimagen del colaborador.
* **Ocupaciones**: una lista de ocupaciones atribuidas al colaborador. Las ocupaciones deben ordenarse alfabéticamente en orden ascendente (de la a a la z).

1. Dentro de la carpeta `byline`, cree una carpeta denominada `_cq_dialog`.
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

   Estas definiciones de nodos de diálogo utilizan la [fusión de recursos de Sling](https://sling.apache.org/documentation/bundles/resource-merger.html) para controlar qué pestañas de diálogo se heredan del componente `sling:resourceSuperType`, en este caso el componente de imagen de **componentes principales**.

   ![cuadro de diálogo completado para la firma](assets/custom-component/byline-dialog-created.png)

### Creación del cuadro de diálogo Política {#create-the-policy-dialog}

Siguiendo el mismo enfoque que con la creación del cuadro de diálogo, cree un cuadro de diálogo de directiva (anteriormente conocido como cuadro de diálogo de diseño) para ocultar campos no deseados en la configuración de directiva heredada del componente de imagen de los componentes principales.

1. Dentro de la carpeta `byline`, cree una carpeta denominada `_cq_design_dialog`.
1. Dentro de `byline/_cq_design_dialog`, cree un archivo denominado `.content.xml`. Actualice el archivo con lo siguiente: con el siguiente XML. Es más fácil abrir `.content.xml` y copiar/pegar el XML siguiente en él.

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

   La base del XML **diálogo de directivas** anterior se obtuvo del [componente de imagen de componentes principales](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Al igual que en la configuración del cuadro de diálogo, [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) se usa para ocultar campos irrelevantes que de otra manera se heredan de `sling:resourceSuperType`, tal como lo ven las definiciones de nodo con propiedad `sling:hideResource="{Boolean}true"`.

### Implementación del código {#deploy-the-code}

1. Sincronice los cambios de `ui.apps` con su IDE o con sus habilidades con Maven.

   ![Exportar al componente de firma del servidor de AEM](assets/custom-component/export-byline-component-aem.png)

## Añadir el componente a una página {#add-the-component-to-a-page}

Para que las cosas sean sencillas y se centren en el desarrollo de componentes de AEM, vamos a agregar el componente Byline en su estado actual a una página de artículo para comprobar que la definición del nodo `cq:Component` es correcta. Compruebe también que AEM reconoce la nueva definición del componente y que el cuadro de diálogo del componente funciona para la creación.

### Añadir una imagen a los AEM Assets

En primer lugar, cargue una captura de cabeza de muestra a los AEM Assets para utilizarla para rellenar la imagen en el componente Firma.

1. Vaya a la carpeta LA Skateparks en AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. Cargue la captura de pantalla de **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** en la carpeta.

   ![Captura de pantalla cargada a AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Crear el componente {#author-the-component}

A continuación, añada el componente Firma a una página de AEM. Dado que el componente Firma se agrega al grupo de componentes **Proyecto de sitios WKND - Contenido**, a través de la definición `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml`, estará disponible automáticamente para cualquier **contenedor** cuya **directiva** permita el grupo de componentes **Proyecto de sitios WKND - Contenido**. Por lo tanto, está disponible en el contenedor de diseño de la página de artículo

1. Vaya al artículo de LA Skatepark en: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Desde la barra lateral izquierda, arrastre y suelte un **componente Byline** en **bottom** del contenedor de diseño de la página de artículo abierta.

   ![agregar componente de firma a la página](assets/custom-component/add-to-page.png)

1. Asegúrese de que la barra lateral izquierda esté abierta **y visible y de que el** Buscador de recursos** esté seleccionado.

1. Seleccione el marcador de posición **Byline component**, que a su vez muestra la barra de acciones y pulse el icono **llave inglesa** para abrir el cuadro de diálogo.

1. Con el cuadro de diálogo abierto y la primera pestaña (Recurso) activa, abra la barra lateral izquierda y, desde el buscador de recursos, arrastre una imagen a la zona desplegable Imagen. Busque &quot;stacey&quot; para encontrar la biografía de Stacey Roswells proporcionada en el paquete ui.content de WKND.

   ![agregar imagen al cuadro de diálogo](assets/custom-component/add-image.png)

1. Después de agregar una imagen, haz clic en la ficha **Propiedades** para escribir **Nombre** y **Ocupaciones**.

   Al ingresar ocupaciones, ingréselas en **orden alfabético inverso**, de modo que se verifique la lógica empresarial de alfabetización implementada en el modelo Sling.

   Pulse el botón **Listo** en la parte inferior derecha para guardar los cambios.

   ![rellenar propiedades del componente de firma ](assets/custom-component/add-properties.png)

   Los autores de AEM configuran y crean componentes a través de los cuadros de diálogo. En este punto, en el desarrollo del componente Firma, se incluyen los cuadros de diálogo para recopilar los datos, pero aún no se ha añadido la lógica para procesar el contenido creado. Por lo tanto, solo se muestra el marcador de posición.

1. Después de guardar el cuadro de diálogo, vaya a [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) y revise cómo se almacena el contenido del componente en el nodo de contenido del componente de firma en la página de AEM.

   Busque el nodo de contenido del componente Firma debajo de la página Parques de patinaje LA, por ejemplo `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Observe que los nombres de propiedad `name`, `occupations` y `fileReference` están almacenados en el **nodo de firma**.

   Además, observe que `sling:resourceType` del nodo está establecido en `wknd/components/content/byline`, que es lo que enlaza este nodo de contenido a la implementación del componente Firma.

   ![propiedades de firma en CRXDE](assets/custom-component/byline-properties-crxde.png)

## Crear modelo Sling de firma {#create-sling-model}

A continuación, vamos a crear un Modelo Sling para que actúe como modelo de datos y aloje la lógica empresarial del componente Firma.

Los modelos Sling son objetos Java™ POJO (Java™ antiguos sin formato) impulsados por anotaciones que facilitan la asignación de datos desde el JCR a las variables Java™ y proporcionan eficacia al desarrollar en el contexto de AEM.

### Revisar dependencias de Maven {#maven-dependency}

El modelo Sling de firma se basa en varias API de Java™ proporcionadas por AEM. Estas API están disponibles a través de `dependencies` enumeradas en el archivo POM del módulo `core`. El proyecto utilizado para este tutorial se ha creado para AEM as a Cloud Service. Sin embargo, es único, ya que es compatible con versiones anteriores de AEM 6.5/6.4. Por lo tanto, se incluyen ambas dependencias para Cloud Service y AEM 6.x.

1. Abra el archivo de `pom.xml` debajo de `<src>/aem-guides-wknd/core/pom.xml`.
1. Buscar la dependencia de `aem-sdk-api` - **Solo AEM as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) contiene todas las API de Java™ públicas expuestas por AEM. `aem-sdk-api` se usa de forma predeterminada al crear este proyecto. La versión se mantiene en el pom del reactor principal desde la raíz del proyecto en `aem-guides-wknd/pom.xml`.

1. Busque la dependencia para `uber-jar` - **Solo AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   `uber-jar` solo se incluye cuando se invoca el perfil `classic`, es decir `mvn clean install -PautoInstallSinglePackage -Pclassic`. De nuevo, esto es exclusivo de este proyecto. En un proyecto real, generado a partir del tipo de archivo del proyecto de AEM, `uber-jar` es la versión predeterminada si la versión de AEM especificada es 6.5 o 6.4.

   [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contiene todas las API de Java™ públicas expuestas por AEM 6.x. La versión se mantiene en el pom del reactor principal desde la raíz del proyecto `aem-guides-wknd/pom.xml`.

1. Buscar la dependencia de `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Estas son las API públicas de Java™ completas expuestas por los componentes principales de AEM. Los componentes principales de AEM son un proyecto que se mantiene fuera de AEM y, por lo tanto, tiene un ciclo de lanzamiento independiente. Por este motivo, es una dependencia que debe incluirse por separado y que **no** se incluye con `uber-jar` o `aem-sdk-api`.

   Al igual que uber-jar, la versión de esta dependencia se mantiene en el archivo pom del reactor principal de `aem-guides-wknd/pom.xml`.

   Más adelante en este tutorial, la clase de imagen del componente principal se utiliza para mostrar la imagen en el componente Firma. Es necesario tener la dependencia del componente principal para generar y compilar el modelo Sling.

### Interfaz de firma {#byline-interface}

Cree una interfaz Java™ pública para el Firma. `Byline.java` define los métodos públicos necesarios para controlar el script HTL `byline.html`.

1. Dentro, el módulo `core` dentro de la carpeta `core/src/main/java/com/adobe/aem/guides/wknd/core/models` crea un archivo con el nombre `Byline.java`

   ![crear interfaz de firma](assets/custom-component/create-byline-interface.png)

1. Actualice `Byline.java` con los métodos siguientes:

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

   Los dos primeros métodos exponen los valores del **nombre** y de **ocupaciones** para el componente Byline.

   El método `isEmpty()` se usa para determinar si el componente tiene contenido para procesar o si está a la espera de ser configurado.

   Observe que no hay ningún método para la imagen; [esto se revisará más adelante](#tackling-the-image-problem).

1. Los paquetes Java™ que contienen clases Java™ públicas, en este caso un modelo Sling, deben tener versiones con el archivo `package-info.java` del paquete.

   Dado que el paquete Java™ `com.adobe.aem.guides.wknd.core.models` del origen WKND declara la versión de `1.0.0`, y se están agregando una interfaz pública de no separación y métodos, la versión debe aumentarse a `1.1.0`. Abra el archivo en `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` y actualice `@Version("1.0.0")` a `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

Siempre que se realicen cambios en los archivos de este paquete, la [versión del paquete debe ajustarse semánticamente](https://semver.org/). Si no es así, [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd) del proyecto Maven detecta una versión de paquete no válida y rompe el complemento generado. Afortunadamente, en caso de error, el complemento Maven informa de la versión del paquete Java™ no válida y de la versión que debería ser. Actualice la declaración `@Version("...")` en el paquete Java™ que infringe `package-info.java` a la versión recomendada por el complemento para corregirla.

### Implementación de firma {#byline-implementation}

`BylineImpl.java` es la implementación del modelo Sling que implementa la interfaz `Byline.java` definida anteriormente. El código completo de `BylineImpl.java` se encuentra al final de esta sección.

1. Cree una carpeta denominada `impl` debajo de `core/src/main/java/com/adobe/aem/guides/core/models`.
1. En la carpeta `impl`, cree un archivo `BylineImpl.java`.

   ![Archivo Impl De Firma](assets/custom-component/byline-impl-file.png)

1. Abra `BylineImpl.java`. Especifique que implementa la interfaz `Byline`. Utilice las características de autocompletar del IDE o actualice manualmente el archivo para incluir los métodos necesarios para implementar la interfaz `Byline`:

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

1. Agregue las anotaciones del modelo Sling actualizando `BylineImpl.java` con las siguientes anotaciones de nivel de clase. Esta `@Model(..)`anotación es lo que convierte la clase en un modelo Sling.

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

   * La anotación `@Model` registra BylineImpl como modelo Sling cuando se implementa en AEM.
   * El parámetro `adaptables` especifica que la solicitud puede adaptar este modelo.
   * El parámetro `adapters` permite registrar la clase de implementación en la interfaz Byline. Esto permite que el script HTL llame al modelo Sling a través de la interfaz (en lugar de la implementación directamente). [Encontrará más detalles sobre adaptadores aquí](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * `resourceType` señala al tipo de recurso del componente Firma (creado anteriormente) y ayuda a resolver el modelo correcto si hay varias implementaciones. [Aquí puede encontrar más detalles acerca de cómo asociar una clase de modelo con un tipo de recurso](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementación de los métodos del modelo Sling {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

El primer método que se implementa es `getName()`, simplemente devuelve el valor almacenado en el nodo de contenido JCR de la firma bajo la propiedad `name`.

Para ello, se utiliza la anotación del modelo Sling `@ValueMapValue` para insertar el valor en un campo Java™ mediante el ValueMap de recursos de la solicitud.


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

Dado que la propiedad JCR comparte name como el campo Java™ (ambos son &quot;name&quot;), `@ValueMapValue` resuelve automáticamente esta asociación e inserta el valor de la propiedad en el campo Java™.

#### getOccupations() {#implementing-get-occupations}

El siguiente método para implementar es `getOccupations()`. Este método carga las ocupaciones almacenadas en la propiedad JCR `occupations` y devuelve una colección ordenada (alfabéticamente) de ellas.

Utilizando la misma técnica explorada en `getName()`, el valor de la propiedad se puede insertar en el campo del modelo Sling.

Una vez que los valores de propiedad JCR están disponibles en el modelo Sling a través del campo Java™ `occupations` insertado, la lógica empresarial de ordenación se puede aplicar en el método `getOccupations()`.


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

El último método público es `isEmpty()`, que determina cuándo el componente debe considerarse &quot;lo suficientemente creado&quot; como para procesarse.

Para este componente, el requisito empresarial son los tres campos, `name, image and occupations` debe rellenarse *antes de* que se pueda procesar el componente.


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

Comprobar las condiciones de nombre y ocupación es trivial y Apache Commons Lang3 proporciona la práctica clase [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html). Sin embargo, no está claro cómo se puede validar la presencia **de la imagen**, ya que el componente de imagen de componentes principales se utiliza para mostrar la imagen.

Hay dos maneras de abordar esto:

Compruebe si la propiedad JCR `fileReference` se resuelve en un recurso. *OR* Convierta este recurso en un modelo de Sling de imagen de componente principal y asegúrese de que el método `getSrc()` no esté vacío.

Usemos el enfoque de **segundo**. El primer enfoque es probablemente suficiente, pero en este tutorial se utiliza el último para permitirnos explorar otras características de los modelos Sling.

1. Cree un método privado que obtenga la imagen. Este método se deja privado porque no es necesario exponer el objeto Image en el propio HTL y solo se usa para dirigir `isEmpty().`

   Agregue el siguiente método privado para `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Como se mencionó anteriormente, existen dos enfoques más para obtener el **Modelo Sling de imagen**:

   La primera usa la anotación `@Self` para adaptar automáticamente la solicitud actual a `Image.class` del componente principal

   El segundo usa el servicio OSGi [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html), que es un servicio práctico y nos ayuda a crear modelos Sling de otros tipos en código Java™.

   Usemos el segundo enfoque.

   >[!NOTE]
   >
   >En una implementación real, se prefiere usar `@Self` para abordar el enfoque &quot;Uno&quot;, ya que es la solución más sencilla y elegante. En este tutorial se utiliza el segundo enfoque, ya que requiere explorar más facetas de los modelos Sling que son útiles en componentes más complejos.

   Dado que los modelos Sling son Java™ POJO y no OSGi Services, no se pueden usar las anotaciones de inyección OSGi habituales `@Reference` **no se pueden**; en su lugar, los modelos Sling proporcionan una anotación **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** especial que proporciona una funcionalidad similar.

1. Actualice `BylineImpl.java` para incluir la anotación `OSGiService` para insertar `ModelFactory`:

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

   Con `ModelFactory` disponible, se puede crear un modelo de Sling de imagen de componente principal mediante lo siguiente:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Sin embargo, este método requiere una solicitud y un recurso, que aún no están disponibles en el modelo Sling. Para obtener estos, se utilizan más anotaciones del Modelo Sling.

   Para obtener la solicitud actual, se puede usar la anotación **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** para insertar `adaptable` (que se define en `@Model(..)` como `SlingHttpServletRequest.class`, en un campo de clase Java™.

1. Agregue la anotación **@Self** para obtener la **solicitud SlingHttpServletRequest**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Recuerde, el uso de `@Self Image image` para insertar el componente principal del modelo de Sling de imagen era una opción anterior: la anotación `@Self` intenta insertar el objeto adaptable (en este caso, un SlingHttpServletRequest) y adaptarse al tipo de campo de anotación. Dado que el modelo Sling de imagen del componente principal se puede adaptar desde los objetos SlingHttpServletRequest, esto habría funcionado y es menos código que un enfoque `modelFactory` más exploratorio.

   Ahora se insertan las variables necesarias para crear una instancia del modelo de imagen mediante la API ModelFactory. Usemos la anotación **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** del modelo Sling para obtener este objeto después de que el modelo Sling cree una instancia.

   `@PostConstruct` es increíblemente útil y actúa en una capacidad similar a la de un constructor; sin embargo, se invoca después de crear una instancia de la clase y de insertar todos los campos Java™ anotados. Mientras que otras anotaciones del modelo Sling anotan campos de clase Java™ (variables), `@PostConstruct` anota un método de parámetro nulo, cero, generalmente denominado `init()` (pero se puede nombrar cualquier cosa).

1. Agregar método **@PostConstruct**:

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

   Recuerde, los modelos Sling son **NO** servicios OSGi, por lo que es seguro mantener el estado de clase. A menudo `@PostConstruct` deriva y configura el estado de clase del modelo Sling para su uso posterior, de forma similar a lo que hace un constructor sin formato.

   Si el método `@PostConstruct` genera una excepción, el modelo Sling no se crea una instancia y es nulo.

1. **getImage()** ahora se puede actualizar para devolver simplemente el objeto de imagen.

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

   Tenga en cuenta que las llamadas múltiples a `getImage()` no plantean problemas, ya que devuelve la variable de clase `image` inicializada y no invoca `modelFactory.getModelFromWrappedRequest(...)`, lo cual no es excesivamente costoso, pero vale la pena evitar llamar innecesariamente.

1. El(la) `BylineImpl.java` final debe tener el siguiente aspecto:


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

En el módulo `ui.apps`, abra `/apps/wknd/components/byline/byline.html` que se creó en la configuración anterior del componente AEM.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Revisemos lo que hace esta secuencia de comandos HTL hasta ahora:

* `placeholderTemplate` señala al marcador de posición de los componentes principales, que se muestra cuando el componente no está completamente configurado. Esto se representa en el Editor de páginas de AEM Sites como un cuadro con el título del componente, tal como se ha definido anteriormente en la propiedad `jcr:title` de `cq:Component`.

* `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` carga el elemento `placeholderTemplate` definido anteriormente y pasa un valor booleano (actualmente codificado para `false`) a la plantilla de marcador de posición. Cuando `isEmpty` es verdadero, la plantilla del marcador de posición representa el cuadro gris; de lo contrario, no representa nada.

### Actualizar firma HTL

1. Actualice **byline.html** con la siguiente estructura esquemática de HTML:

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

   Tenga en cuenta que las clases CSS siguen la [convención de nomenclatura de BEM](https://getbem.com/naming/). Aunque el uso de convenciones de BEM no es obligatorio, se recomienda utilizar BEM porque se utiliza en clases CSS de componentes principales y, por lo general, genera reglas CSS limpias y legibles.

### Creación de instancias de objetos del modelo Sling en HTL {#instantiating-sling-model-objects-in-htl}

[Use la instrucción de bloque](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) para crear una instancia de los objetos del modelo Sling en el script HTL y asignarla a una variable HTL.

El objeto `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` utiliza la interfaz Byline (com.adobe.aem.guides.wknd.models.Byline) implementada por BylineImpl y adapta el objeto SlingHttpServletRequest actual a la misma, y el resultado se almacena en una línea de nombre de variable HTL ( `data-sly-use.<variable-name>`).

1. Actualice el elemento externo `div` para que haga referencia al modelo Sling **Byline** por su interfaz pública:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Acceder a métodos del modelo Sling {#accessing-sling-model-methods}

HTL toma prestado de JSTL y utiliza el mismo acortamiento de los nombres de los métodos de captador Java™.

Por ejemplo, si invoca el método `getName()` del modelo Sling Byline, se puede acortar a `byline.name`; de manera similar, en lugar de a `byline.isEmpty`, se puede acortar a `byline.empty`. El uso de nombres de método completos, `byline.getName` o `byline.isEmpty`, también funciona. Tenga en cuenta que `()` nunca se utilizan para invocar métodos en HTL (similar a JSTL).

Los métodos Java™ que requieren un parámetro **no se pueden** usar en HTL. Esto es por diseño para mantener la lógica en HTL simple.

1. El nombre Byline se puede agregar al componente invocando el método `getName()` en el modelo Sling Byline o en HTL: `${byline.name}`.

   Actualizar la etiqueta `h2`:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### Uso de opciones de expresión HTL {#using-htl-expression-options}

[Las opciones de expresiones HTL](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) actúan como modificadores del contenido en HTL y van desde el formato de fecha hasta la traducción i18n. Las expresiones también se pueden utilizar para unir listas o matrices de valores, que son lo que se necesita para mostrar las ocupaciones en un formato delimitado por comas.

Las expresiones se agregan mediante el operador `@` en la expresión HTL.

1. Para unirse a la lista de ocupaciones con &quot;, &quot;, se utiliza el siguiente código:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### Visualización condicional del marcador de posición {#conditionally-displaying-the-placeholder}

La mayoría de los scripts HTL para componentes de AEM utilizan el **paradigma de marcador de posición** para proporcionar una pista visual a los autores **que indica que un componente se ha creado incorrectamente y que no se muestra en la publicación de AEM**. La convención para tomar esta decisión es implementar un método en el modelo Sling de respaldo del componente, en este caso: `Byline.isEmpty()`.

El método `isEmpty()` se invoca en el modelo Sling de firma y el resultado (o mejor dicho, es negativo, a través del operador `!`) se guarda en una variable HTL llamada `hasContent`:

1. Actualice el `div` externo para guardar una variable HTL llamada `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Tenga en cuenta el uso de `data-sly-test`, el bloque HTL `test` es clave, establece una variable HTL y procesa/no procesa el elemento HTML en el que está. Se basa en el resultado de la evaluación de expresiones HTL. Si es &quot;true&quot;, el elemento HTML se procesa; de lo contrario, no se procesa.

   Esta variable HTL `hasContent` ahora se puede reutilizar para mostrar/ocultar condicionalmente el marcador de posición.

1. Actualice la llamada condicional a `placeholderTemplate` en la parte inferior del archivo con lo siguiente:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### Mostrar la imagen mediante los componentes principales {#using-the-core-components-image}

El script HTL para `byline.html` está casi completo y solo falta la imagen.

A medida que `sling:resourceSuperType` señala al componente de imagen del componente principal para crear la imagen, se puede utilizar el componente de imagen del componente principal para procesar la imagen.

Para ello, vamos a incluir el recurso de firma actual, pero forzar el tipo de recurso del componente de imagen del componente principal, utilizando el tipo de recurso `core/wcm/components/image/v2/image`. Se trata de un patrón potente para la reutilización de componentes. Para ello, se utiliza el bloque `data-sly-resource` de HTL.

1. Reemplace `div` por una clase de `cmp-byline__image` con lo siguiente:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Este(a) `data-sly-resource`, incluye el recurso actual a través de la ruta relativa `'.'` y fuerza la inclusión del recurso actual (o el recurso de contenido de la firma) con el tipo de recurso de `core/wcm/components/image/v2/image`.

   El tipo de recurso del componente principal se utiliza directamente, y no a través de un proxy, porque es un uso dentro del script y nunca persiste en el contenido.

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

3. Implemente el código base en una instancia local de AEM. Debido a que se hicieron cambios en `core` y `ui.apps`, es necesario implementar ambos módulos.

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Para implementar en AEM 6.5/6.4, invoque el perfil `classic`:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > También puede generar todo el proyecto a partir de la raíz mediante el perfil de Maven `autoInstallSinglePackage`, pero esto puede sobrescribir los cambios de contenido en la página. Esto se debe a que `ui.content/src/main/content/META-INF/vault/filter.xml` se ha modificado para el código de inicio del tutorial a fin de sobrescribir sin problemas el contenido de AEM existente. En un escenario real, esto no es un problema.

### Revisión del componente Firma sin estilo {#reviewing-the-unstyled-byline-component}

1. Después de implementar la actualización, vaya a la página [Guía de Ultimate para parques de patinaje de Los Ángeles](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) o donde haya agregado el componente Firma anteriormente en el capítulo.

1. Ahora aparecen las **imágenes**, **nombres** y **ocupaciones**, con un componente sin estilo, pero que funciona como Byline.

   ![componente de firma sin estilo](assets/custom-component/unstyled.png)

### Revisión del registro del modelo Sling {#reviewing-the-sling-model-registration}

La [vista de estado de modelos Sling de la consola web de AEM](http://localhost:4502/system/console/status-slingmodels) muestra todos los modelos Sling registrados en AEM. El modelo Sling de firma puede validarse como instalado y reconocido revisando esta lista.

Si **BylineImpl** no se muestra en esta lista, es probable que haya un problema con las anotaciones del modelo Sling o que el modelo no se haya agregado al paquete correcto (`com.adobe.aem.guides.wknd.core.models`) en el proyecto principal.

![Modelo Sling de firma electrónica registrado](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## Estilos de firma {#byline-styles}

Para alinear el componente Firma con el diseño creativo proporcionado, vamos a aplicarle estilo. Esto se logra usando el archivo SCSS y actualizando el archivo en el módulo **ui.frontend**.

### Añadir un estilo predeterminado

Agregue estilos predeterminados para el componente Firma.

1. Vuelva al IDE y al proyecto **ui.frontend** en `/src/main/webpack/components`:
1. Cree un archivo con el nombre `_byline.scss`.

   ![explorador de proyecto de firma](assets/custom-component/byline-style-project-explorer.png)

1. Agregue CSS (escrito como SCSS) de implementaciones Byline a `_byline.scss`:

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

1. Abra un terminal y vaya al módulo `ui.frontend`.
1. Inicie el proceso `watch` con el siguiente comando npm:

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Vuelva al explorador y navegue hasta el [artículo de LA SkateParks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Debería ver los estilos actualizados del componente.

   ![componente de firma finalizada](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Es posible que tenga que borrar la caché del explorador para asegurarse de que no se proporciona CSS obsoleto y actualizar la página con el componente Firma para obtener el estilo completo.

## Enhorabuena. {#congratulations}

¡Ha creado un componente personalizado desde cero con Adobe Experience Manager!

### Pasos siguientes {#next-steps}

Continúe aprendiendo sobre el desarrollo de componentes de AEM explorando cómo escribir pruebas JUnit para el código Java™ de Byline para garantizar que todo se desarrolle correctamente y que la lógica empresarial implementada sea correcta y completa.

* [Escribir pruebas unitarias para componentes de AEM](unit-testing.md)

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/custom-component-solution`.

1. Clonar el repositorio [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Consulte la rama `tutorial/custom-component-solution`

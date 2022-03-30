---
title: 'Desarrollo con AEM Editor de SPA: Tutorial Hello World'
description: AEM SPA Editor es compatible con la edición en contexto de una aplicación o SPA de una sola página. Este tutorial es una introducción SPA desarrollo que se utilizará con AEM SDK de Editor JS. El tutorial ampliará la aplicación We.Retail Journal añadiendo un componente personalizado de Hello World. Los usuarios pueden completar el tutorial utilizando los marcos de React o Angular.
version: 6.3, 6.4, 6.5
topic: SPA
feature: SPA Editor
role: Developer
level: Beginner
exl-id: e900301d-411c-4c02-8443-2a0fa56b65b5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '3144'
ht-degree: 2%

---

# Desarrollo con AEM Editor de SPA: Tutorial Hello World {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Este tutorial es **obsoleto**. Se recomienda seguir: [Introducción al AEM SPA Editor y Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) o [Introducción al AEM SPA Editor y React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM SPA Editor es compatible con la edición en contexto de una aplicación o SPA de una sola página. Este tutorial es una introducción SPA desarrollo que se utilizará con AEM SDK de Editor JS. El tutorial ampliará la aplicación We.Retail Journal añadiendo un componente personalizado de Hello World. Los usuarios pueden completar el tutorial utilizando los marcos de React o Angular.

>[!NOTE]
>
> La función Editor de aplicaciones de una sola página (SPA) requiere AEM 6.4 service pack 2 o posterior.
>
> El Editor de SPA es la solución recomendada para proyectos que requieren SPA procesamiento del lado del cliente basado en el marco de trabajo (por ejemplo, React o Angular).

## Lectura de requisitos previos {#prereq}

Este tutorial pretende resaltar los pasos necesarios para asignar un componente SPA a un componente AEM para permitir la edición en contexto. Los usuarios que inicien este tutorial deben estar familiarizados con los conceptos básicos de desarrollo de Adobe Experience Manager, AEM, así como con el desarrollo de los marcos de React of Angular. El tutorial cubre tanto las tareas de desarrollo del back-end como del front-end.

Se recomienda revisar los siguientes recursos antes de iniciar este tutorial:

* [Vídeo de funciones del editor de SPA](spa-editor-framework-feature-video-use.md) : Vídeo con información general sobre la aplicación SPA Editor y We.Retail Journal.
* [Tutorial de React.js](https://reactjs.org/tutorial/tutorial.html) - Una introducción al desarrollo con el marco React.
* [Tutorial de angular](https://angular.io/tutorial) - Una introducción al desarrollo con Angular

## Entorno de desarrollo local {#local-dev}

Este tutorial está diseñado para:

[Adobe Experience Manager 6.5](https://helpx.adobe.com/es/experience-manager/6-5/release-notes.html) o [Adobe Experience Manager 6.4](https://helpx.adobe.com/es/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [Service Pack 5](https://helpx.adobe.com/es/experience-manager/6-4/release-notes/sp-release-notes.html)

En este tutorial se deben instalar las siguientes tecnologías y herramientas:

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) y npm 5.6.0+ (npm se instala con node.js)

Compruebe la instalación de las herramientas anteriores abriendo un terminal nuevo y ejecutando lo siguiente:

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## Información general {#overview}

El concepto básico es asignar un componente SPA a un componente AEM. AEM componentes, ejecutar en el lado del servidor, exportar contenido en forma de JSON. El contenido JSON lo consume la SPA, que ejecuta el lado del cliente en el explorador. Se crea una asignación 1:1 entre SPA componentes y un componente AEM.

![Asignación de componentes SPA](assets/spa-editor-helloworld-tutorial-use/mapto.png)

Marcos populares [React JS](https://reactjs.org/) y [Angular](https://angular.io/) son compatibles de forma predeterminada. Los usuarios pueden completar este tutorial en Angular o React, independientemente del marco con el que se sientan más cómodos.

## Configuración del proyecto {#project-setup}

SPA desarrollo tiene un pie en AEM desarrollo, y el otro fuera. El objetivo es permitir que SPA desarrollo se produzca de forma independiente, y (en su mayoría) no tenga sentido AEM.

* SPA proyectos pueden funcionar independientemente del proyecto de AEM durante el desarrollo del front-end.
* Herramientas y tecnologías de compilación front-end como Webpack, NPM, [!DNL Grunt] y [!DNL Gulp]se seguirá utilizando.
* Para compilar para AEM, el proyecto de SPA se compila y se incluye automáticamente en el proyecto de AEM.
* Paquetes AEM estándar utilizados para implementar el SPA en AEM.

![Resumen de artefactos e implementación](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA desarrollo tiene un pie en AEM desarrollo, y el otro fuera - permitiendo que el desarrollo de SPA ocurra de manera independiente, y (en su mayoría) agnóstico para AEM.*

El objetivo de este tutorial es ampliar la aplicación de diario We.Retail con un nuevo componente. Comience descargando el código fuente de la aplicación We.Retail Journal e implementando en una AEM local.

1. **Descargar** la última [Código de diario We.Retail de GitHub](https://github.com/adobe/aem-sample-we-retail-journal).

   O clone el repositorio desde la línea de comandos:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >El tutorial funcionará con la variable **maestro** ramificación con **1.2.1: INSTANTÁNEA** versión del proyecto.

1. La siguiente estructura debería ser visible:

   ![Estructura de carpetas del proyecto](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   El proyecto contiene los siguientes módulos maven:

   * `all`: Incrusta e instala todo el proyecto en un solo paquete.
   * `bundles`: Contiene dos paquetes OSGi: commons y core que contienen [!DNL Sling Models] y otro código Java.
   * `ui.apps`: contiene las partes /apps del proyecto, ie JS &amp; CSS clientlibs, componentes y configuraciones específicas del modo de ejecución.
   * `ui.content`: contiene contenido estructural y configuraciones (`/content`, `/conf`)
   * `react-app`: Aplicación We.Retail Journal React. Este es un módulo Maven y un proyecto de webpack.
   * `angular-app`: aplicación de Angular de diario We.Retail. Esto es a la vez un [!DNL Maven] y un proyecto de webpack.

1. Abra una nueva ventana de terminal y ejecute el siguiente comando para compilar e implementar toda la aplicación en una instancia de AEM local que se ejecute en [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > En este proyecto, el perfil de Maven para crear y empaquetar todo el proyecto es `autoInstallSinglePackage`

1. Vaya a:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   La aplicación de diario We.Retail debe mostrarse en el editor de AEM Sites.

1. En [!UICONTROL Editar] , seleccione un componente para editar y actualice el contenido.

   ![Editar un componente](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. Seleccione el [!UICONTROL Propiedades de página] para abrir [!UICONTROL Propiedades de página]. Select [!UICONTROL Editar plantilla] para abrir la plantilla de la página.

   ![Menú Propiedades de página](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. En la última versión del SPA Editor, [Plantillas editables](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/page-templates-editable.html) se puede usar del mismo modo que con las implementaciones tradicionales de Sites. Esto se volverá a examinar más adelante con nuestro componente personalizado.

   >[!NOTE]
   >
   > Solo AEM 6.5 y AEM 6.4 + **Service Pack 5** admiten plantillas editables.

## Información general sobre desarrollo {#development-overview}

![Desarrollo de la información general](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA iteraciones de desarrollo se producen independientemente del AEM. Cuando el SPA está listo para ser desplegado en AEM, se dan los siguientes pasos de alto nivel (como se ilustra más arriba).

1. Se invoca la compilación del proyecto AEM, que a su vez déclencheur una compilación del proyecto SPA. El diario We.Retail usa el [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. El proyecto SPA [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) incrusta el SPA compilado como una biblioteca de cliente de AEM en el proyecto de AEM.
1. El proyecto AEM genera un paquete de AEM, incluido el SPA compilado, además de cualquier otro código AEM compatible.

## Crear AEM componente {#aem-component}

**Persona: Desarrollador de AEM**

Primero se creará un componente AEM. El componente AEM es responsable de procesar las propiedades JSON leídas por el componente React. El componente AEM también es responsable de proporcionar un cuadro de diálogo para cualquier propiedad editable del componente.

Uso [!DNL Eclipse], u otros [!DNL IDE], importe el proyecto Maven de diario de We.Retail.

1. Actualizar el reactor **pom.xml** para quitar el [!DNL Apache Rat] plugin. Este complemento comprueba cada archivo para asegurarse de que hay un encabezado de licencia. Para nuestros propósitos no necesitamos preocuparnos por esta funcionalidad.

   En **aem-sample-we-retail-journal/pom.xml** remove **apache-rate-plugin**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. En el **we-retail-journal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) cree un nuevo nodo debajo de `ui.apps/jcr_root/apps/we-retail-journal/components` named **helloworld** de tipo **cq:Component**.
1. Agregue las siguientes propiedades al **helloworld** componente, representado en XML (`/helloworld/.content.xml`) a continuación:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Componente Hello World](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > Para ilustrar la función Plantillas editables, hemos establecido deliberadamente la variable `componentGroup="Custom Components"`. En un proyecto en el mundo real, es mejor minimizar el número de grupos de componentes, de modo que un grupo mejor sería &quot;[!DNL We.Retail Journal]&quot; para que coincida con los otros componentes de contenido.
   >
   > Solo AEM 6.5 y AEM 6.4 + **Service Pack 5** admiten plantillas editables.

1. A continuación, se creará un cuadro de diálogo para permitir que se configure un mensaje personalizado para el **Hello World** componente. Bajo `/apps/we-retail-journal/components/helloworld` agregar un nombre de nodo **cq:dialog** de **nt:unstructured**.
1. La variable **cq:dialog** mostrará un único campo de texto que mantiene el texto en una propiedad denominada **[!DNL message]**. Debajo de los recién creados **cq:dialog** agregue los siguientes nodos y propiedades, representados en XML a continuación (`helloworld/_cq_dialog/.content.xml`) :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
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
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
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

   ![estructura de archivos](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   La definición de nodo XML anterior creará un cuadro de diálogo con un único campo de texto que permitirá al usuario introducir un &quot;mensaje&quot;. Tenga en cuenta la propiedad `name="./message"` dentro de la variable `<message />` nodo . Este es el nombre de la propiedad que se almacenará en el JCR dentro de AEM.

1. A continuación, se creará un cuadro de diálogo de directiva vacío (`cq:design_dialog`). El cuadro de diálogo Política es necesario para ver el componente en el Editor de plantillas. Para este caso de uso simple, será un cuadro de diálogo vacío.

   Bajo `/apps/we-retail-journal/components/helloworld` agregar un nombre de nodo `cq:design_dialog` de `nt:unstructured`.

   La configuración se representa en XML a continuación (`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. Implemente el código base para AEM desde la línea de comandos:

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   En [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) valide que el componente se haya implementado inspeccionando la carpeta en `/apps/we-retail-journal/components:`

   ![Estructura de componentes implementada en el CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Crear modelo de Sling {#create-sling-model}

**Persona: Desarrollador de AEM**

Siguiente a [!DNL Sling Model] se crea para respaldar el [!DNL Hello World] componente. En un caso de uso tradicional de WCM, la variable [!DNL Sling Model] implementa cualquier lógica empresarial y un script de renderización del lado del servidor (HTL) realizará una llamada al [!DNL Sling Model]. Esto mantiene el script de renderización relativamente simple.

[!DNL Sling Models] también se utilizan en el caso de uso de SPA para implementar la lógica empresarial del lado del servidor. La diferencia es que en la variable [!DNL SPA] caso de uso, la variable [!DNL Sling Models] expone sus métodos como JSON serializados.

>[!NOTE]
>
>Como práctica recomendada, los desarrolladores deben buscar utilizar [Componentes principales AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es) cuando sea posible. Entre otras funciones, los componentes principales proporcionan [!DNL Sling Models] con la salida JSON &quot;lista para SPA&quot;, que permite a los desarrolladores centrarse más en la presentación del front-end.

1. En el editor de su elección, abra el **we-retail-journal-commons** proyecto ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. En el paquete `com.adobe.cq.sample.spa.commons.impl.models`:
   * Cree una nueva clase denominada `HelloWorld`.
   * Añadir una interfaz de implementación para `com.adobe.cq.export.json.ComponentExporter.`

   ![Asistente para nuevas clases Java](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   La variable `ComponentExporter` debe implementarse para que [!DNL Sling Model] para ser compatible con los servicios de contenido AEM.

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. Añada una variable estática denominada `RESOURCE_TYPE` para identificar el [!DNL HelloWorld] tipo de recurso del componente:

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. Añadir anotaciones OSGi para `@Model` y `@Exporter`. La variable `@Model` la anotación registrará la clase como un [!DNL Sling Model]. La variable `@Exporter` la anotación mostrará los métodos como JSON serializados usando la variable [!DNL Jackson Exporter] marco.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. Implementar el método `getDisplayMessage()` para devolver la propiedad JCR `message`. Utilice la variable [!DNL Sling Model] anotación de `@ValueMapValue` para facilitar la recuperación de la propiedad `message` se almacenan debajo del componente. La variable `@Optional` la anotación es importante, ya que cuando el componente se añade por primera vez a la página,  `message`  no se rellenará.

   Como parte de la lógica empresarial, una cadena, &quot;**Hello**&quot;, se añadirá al mensaje.

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > El nombre del método `getDisplayMessage` es importante. Cuando la variable [!DNL Sling Model] se serializa con la variable [!DNL Jackson Exporter] se expone como una propiedad JSON: `displayMessage`. La variable [!DNL Jackson Exporter] serializará y expondrá todo `getter` métodos que no toman un parámetro (a menos que se indique explícitamente que deben ignorarse). Más adelante en la aplicación React/Angular, leeremos este valor de propiedad y lo mostraremos como parte de la aplicación.

   El método `getExportedType` también es importante. El valor del componente `resourceType` se utilizará para &quot;asignar&quot; los datos JSON al componente front-end (Angular/Reacción). Exploraremos esto en la siguiente sección.

1. Implementar el método `getExportedType()` para devolver el tipo de recurso de la variable `HelloWorld` componente.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   El código completo de [**HelloWorld.java** se puede encontrar aquí.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Implemente el código en AEM mediante Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Verifique la implementación y el registro del [!DNL Sling Model] navegando a [[!UICONTROL Estado] > [!UICONTROL Modelos de Sling]](http://localhost:4502/system/console/status-slingmodels) en la consola OSGi.

   Debería ver que la variable `HelloWorld` El modelo de Sling está enlazado al `we-retail-journal/components/helloworld` Tipo de recurso Sling y que está registrado como un [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## Crear componente React {#react-component}

**Persona: Desarrollador de front-end**

A continuación, se creará el componente React . Abra el **react-app** módulo ( `<src>/aem-sample-we-retail-journal/react-app`) utilizando el editor de su elección.

>[!NOTE]
>
> No dude en omitir esta sección si solo está interesado en [Desarrollo de angulars](#angular-component).

1. Dentro de `react-app` vaya a su carpeta src. Expanda la carpeta de componentes para ver los archivos de componentes React existentes.

   ![reaccionar a la estructura de archivos de componente](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. Añada un nuevo archivo debajo de la carpeta de componentes denominada `HelloWorld.js`.
1. Abra `HelloWorld.js`. Agregue una instrucción import para importar la biblioteca de componentes React. Agregue una segunda instrucción de importación para importar la variable `MapTo` ayuda proporcionada por Adobe. La variable `MapTo` helper proporciona una asignación del componente React al JSON del componente AEM.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. Debajo de las importaciones, cree una nueva clase denominada `HelloWorld` que amplía el React `Component` interfaz. Añada el `render()` para `HelloWorld` Clase .

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. La variable `MapTo` helper incluye automáticamente un objeto llamado `cqModel` como parte de las propiedades del componente React. La variable `cqModel` incluye todas las propiedades expuestas por el [!DNL Sling Model].

   Recuerde [!DNL Sling Model] creado anteriormente contiene un método `getDisplayMessage()`. `getDisplayMessage()` se traduce como una clave JSON denominada `displayMessage` al generar la salida.

   Implementación de `render()` método para generar un `h1` que contiene el valor de `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html), una extensión de sintaxis a JavaScript, se utiliza para devolver el marcado final del componente.

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. Implemente un método de configuración de edición. Este método se pasa a través de la variable `MapTo` y proporciona al editor de AEM información para mostrar un marcador de posición en caso de que el componente esté vacío. Esto ocurre cuando el componente se agrega al SPA pero aún no se ha creado. Agregue lo siguiente a continuación: `HelloWorld` Clase :

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. Al final del archivo, llame a la función `MapTo` ayuda, pasando el `HelloWorld` y `HelloWorldEditConfig`. Esto asignará el componente React al componente AEM en función del tipo de recurso del componente AEM: `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   El código completado para [**HelloWorld.js** se puede encontrar aquí.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Abra el archivo `ImportComponents.js`. Se encuentra en `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   Añada una línea para requerir que el `HelloWorld.js` con los demás componentes del paquete JavaScript compilado:

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. En el  `components`  carpeta crear un nuevo archivo denominado `HelloWorld.css` como hermano de `HelloWorld.js.` Rellene el archivo con lo siguiente para crear un estilo básico para la variable `HelloWorld` componente:

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Volver a abrir `HelloWorld.js` y actualizar debajo de las instrucciones de importación para requerir `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Implemente el código en AEM mediante Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. En [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) open `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. Realice una búsqueda rápida de HelloWorld en app.js para comprobar que el componente React se ha incluido en la aplicación compilada.

   >[!NOTE]
   >
   > **app.js** es la aplicación React incluida. El código ya no es legible por el ser humano. La variable `npm run build` ha activado una compilación optimizada que genera JavaScript compilado que puede ser interpretado por exploradores modernos.


## Crear componente de Angular {#angular-component}

**Persona: Desarrollador de front-end**

>[!NOTE]
>
> No dude en omitir esta sección si solo está interesado en el desarrollo de React.

A continuación, se creará el componente Angular. Abra el **aplicación de angular** módulo (`<src>/aem-sample-we-retail-journal/angular-app`) utilizando el editor de su elección.

1. Dentro de `angular-app` carpeta navegar a su `src` carpeta. Expanda la carpeta de componentes para ver los archivos de componentes de Angular existentes.

   ![Estructura del archivo de angular](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Añada una nueva carpeta debajo de la carpeta de componentes denominada `helloworld`. Debajo de la variable `helloworld` carpeta agregar nuevos archivos llamados `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. Abra `helloworld.component.ts`. Agregar una instrucción import para importar el Angular `Component` y `Input` clases . Cree un componente nuevo, señalando la variable `styleUrls` y `templateUrl` a `helloworld.component.css` y `helloworld.component.html`. Por último, exporte la clase `HelloWorldComponent` con la entrada esperada de `displayMessage`.

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > Si recuerda el [!DNL Sling Model] creado anteriormente, había un método **getDisplayMessage()**. El JSON serializado de este método será **displayMessage**, que ahora estamos leyendo en la aplicación de Angular.

1. Apertura `helloworld.component.html` para incluir un `h1` que imprimirá el `displayMessage` propiedad:

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. Actualizar `helloworld.component.css` para incluir algunos estilos básicos para el componente.

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Actualizar `helloworld.component.spec.ts` con el siguiente banco de pruebas:

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. Actualización siguiente `src/components/mapping.ts` para incluir el `HelloWorldComponent`. Agregue un `HelloWorldEditConfig` que marcará el marcador de posición en el editor de AEM antes de que se haya configurado el componente. Finalmente, agregue una línea para asignar el componente AEM al componente Angular con el `MapTo` ayuda.

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   El código completo de [**mapping.ts** se puede encontrar aquí.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. Actualizar `src/app.module.ts` para actualizar el **NgModule**. Agregue la variable **`HelloWorldComponent`** como **declaración** que pertenece a la variable **AppModule**. Agregue también la variable `HelloWorldComponent` como **entryComponent** para que se compile y incluya dinámicamente en la aplicación a medida que se procesa el modelo JSON.

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   El código completado para [**app.module.ts** se puede encontrar aquí.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Implemente el código para AEM mediante Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. En [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) open `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Realice una búsqueda rápida para **HelloWorld** en `main.js` para comprobar si se ha incluido el componente de Angular.

   >[!NOTE]
   >
   > **main.js** es la aplicación de Angular agrupada. El código ya no es legible por el ser humano. El comando npm run build ha activado una compilación optimizada que genera JavaScript compilado que puede ser interpretado por los navegadores modernos.

## Actualización de la plantilla {#template-update}

1. Vaya a la Plantilla editable para las versiones React y/o Angular:

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. Seleccione el principal [!UICONTROL Contenedor de diseño] y seleccione [!UICONTROL Política] para abrir su directiva:

   ![Seleccionar política de diseño](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   En **[!UICONTROL Propiedades]** > **[!UICONTROL Componentes permitidos]**, realice una búsqueda de **[!DNL Custom Components]**. Debería ver el **[!DNL Hello World]** , selecciónelo. Guarde los cambios haciendo clic en la casilla de verificación en la esquina superior derecha.

   ![Configuración de la directiva del contenedor de diseño](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. Después de guardar, debería ver la variable **[!DNL HelloWorld]** como componente permitido en el [!UICONTROL Contenedor de diseño].

   ![Componentes permitidos actualizados](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Solo AEM 6.5 y AEM 6.4.5 son compatibles con la función Plantilla editable del Editor de SPA. Si utiliza AEM 6.4, deberá configurar manualmente la directiva para los componentes permitidos mediante el CRXDE Lite: `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` o `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite que muestra las configuraciones de directiva actualizadas para [!UICONTROL Componentes permitidos] en el [!UICONTROL Contenedor de diseño]:

   ![CRXDE Lite que muestra las configuraciones de directiva actualizadas para los componentes permitidos en el contenedor de diseño](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## En resumen {#putting-together}

1. Vaya a las páginas Angular o Reacción :

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. Busque la **[!DNL Hello World]** y arrastre y suelte el **[!DNL Hello World]** en la página.

   ![hola mundo arrastrar + soltar](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   Debe aparecer el marcador de posición.

   ![Hola, portaequipajes](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. Seleccione el componente y añada un mensaje en el cuadro de diálogo, es decir, &quot;Mundo&quot; o &quot;Su nombre&quot;. Guarde los cambios.

   ![componente renderizado](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   Tenga en cuenta que la cadena &quot;Hello&quot; siempre se antepone al mensaje. Esto es resultado de la lógica en la variable `HelloWorld.java` [!DNL Sling Model].

## Siguientes pasos {#next-steps}

[Solución completa para el componente HelloWorld](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Código fuente completo para [[!DNL We.Retail Journal] en GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Consulte un tutorial más detallado sobre el desarrollo de React con [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/spa-editor/spa-editor-framework-feature-video-use.html?lang=es)

## Solución de problemas {#troubleshooting}

### No se puede crear el proyecto en Eclipse {#unable-to-build-project-in-eclipse}

**Error:** Error al importar la variable [!DNL We.Retail Journal] proyectar en Eclipse para ejecuciones de objetivos no reconocidas:

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![asistente de error de eclipse](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Resolución**: Haga clic en Finish para resolverlos más adelante. Esto no debe impedir la finalización del tutorial.

**Error**: El módulo React, `react-app`, no se genera correctamente durante una compilación de Maven.

**Resolución:** Intente eliminar la variable `node_modules` carpeta debajo de la **react-app**. Vuelva a ejecutar el comando Apache Maven `mvn  clean install -PautoInstallSinglePackage` desde la raíz del proyecto.

### Dependencias insatisfechas en AEM {#unsatisfied-dependencies-in-aem}

![Error de dependencia del administrador de paquetes](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Si no se cumple una dependencia de AEM, en **[!UICONTROL Administrador de paquetes AEM]** o en el **[!UICONTROL Consola web AEM]** (Consola Felix), esto indica que SPA característica Editor no está disponible.

### El componente no se muestra

**Error**: Incluso después de una implementación correcta y de comprobar que las versiones compiladas de las aplicaciones React/Angular han actualizado `helloworld` mi componente no se muestra cuando lo arrastro a la página. Puedo ver el componente en la interfaz de usuario de AEM.

**Resolución**: Borre el historial/caché del explorador y/o abra un explorador nuevo o utilice el modo incógnito. Si eso no funciona, invalide la caché de la biblioteca del cliente en la instancia de AEM local. AEM intenta almacenar en caché bibliotecas de clientes grandes para que sean eficientes. A veces, es necesario invalidar manualmente la caché para solucionar problemas en los que el código obsoleto se almacena en caché.

Vaya a: [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) y haga clic en Invalidar caché. Vuelva a la página React/Angular y actualice la página.

![Reconstruir biblioteca de cliente](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)

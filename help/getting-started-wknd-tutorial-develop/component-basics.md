---
title: 'Introducción a AEM Sites: conceptos básicos de componentes'
description: Comprender la tecnología subyacente de un componente de sitios de Adobe Experience Manager (AEM) a través de un sencillo ejemplo de "HelloWorld". Se exploran los temas de HTL, modelos Sling, bibliotecas del lado del cliente y diálogos de creación.
sub-product: sitios
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 2%

---


# Conceptos básicos de componentes {#component-basics}

En este capítulo analizaremos la tecnología subyacente de un componente Sitios de Adobe Experience Manager (AEM) a través de un `HelloWorld` ejemplo sencillo. Se realizarán pequeñas modificaciones en un componente existente, que abarcará temas de creación, HTL, modelos Sling, bibliotecas del lado del cliente.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un entorno [de desarrollo](overview.md#local-dev-environment)local.

## Objetivo {#objective}

1. Conozca la función de las plantillas HTL y los modelos Sling para procesar HTML dinámicamente.
1. Comprender cómo se utilizan los diálogos para facilitar la creación de contenido.
1. Conozca los conceptos básicos de las bibliotecas del lado del cliente para incluir CSS y JavaScript para admitir un componente.

## Qué va a generar {#what-you-will-build}

En este capítulo, realizará varias modificaciones en un `HelloWorld` componente muy sencillo. En el proceso de realizar actualizaciones del `HelloWorld` componente, aprenderá las áreas clave del desarrollo de AEM componente.

## Proyecto de inicio de capítulo {#starter-project}

Este capítulo se basa en un proyecto genérico generado por el [AEM Arquetipo](https://github.com/adobe/aem-project-archetype)del Proyecto. Vea el siguiente vídeo y revise los [requisitos](#prerequisites) para empezar.

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

Abra un nuevo terminal de línea de comandos y realice las siguientes acciones.

1. En un directorio vacío, clona el repositorio [aem-guide-wknd](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > Opcionalmente, puede descargar la [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) rama directamente.

1. Desplácese al `aem-guides-wknd` directorio:

   ```shell
   $ cd aem-guides-wknd
   ```

1. Cambiar a la `component-basics/start` rama:

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. Cree e implemente el proyecto en una instancia local de AEM con el siguiente comando:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. Importe el proyecto en su IDE preferido siguiendo las instrucciones para configurar un entorno [de desarrollo](overview.md#local-dev-environment)local.

## Creación de componentes {#component-authoring}

Los componentes pueden considerarse pequeños componentes modulares de una página web. Para reutilizar componentes, estos deben ser configurables. Esto se logra a través del diálogo de creación. A continuación, crearemos un componente sencillo e inspeccionaremos cómo se mantienen los valores del cuadro de diálogo en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Cree una nueva página con el nombre **Conceptos básicos** de componentes debajo del sitio **** WKND `>` US **** en `>` fin ****.
1. Añada el componente **** Hello World a la página recientemente creada.
1. Abra el cuadro de diálogo del componente e introduzca texto. Guarde los cambios para ver el mensaje mostrado en la página.
1. Cambie al modo de desarrollador y vista de la ruta de contenido en CRXDE-Lite e inspeccione las propiedades de la instancia de componente.
1. Utilice CRXDE-Lite para vista de la secuencia de comandos `cq:dialog` y `helloworld.html` ubicada en `/apps/wknd/components/content/helloworld`.

## HTML Template Language (HTL) {#htl-templates}

HTML Template Language o [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) es un lenguaje de plantilla de peso ligero en el servidor que utilizan los componentes de AEM para procesar contenido.

A continuación, se actualizará el script `HelloWorld` HTL para mostrar un saludo adicional antes del mensaje de texto.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Cambie al IDE de Eclipse y abra el proyecto al `ui.apps` módulo.
1. Abra el `.content.xml` archivo que define el cuadro de diálogo del `HelloWorld` componente en:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. Actualice el cuadro de diálogo para agregar un campo de texto adicional llamado **Saludo** con un nombre de `./greeting`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. Abra el archivo `helloworld.html`, que representa el script HTL principal responsable de procesar el `HelloWorld` componente, ubicado en:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. Actualice `helloworld.html` para representar el valor del campo de texto **Saludo** como parte de una `H1` etiqueta:

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Implemente los cambios en una instancia local de AEM con el complemento Eclipse Developer o con sus conocimientos de Maven.

## Modelos Sling {#sling-models}

Los modelos de Sling son objetos Java &quot;POJO&quot; (Plain Old Java Objects) impulsados por anotaciones que facilitan la asignación de datos de JCR a variables Java y proporcionan otras variedades cuando se desarrollan en el contexto de AEM.

A continuación, realizaremos algunas actualizaciones en el modelo de `HelloWorldModel` Sling para aplicar cierta lógica empresarial a los valores almacenados en el JCR antes de enviarlos a la página.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. Abra el archivo `HelloWorldModel.java`, que es el modelo de Sling utilizado con el `HelloWorld` componente.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Añada las siguientes líneas a la `HelloWorldModel` clase para asignar los valores de las propiedades JCR del componente `greeting` y `text` a las variables Java:

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. Añada el siguiente método `getGreeting()` a la `HelloWorldModel` clase, que devuelve el valor de la propiedad denominada `greeting`. Este método agrega la lógica adicional para devolver un valor String de &quot;Hello&quot; si la propiedad `greeting` es nula o está en blanco:

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. Añada el siguiente método `getTextUpperCase()` a la `HelloWorldModel` clase, que devuelve el valor de la propiedad denominada `text`. Este método transforma la cadena a todos los caracteres de mayúsculas y minúsculas.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Actualice el archivo `helloworld.html` en `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` para utilizar los métodos recién creados del `HelloWorld` modelo:

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Implemente los cambios en una instancia local de AEM con el complemento Eclipse Developer o con sus conocimientos de Maven.

## Bibliotecas de cliente {#client-side-libraries}

Las bibliotecas del lado del cliente, clientlibs para abreviar, proporcionan un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. Las bibliotecas de cliente son la forma estándar de incluir CSS y JavaScript en una página de AEM.

A continuación, incluiremos algunos estilos CSS para el `HelloWorld` componente a fin de comprender los conceptos básicos de las bibliotecas del lado del cliente.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. debajo `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` cree un nuevo nodo denominado `clientlibs` con un tipo de nodo de `cq:ClientLibraryFolder`.
1. Cree una carpeta y una estructura de archivos como la siguiente debajo `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. Rellene `helloworld/clientlibs/css/helloworld.css` con lo siguiente:

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. Rellene `helloworld/clientlibs/css.txt` con lo siguiente:

   ```plain
   #base=css
   helloworld.css
   ```

1. Rellene `helloworld/clientlibs/js/helloworld.js` con lo siguiente:

   ```js
   console.log("hello world!");
   ```

1. Rellene `helloworld/clientlibs/js.txt` con lo siguiente:

   ```plain
   #base=js
   helloworld.js
   ```

1. Actualice las propiedades del `clientlibs` nodo para incluir las dos propiedades siguientes:

   | Nombre | Tipo | Value |
   |------|------|-------|
   | categorías | Cadena | wknd.base |
   | allowProxy | Booleano | verdadero |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Implemente los cambios en una instancia local de AEM con el complemento Eclipse Developer o con sus conocimientos de Maven.

## Felicitaciones! {#congratulations}

Felicitaciones, acaba de aprender los conceptos básicos del desarrollo de componentes en Adobe Experience Manager!

### Próximos pasos {#next-steps}

Familiarícese con las páginas y plantillas de Adobe Experience Manager en el capítulo siguiente [Páginas y plantillas](pages-templates.md). Comprenda cómo se procesan los componentes principales en el proyecto y aprenda las configuraciones de políticas avanzadas de las plantillas editables para crear una plantilla de página del artículo bien estructurada.

Vista el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la llave de Git `component-basics/solution`.

1. Clona el repositorio de [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) .
1. Extraer la `component-basics/solution` rama

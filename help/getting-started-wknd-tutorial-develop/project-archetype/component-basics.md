---
title: 'Introducción a AEM Sites: Conceptos básicos de los componentes'
description: Comprenda la tecnología subyacente de un componente de Adobe Experience Manager AEM () Sites con un sencillo ejemplo de HelloWorld. Se exploran los temas de HTL, modelos Sling, bibliotecas del lado del cliente y cuadros de diálogo de autor.
version: 6.5, Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
duration: 1715
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 1%

---

# Conceptos básicos de componentes {#component-basics}

En este capítulo, vamos a explorar la tecnología subyacente de un componente de sitios de Adobe Experience Manager AEM () a través de una sencilla `HelloWorld` ejemplo. Se realizan pequeñas modificaciones en un componente existente, que cubren temas de creación, HTL, modelos Sling y bibliotecas del lado del cliente.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](./overview.md#local-dev-environment).

El IDE utilizado en los vídeos es [Código de Visual Studio](https://code.visualstudio.com/) y el [AEM Sincronización de VSCode con la](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) plugin.

## Objetivo {#objective}

1. Aprenda la función que desempeñan las plantillas HTL y los modelos Sling para procesar dinámicamente el HTML.
1. Descubra cómo se utilizan los cuadros de diálogo para facilitar la creación de contenido.
1. Conozca los conceptos básicos de las bibliotecas del lado del cliente para incluir CSS y JavaScript para admitir un componente.

## Lo que va a generar {#what-build}

En este capítulo, se realizan varias modificaciones en un `HelloWorld` componente. Al realizar actualizaciones en `HelloWorld` AEM componente, aprenderá sobre las áreas clave del desarrollo de componentes de la red de trabajo de la red de la red de.

## Proyecto de inicio de capítulo {#starter-project}

Este capítulo se basa en un proyecto genérico generado por el [AEM Tipo de archivo del proyecto](https://github.com/adobe/aem-project-archetype). Vea el siguiente vídeo y revise la [requisitos previos](#prerequisites) para empezar.

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

Abra un nuevo terminal de línea de comandos y realice las siguientes acciones.

1. En un directorio vacío, clone el [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repositorio:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > De forma opcional, puede seguir utilizando el proyecto generado en el capítulo anterior, [Configuración del proyecto](./project-setup.md).

1. Vaya a  `aem-guides-wknd` carpeta.

   ```shell
   $ cd aem-guides-wknd
   ```

1. AEM Genere e implemente el proyecto en una instancia local de con el siguiente comando:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM Si se utiliza la versión 6.5 o 6.4, se debe anexar la variable `classic` de perfil a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importe el proyecto en su IDE preferido siguiendo las instrucciones para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

## Creación de componentes {#component-authoring}

Los componentes se pueden considerar como pequeños componentes modulares de una página web. Para reutilizar componentes, estos deben poder configurarse. Esto se realiza a través del cuadro de diálogo de autor. AEM A continuación, vamos a crear un componente simple e inspeccionar cómo se conservan los valores del cuadro de diálogo en los puntos de vista de la interfaz de usuario de la interfaz de usuario de la.

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Cree una página llamada **Conceptos básicos de componentes** debajo **Sitio WKND** `>` **US** `>` **en**.
1. Añada el **Componente Hello World** a la página recién creada.
1. Abra el cuadro de diálogo del componente e introduzca un texto. Guarde los cambios para ver el mensaje que se muestra en la página.
1. Cambie al modo de desarrollador y vea la ruta de contenido en CRXDE-Lite e inspeccione las propiedades de la instancia del componente.
1. Utilice CRXDE-Lite para ver la `cq:dialog` y `helloworld.html` script desde `/apps/wknd/components/content/helloworld`.

## Lenguaje de plantilla de HTML (HTL) y cuadros de diálogo {#htl-dialogs}

Lenguaje de plantilla de HTML o **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** AEM es un lenguaje ligero de creación de plantillas del lado del servidor que utilizan los componentes de la para procesar contenido.

**Cuadros de diálogo** defina las configuraciones disponibles que se pueden realizar para un componente.

A continuación, vamos a actualizar el `HelloWorld` Script HTL para mostrar un saludo adicional antes del mensaje de texto.

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Cambie al IDE y abra el proyecto en `ui.apps` módulo.
1. Abra el `helloworld.html` y actualice el HTML Markup.
1. Utilice las herramientas IDE como [AEM Sincronización de VSCode con la](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) AEM para sincronizar el cambio de archivo con la instancia de la instancia local de la.
1. Vuelva al explorador y observe que el procesamiento del componente ha cambiado.
1. Abra el `.content.xml` que define el cuadro de diálogo de `HelloWorld` componente en:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Actualice el cuadro de diálogo para añadir un campo de texto adicional llamado **Título** con el nombre de `./title`:

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
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
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

1. Vuelva a abrir el archivo `helloworld.html`, que representa la secuencia de comandos HTL principal responsable de procesar el `HelloWorld` componente de la siguiente ruta:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Actualizar `helloworld.html` para representar el valor de **Saludo** textfield como parte de un `H1` etiqueta:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. AEM Implemente los cambios en una instancia local de con el complemento para desarrolladores o con sus habilidades con Maven.

## Modelos Sling {#sling-models}

Los modelos Sling son objetos Java™ &quot;POJO&quot; (Plain Old Java™ Objects) impulsados por anotaciones que facilitan la asignación de datos de las variables JCR a Java™. AEM También proporcionan varias otras sutilezas cuando se desarrollan en el contexto de la.

A continuación, vamos a realizar algunas actualizaciones en el `HelloWorldModel` Modelo Sling para aplicar lógica empresarial a los valores almacenados en el JCR antes de enviarlos a la página.

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. Abra el archivo `HelloWorldModel.java`, que es el modelo Sling utilizado con `HelloWorld` componente.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Añada las siguientes instrucciones de importación:

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Actualice el `@Model` anotación para utilizar un `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Añada las siguientes líneas a `HelloWorldModel` para asignar los valores de las propiedades JCR del componente `title` y `text` a variables Java™:

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. Añada el siguiente método `getTitle()` a la `HelloWorldModel` clase, que devuelve el valor de la propiedad denominada `title`. Este método agrega la lógica adicional para devolver un valor de cadena de &quot;Valor predeterminado aquí&quot;. si la propiedad `title` es nulo o está en blanco:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Añada el siguiente método `getText()` a la `HelloWorldModel` clase, que devuelve el valor de la propiedad denominada `text`. Este método transforma la cadena en todos los caracteres en mayúsculas.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Cree e implemente el paquete desde el `core` módulo:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > AEM Para el uso de 6.4/6.5, se utiliza: `mvn clean install -PautoInstallBundle -Pclassic`

1. Actualizar el archivo `helloworld.html` en `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` para utilizar los métodos recién creados del `HelloWorld` modelo.

   El `HelloWorld` Se crea una instancia del modelo para esta instancia de componente mediante la directiva HTL: `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"`, guardando la instancia en la variable `model`.

   El `HelloWorld` La instancia de modelo de ya está disponible en HTL a través de la `model` usando la variable `HelloWord`. Estas invocaciones a estos métodos pueden utilizar sintaxis de método abreviada, por ejemplo: `${model.getTitle()}` se puede abreviar como `${model.title}`.

   Del mismo modo, todos los scripts HTL se insertan con [objetos globales](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) a los que se puede acceder mediante la misma sintaxis que los objetos del modelo Sling.

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld" 
       data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
   </div>
   ```

1. AEM Implemente los cambios en una instancia local de con el complemento Eclipse Developer o con sus habilidades con Maven.

## Bibliotecas del cliente {#client-side-libraries}

Bibliotecas del cliente, `clientlibs` en pocas palabras, proporciona un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. AEM Las bibliotecas del lado del cliente son la forma estándar de incluir CSS y JavaScript en una página de.

El [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) El módulo está desacoplado [webpack](https://webpack.js.org/) proyecto que se integra en el proceso de compilación. Esto permite el uso de bibliotecas de front-end populares como Sass, LESS y TypeScript. El `ui.frontend` El módulo de se explora con más detalle en [Capítulo Bibliotecas del cliente](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

A continuación, actualice los estilos CSS para `HelloWorld` componente.

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Abra una ventana de terminal y navegue hasta el `ui.frontend` directorio

1. Estar en `ui.frontend` ejecute el `npm install npm-run-all --save-dev` para instalar el [npm-run-all](https://www.npmjs.com/package/npm-run-all) módulo del nodo. Este paso es **AEM requerido en el tipo de archivo 39 generado por el proyecto de** Sin embargo, en la próxima versión de tipo de archivo, esto no es obligatorio.

1. A continuación, ejecute el `npm run watch` comando:

   ```shell
   $ npm run watch
   ```

1. Cambie al IDE y abra el proyecto en `ui.frontend` módulo.
1. Abra el archivo `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. Actualice el archivo para mostrar un título rojo:

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. En el terminal, debería ver la actividad que indica que la variable `ui.frontend` AEM Este módulo está compilando y sincronizando los cambios con la instancia local de.

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. Vuelva al explorador y observe que el color del título ha cambiado.

   ![Actualización de conceptos básicos de componentes](assets/component-basics/color-update.png)

## Enhorabuena. {#congratulations}

¡Felicidades, ha aprendido los conceptos básicos del desarrollo de componentes en Adobe Experience Manager!

### Siguientes pasos {#next-steps}

Familiarícese con las páginas y plantillas de Adobe Experience Manager en el capítulo siguiente [Páginas y plantillas](pages-templates.md). Comprenda cómo los componentes principales se procesan como proxy en el proyecto y aprenda configuraciones de directiva avanzadas de plantillas editables para crear una plantilla de página de artículo bien estructurada.

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/component-basics-solution`.

---
title: 'Introducción a AEM Sites: conceptos básicos de componentes'
description: Comprenda la tecnología subyacente de un componente de Adobe Experience Manager (AEM) Sites mediante un sencillo ejemplo de "HelloWorld". Se exploran los temas de HTL, los modelos Sling, las bibliotecas del lado del cliente y los cuadros de diálogo de autor.
sub-product: sitios
feature: componentes, modelos sling, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '1148'
ht-degree: 1%

---


# Conceptos básicos de componentes {#component-basics}

En este capítulo analizaremos la tecnología subyacente de un componente de Adobe Experience Manager (AEM) Sites a través de un ejemplo sencillo `HelloWorld`. Se realizarán pequeñas modificaciones en un componente existente que cubrirá temas como la creación, HTL, modelos Sling y bibliotecas del lado del cliente.

## Requisitos previos {#prerequisites}

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

El IDE utilizado en los vídeos es [Visual Studio Code](https://code.visualstudio.com/) y el complemento [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync).

## Objetivo {#objective}

1. Aprenda la función de las plantillas HTL y los modelos Sling para procesar HTML de forma dinámica.
1. Comprender cómo se utilizan los cuadros de diálogo para facilitar la creación de contenido.
1. Conozca los conceptos básicos de las bibliotecas del lado del cliente para incluir CSS y JavaScript para admitir un componente.

## Qué va a generar {#what-you-will-build}

En este capítulo realizará varias modificaciones en un componente `HelloWorld` muy sencillo. En el proceso de realizar actualizaciones del componente `HelloWorld`, aprenderá sobre las áreas clave del desarrollo de componentes de AEM.

## Proyecto de inicio de capítulo {#starter-project}

Este capítulo se basa en un proyecto genérico generado por el [Tipo de archivo del proyecto AEM](https://github.com/adobe/aem-project-archetype). Vea el siguiente vídeo y revise los [requisitos previos](#prerequisites) para empezar.

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Abra un nuevo terminal de línea de comandos y realice las siguientes acciones.

1. En un directorio vacío, clone el repositorio [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd):

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Opcionalmente, puede seguir utilizando el proyecto generado en el capítulo anterior, [Configuración del proyecto](./project-setup.md).

1. Vaya a la carpeta `aem-guides-wknd` .

   ```shell
   $ cd aem-guides-wknd
   ```

1. Cree e implemente el proyecto en una instancia local de AEM con el siguiente comando:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importe el proyecto en su IDE preferido siguiendo las instrucciones para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

## Creación de componentes {#component-authoring}

Los componentes se pueden considerar pequeños componentes modulares de una página web. Para reutilizar componentes, estos deben ser configurables. Esto se logra mediante el cuadro de diálogo de creación. A continuación, crearemos un componente simple e inspeccionaremos cómo se mantienen los valores del cuadro de diálogo en AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Cree una nueva página con el nombre **Conceptos básicos de componentes** debajo de **Sitio WKND** `>` **EE.UU.** `>` **es**.
1. Añada el **Hello World Component** a la página recién creada.
1. Abra el cuadro de diálogo del componente e introduzca texto. Guarde los cambios para ver el mensaje que aparece en la página.
1. Cambie al modo de desarrollador y vea la Ruta de contenido en CRXDE-Lite e inspeccione las propiedades de la instancia de componente.
1. Utilice CRXDE-Lite para ver el script `cq:dialog` y `helloworld.html` ubicado en `/apps/wknd/components/content/helloworld`.

## HTL (lenguaje de plantilla HTML) y cuadros de diálogo {#htl-dialogs}

El lenguaje de plantilla HTML o **[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)** es un lenguaje de plantilla ligero del lado del servidor que utilizan los componentes de AEM para procesar contenido.

**** Los cuadros de diálogo definen las configuraciones disponibles que se pueden realizar para un componente.

A continuación, actualizaremos el script HTL `HelloWorld` para mostrar un saludo adicional antes del mensaje de texto.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. Cambie al IDE y abra el proyecto en el módulo `ui.apps`.
1. Abra el archivo `helloworld.html` y realice un cambio en el código HTML Markup.
1. Utilice las herramientas IDE como [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) para sincronizar el cambio de archivo con la instancia local de AEM.
1. Vuelva al explorador y observe que el procesamiento del componente ha cambiado.
1. Abra el archivo `.content.xml` que define el cuadro de diálogo para el componente `HelloWorld` en:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Actualice el cuadro de diálogo para agregar un campo de texto adicional denominado **Title** con un nombre `./title`:

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

1. Vuelva a abrir el archivo `helloworld.html`, que representa el script HTL principal responsable de procesar el componente `HelloWorld`, ubicado en:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Actualice `helloworld.html` para representar el valor del campo de texto **Saludo** como parte de una etiqueta `H1`:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Implemente los cambios en una instancia local de AEM mediante el complemento para desarrolladores o con sus habilidades con Maven.

## Modelos Sling {#sling-models}

Los modelos Sling son objetos Java &quot;POJO&quot; (antiguos objetos Java comunes) impulsados por anotaciones que facilitan la asignación de datos de JCR a variables Java y proporcionan otras variedades al desarrollar en el contexto de AEM.

A continuación, realizaremos algunas actualizaciones en el modelo de Sling `HelloWorldModel` para aplicar cierta lógica empresarial a los valores almacenados en el JCR antes de enviarlos a la página.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Abra el archivo `HelloWorldModel.java`, que es el modelo de Sling utilizado con el componente `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Añada las siguientes instrucciones de importación:

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Actualice la anotación `@Model` para utilizar una `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Agregue las siguientes líneas a la clase `HelloWorldModel` para asignar los valores de las propiedades JCR del componente `title` y `text` a las variables Java:

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

1. Agregue el siguiente método `getTitle()` a la clase `HelloWorldModel`, que devuelve el valor de la propiedad denominada `title`. Este método agrega la lógica adicional para devolver un valor de cadena de &quot;Valor predeterminado aquí!&quot; si la propiedad `title` es nula o está en blanco:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Agregue el siguiente método `getText()` a la clase `HelloWorldModel`, que devuelve el valor de la propiedad denominada `text`. Este método transforma la cadena a todos los caracteres en mayúsculas.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Cree e implemente el paquete desde el módulo `core`:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.4/6.5, utilice `mvn clean install -PautoInstallBundle -Pclassic`

1. Actualice el archivo `helloworld.html` en `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` para utilizar los métodos recién creados del modelo `HelloWorld`:

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
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Implemente los cambios en una instancia local de AEM mediante el complemento para desarrolladores de Eclipse o con sus habilidades con Maven.

## Bibliotecas de cliente {#client-side-libraries}

Las bibliotecas del lado del cliente, clientlibs para abreviar, proporcionan un mecanismo para organizar y administrar los archivos CSS y JavaScript necesarios para una implementación de AEM Sites. Las bibliotecas del lado del cliente son la forma estándar de incluir CSS y JavaScript en una página de AEM.

A continuación, incluiremos algunos estilos CSS para el componente `HelloWorld` para comprender los conceptos básicos de las bibliotecas del lado del cliente.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

A continuación se muestran los pasos de alto nivel realizados en el vídeo anterior.

1. debajo de `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` cree una nueva carpeta denominada `clientlib-helloworld`.
1. Cree una carpeta y una estructura de archivos como la siguiente debajo de `clientlibs`

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. Rellene `helloworld.css` con lo siguiente:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
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
   console.log("Hello World from Javascript!");
   ```

1. Rellene `helloworld/clientlibs/js.txt` con lo siguiente:

   ```plain
   #base=js
   helloworld.js
   ```

1. Actualice el archivo `clientlib-helloworld/.conten.xml` para incluir las siguientes propiedades:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Actualice el archivo `clientlibs/clientlib-base/.content.xml` a **incrustar** en la categoría `wknd.helloworld`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Implemente los cambios en una instancia local de AEM mediante el complemento para desarrolladores o con sus habilidades con Maven.

   >[!NOTE]
   >
   > CSS y JavaScript se almacenan con frecuencia en la caché por el explorador por motivos de rendimiento. Si no ve inmediatamente el cambio para la biblioteca de cliente, realice una actualización dura y borre la caché del explorador. Puede resultar útil utilizar una ventana de incógnito para garantizar una nueva caché.

## Felicitaciones! {#congratulations}

¡Enhorabuena, acaba de aprender los conceptos básicos del desarrollo de componentes en Adobe Experience Manager!

### Pasos siguientes {#next-steps}

Familiarícese con las páginas y plantillas de Adobe Experience Manager en el capítulo siguiente [Páginas y plantillas](pages-templates.md). Comprenda cómo se procesan los componentes principales como proxy en el proyecto y aprenda las configuraciones de políticas avanzadas de las plantillas editables para crear una plantilla de página de artículo bien estructurada.

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama `tutorial/component-basics-solution` de Git.


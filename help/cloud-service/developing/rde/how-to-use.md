---
title: Cómo usar el entorno de desarrollo rápido
description: Aprenda a utilizar Rapid Development Environment para implementar código y contenido desde el equipo local.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 0%

---


# Cómo usar el entorno de desarrollo rápido

Más información **cómo usar** Entorno de desarrollo rápido (RDE) en AEM as a Cloud Service. Implemente código y contenido para ciclos de desarrollo más rápidos de su código casi final en el RDE, desde su Entorno de desarrollo integrado (IDE) favorito.

Uso [AEM proyecto de WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) aprenderá a implementar varios artefactos de AEM en el RDE ejecutando AEM-RDE `install` de su IDE favorito.

- Implementación del código AEM y del paquete de contenido (todas, ui.apps)
- Implementación del paquete OSGi y del archivo de configuración
- Apache y Dispatcher configuran la implementación como un archivo zip
- Archivos individuales como HTL, `.content.xml` (cuadro de diálogo XML) implementación
- Revise otros comandos RDE como `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## Requisitos previos

Clonar el [Sitios WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) y ábralo en su IDE favorito para implementar los artefactos AEM en el RDE.

    &quot;shell
    $ git clone git@github.com:adobe/aem-guides-wknd.git
    &quot;

A continuación, créelo e impleméntelo en el AEM-SDK local ejecutando el siguiente comando maven.

    &quot;
    $ cd aem-guides-wknd/
    $ mvn clean install -PautoInstallSinglePackage
    &quot;

## Implementar artefactos AEM usando el complemento AEM-RDE

Al usar la variable `aem:rde:install` vamos a implementar varios artefactos de AEM.

### Implementación `all` y `dispatcher` paquetes

Un punto de partida común es implementar primero la variable `all` y `dispatcher` ejecutando los siguientes comandos.

    &quot;shell
    # Instale el paquete &quot;all&quot;
    $ aio aem:rde:instalar all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip
    
    # Instale el zip &#39;dispatcher&#39;
    $ aio aem:rde:instalar dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
    &quot;

Cuando las implementaciones sean correctas, verifique el sitio WKND tanto en el creador como en los servicios de publicación. Debe poder añadir y editar el contenido en las páginas del sitio WKND y publicarlo.

### Mejora e implementación de un componente

Mejoremos el `Hello World Component` e impleméntelo en el RDE.

1. Abra el XML del cuadro de diálogo (`.content.xml`) archivo desde `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` carpeta
1. Agregue la variable `Description` campo de texto después de la `Text` campo de diálogo

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Abra el `helloworld.html` del archivo `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` carpeta
1. Procesar el `Description` después de la `<div>` del `Text` propiedad.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Compruebe los cambios en el AEM-SDK local realizando la compilación maven o sincronizando archivos individuales.

1. Implemente los cambios en el RDE mediante `ui.apps` o implementando los archivos individuales Dialog y HTL.

   ```shell
   # Using 'ui.apps' package
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Compruebe los cambios en el RDE añadiendo o editando el `Hello World Component` en una página del sitio WKND.

### Consulte la `install` opciones de comando

En el ejemplo de comando de implementación de archivos individual anterior, la variable `-t` y `-p` se utilizan indicadores para indicar el tipo y el destino de la ruta JCR, respectivamente. Revisemos los disponibles `install` para ejecutar el siguiente comando.

    &quot;shell
    $ aio aem:rde:install —help
    &quot;

Las banderas son autoexplicativas, la `-s` El indicador es útil para dirigir la implementación solo al autor o a los servicios de publicación. Utilice la variable `-t` al implementar el **content-file o content-xml** junto con la variable `-p` para especificar la ruta JCR de destino en el entorno RDE AEM.

### Implementación del paquete OSGi

Para aprender a implementar el paquete OSGi, mejoremos el `HelloWorldModel` Clase Java™ e impleméntelo en el RDE.

1. Abra el `HelloWorldModel.java` del archivo `core/src/main/java/com/adobe/aem/guides/wknd/core/models` carpeta
1. Actualice el `init()` como se muestra a continuación:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Compruebe los cambios en el AEM-SDK local implementando el `core` paquete mediante comando maven
1. Implemente los cambios en el RDE ejecutando el siguiente comando

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Compruebe los cambios en el RDE añadiendo o editando el `Hello World Component` en una página del sitio WKND.

### Implementación de la configuración OSGi

Puede implementar los archivos de configuración individuales o el paquete de configuración completo, por ejemplo:

    &quot;shell
    # Implementar archivo de configuración individual
    $ aio aem:rde:instale ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json
    
    # O implementar el paquete de configuración completo
    $ cd ui.config
    $ mvn paquete limpio
    $ aio aem:rde:instale target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
    &quot;

>[!TIP]
>
>Para instalar una configuración OSGi solo en una instancia de autor o publicación, use la variable `-s` indicador.


### Implementación de la configuración de Apache o Dispatcher

Los archivos de configuración de Apache o Dispatcher **no se puede implementar individualmente**, pero toda la estructura de carpetas de Dispatcher debe implementarse en forma de archivo ZIP.

1. Realice un cambio deseado en el archivo de configuración de la variable `dispatcher` , para fines de demostración, actualice el `dispatcher/src/conf.d/available_vhosts/wknd.vhost` para almacenar en caché el `html` solo durante 60 segundos.

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. Compruebe los cambios localmente, consulte [Ejecutar Dispatcher localmente](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) para obtener más información.
1. Implemente los cambios en el RDE ejecutando el siguiente comando:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verificar cambios en el RDE

## Comandos adicionales AEM complemento RDE

Revisemos los comandos adicionales del complemento RDE AEM para administrar e interactuar con el RDE desde su equipo local.

    &quot;shell
    $ aio aem:rde —help
    Interactúe con entornos RapidDev.
    
    USO
    $ aio aem rde COMMAND
    
    COMANDOS
    eliminar paquetes y configuraciones de aem rde Eliminar del modo actual.
    historial de rde de aem Obtenga una lista de las actualizaciones realizadas en el rde actual.
    aem rde install Instalar/actualizar paquetes, configuraciones y paquetes de contenido.
    restablecimiento de rde de aem RDE
    aem rde restart Reinicie el autor y publique un RDE
    estado de rde de aem Obtenga una lista de los paquetes y configuraciones implementados en el rde actual.
    &quot;

El uso de los comandos anteriores permite administrar su RDE desde su IDE favorito para acelerar el ciclo de vida de desarrollo/implementación.

## Siguiente paso

Obtenga información sobre [ciclo de vida de desarrollo/implementación mediante RDE](./development-life-cycle.md) para ofrecer funciones con rapidez.


## Recursos adicionales

[Documentación de comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Complemento CLI de Adobe I/O Runtime para interacciones con entornos de desarrollo rápido AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configuración del proyecto AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)

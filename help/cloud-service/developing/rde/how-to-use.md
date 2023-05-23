---
title: Cómo utilizar el entorno de desarrollo rápido
description: Aprenda a utilizar el entorno de desarrollo rápido para implementar código y contenido desde su equipo local.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---

# Cómo utilizar el entorno de desarrollo rápido

Aprender **cómo usar** AEM Entorno de Desarrollo Rápido (RDE) en as a Cloud Service. Implemente código y contenido para ciclos de desarrollo más rápidos de su código casi final en RDE, desde su entorno de desarrollo integrado (IDE) favorito.

Uso de [AEM Proyecto de sitios de WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM AEM aprenderá a implementar varios artefactos de la en el RDE ejecutando los de RDE de la `install` desde su IDE favorito.

- AEM implementación del paquete de contenido y código de (todo, ui.apps)
- Implementación del paquete OSGi y el archivo de configuración
- Apache y Dispatcher configuran la implementación como un archivo zip
- Archivos individuales como HTL, `.content.xml` Implementación de (dialog XML)
- Revise otros comandos de RDE como `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Requisitos previos

Clonar el [Sitios WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM y ábralo en su IDE favorito para implementar los artefactos de la en el IDE.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

AEM A continuación, créelo e impleméntelo en el SDK de la aplicación local de la aplicación de la plataforma de desarrollo de software (SDK) ejecutando el siguiente comando de Maven.

```
$ cd aem-guides-wknd/
$ mvn clean install -PautoInstallSinglePackage
```

## AEM AEM Implementar artefactos de mediante el complemento de RDE de

Uso del `aem:rde:install` AEM comando, vamos a implementar varios artefactos de la.

### Implementar `all` y `dispatcher` paquetes

Un punto de partida común es implementar primero el `all` y `dispatcher` mediante la ejecución de los siguientes comandos.

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Una vez implementadas correctamente, compruebe el sitio WKND en los servicios de creación y publicación. Debería poder agregar y editar el contenido en las páginas del sitio WKND y publicarlo.

### Mejora e implementación de un componente

Vamos a mejorar la `Hello World Component` e implementarlo en el RDE.

1. Abra el XML de diálogo (`.content.xml`) archivo de `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` carpeta
1. Añada el `Description` campo de texto después del existente `Text` campo de diálogo

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Abra el `helloworld.html` archivo de `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` carpeta
1. Procesar el `Description` después de la propiedad existente `<div>` elemento de la `Text` propiedad.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. AEM Compruebe los cambios en el SDK de la aplicación local de realizando la compilación de Maven o sincronizando los archivos individuales.

1. Implemente los cambios en RDE mediante `ui.apps` o implementando los archivos individuales Dialog y HTL.

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

1. Verifique los cambios en el editor de texto enriquecido agregando o editando el `Hello World Component` en una página del sitio WKND.

### Revise la `install` opciones de comando

En el ejemplo de comando de implementación de archivo individual anterior, la variable `-t` y `-p` los indicadores se utilizan para indicar el tipo y el destino de la ruta JCR respectivamente. Vamos a revisar los disponibles `install` para ejecutar el siguiente comando.

```shell
$ aio aem:rde:install --help
```

Las banderas se explican por sí mismas, la `-s` El indicador es útil para dirigir la implementación solo a los servicios de autor o publicación. Utilice el `-t` al implementar el **content-file o content-xml** archivos junto con el `-p` AEM Indicador para especificar la ruta JCR de destino en el entorno de RDE de la.

### Implementar el paquete OSGi

Para aprender a implementar el paquete OSGi, vamos a mejorar el `HelloWorldModel` Clase Java™ e impleméntelo en el editor de texto enriquecido.

1. Abra el `HelloWorldModel.java` archivo de `core/src/main/java/com/adobe/aem/guides/wknd/core/models` carpeta
1. Actualice el `init()` método como se muestra a continuación:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. AEM Compruebe los cambios en el SDK de la aplicación local de la aplicación mediante la implementación de la variable `core` paquete mediante el comando maven
1. Implemente los cambios en RDE ejecutando el siguiente comando

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Verifique los cambios en el editor de texto enriquecido agregando o editando el `Hello World Component` en una página del sitio WKND.

### Implementar la configuración OSGi

Puede implementar los archivos de configuración individuales o el paquete de configuración completo, por ejemplo:

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>Para instalar una configuración OSGi solo en una instancia de autor o publicación, utilice el `-s` Indicador.


### Implementar la configuración de Apache o Dispatcher

Los archivos de configuración de Apache o Dispatcher **no se puede implementar individualmente**, pero toda la estructura de carpetas de Dispatcher debe implementarse en forma de archivo ZIP.

1. Realice un cambio deseado en el archivo de configuración de `dispatcher` para fines de demostración, actualice el módulo `dispatcher/src/conf.d/available_vhosts/wknd.vhost` para almacenar en caché el `html` solo durante 60 segundos.

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

1. Compruebe los cambios localmente; consulte [Ejecutar Dispatcher localmente](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) para obtener más información.
1. Implemente los cambios en RDE ejecutando el siguiente comando:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verificar los cambios en el RDE

## AEM Comandos adicionales del complemento RDE

AEM Revisemos los comandos adicionales del complemento RDE para administrar e interactuar con el RDE desde el equipo local.

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

Con los comandos anteriores, su RDE se puede administrar desde su IDE favorito para un ciclo de vida de desarrollo/implementación más rápido.

## Siguiente paso

Obtenga información acerca de [ciclo de vida de desarrollo/implementación mediante RDE](./development-life-cycle.md) para ofrecer funciones con rapidez.


## Recursos adicionales

[Documentación de comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Complemento de CLI de Adobe I/O Runtime AEM para interacciones con entornos de desarrollo rápido de](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM Configuración del proyecto de](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)

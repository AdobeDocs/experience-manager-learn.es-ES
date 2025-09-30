---
title: Cómo utilizar el entorno de desarrollo rápido
description: Aprenda a utilizar el entorno de desarrollo rápido para implementar código y contenido desde su equipo local.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 792
source-git-commit: 2f7e10680c7211da836e33fdd241cd7f5d633d5f
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 1%

---

# Cómo utilizar el entorno de desarrollo rápido

Aprenda **a utilizar** el entorno de desarrollo rápido (RDE) en AEM as a Cloud Service. Implemente código y contenido para ciclos de desarrollo más rápidos de su código casi final en RDE, desde su entorno de desarrollo integrado (IDE) favorito.

Con [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) aprenderá a implementar varios artefactos de AEM en RDE ejecutando el comando `install` de AEM-RDE desde su IDE favorito.

- implementación del paquete de contenido y código AEM (todos, ui.apps)
- Implementación del paquete OSGi y el archivo de configuración
- Implementación de configuraciones de Apache y Dispatcher como archivo zip
- Archivos individuales como la implementación de HTL, `.content.xml` (XML de diálogo)
- Revisar otros comandos de RDE como `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Requisitos previos

Clone el proyecto [WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) y ábralo en su IDE favorito para implementar los artefactos de AEM en RDE.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

A continuación, créelo e impleméntelo en el AEM-SDK local ejecutando el siguiente comando de Maven.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Implementar artefactos de AEM mediante el complemento AEM-RDE

Primero, asegúrese de que tiene instalado el [último módulo CLI de `aio`](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools#aio-cli).

A continuación, utilice el comando `aio aem:rde:install` para implementar varios artefactos de AEM.

### Implementar `all` y `dispatcher` paquetes

Un punto de partida común es implementar primero los paquetes `all` y `dispatcher` ejecutando los siguientes comandos.

```shell
# Install the 'all' content package (zip file)
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' deployment artifact (zip file)
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Una vez implementadas correctamente, compruebe el sitio WKND en los servicios de creación y publicación. Debería poder agregar y editar el contenido en las páginas del sitio WKND y publicarlo.

### Mejora e implementación de un componente

Vamos a mejorar `Hello World Component` e implementarlo en RDE.

1. Abrir el archivo XML de diálogo (`.content.xml`) desde la carpeta `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/`
1. Agregar el campo de texto `Description` después del campo de diálogo `Text` existente

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Abrir el archivo `helloworld.html` de la carpeta `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld`
1. Procesar la propiedad `Description` después del elemento `<div>` existente de la propiedad `Text`.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Compruebe los cambios en el SDK local de AEM realizando la compilación de Maven o sincronizando archivos individuales.

1. Implemente los cambios en RDE a través del paquete `ui.apps` o implementando los archivos individuales de Diálogo y HTL:

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

1. Compruebe los cambios en el RDE agregando o editando `Hello World Component` en una página del sitio WKND.

### Revisar las opciones de comando `install`

En el ejemplo de comando de implementación de archivo individual anterior, los indicadores `-t` y `-p` se utilizan para indicar el tipo y el destino de la ruta de acceso JCR respectivamente. Revisemos las opciones de comando `install` disponibles ejecutando el siguiente comando.

```shell
$ aio aem:rde:install --help
```

Los indicadores se explican por sí mismos, el indicador `-s` es útil para dirigir la implementación solo a los servicios de autor o publicación. Utilice el indicador `-t` al implementar los archivos **content-file o content-xml** junto con el indicador `-p` para especificar la ruta JCR de destino en el entorno AEM RDE.

### Implementar el paquete OSGi

Para obtener información sobre cómo implementar el paquete OSGi, vamos a mejorar la clase Java™ `HelloWorldModel` e implementarla en RDE.

1. Abrir el archivo `HelloWorldModel.java` de la carpeta `core/src/main/java/com/adobe/aem/guides/wknd/core/models`
1. Actualice el método `init()` como se muestra a continuación:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Compruebe los cambios en AEM-SDK local implementando el paquete `core` mediante el comando de Maven
1. Implemente los cambios en RDE ejecutando el siguiente comando

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Compruebe los cambios en el RDE agregando o editando `Hello World Component` en una página del sitio WKND.

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
>Para instalar una configuración OSGi solo en una instancia de autor o publicación, utilice el indicador `-s`.


### Implementar la configuración de Apache o Dispatcher

Los archivos de configuración de Apache o Dispatcher **no se pueden implementar individualmente**, pero toda la estructura de carpetas de Dispatcher debe implementarse en forma de archivo ZIP.

1. Realice un cambio deseado en el archivo de configuración del módulo `dispatcher`; con fines de demostración, actualice `dispatcher/src/conf.d/available_vhosts/wknd.vhost` para almacenar en caché los archivos de `html` solo durante 60 segundos.

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

1. Verifique los cambios localmente; consulte [Ejecutar Dispatcher localmente](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools) para obtener más información.
1. Implemente los cambios en RDE ejecutando el siguiente comando:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verifique los cambios en el RDE.

### Implementación de archivos de configuración (YAML)

Los archivos de CDN, tareas de mantenimiento, reenvío de registros y configuración de autenticación de API de AEM se pueden implementar en RDE mediante el comando `install`. Estas configuraciones se administran como archivos YAML en la carpeta `config` del proyecto AEM. Consulte [Configuraciones admitidas](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/operations/config-pipeline#configurations) para obtener más información.

Para aprender a implementar los archivos de configuración, vamos a mejorar el archivo de configuración de `cdn` e implementarlo en RDE.

1. Abrir el archivo `cdn.yaml` desde la carpeta `config`
1. Actualice la configuración deseada, por ejemplo, actualice el límite de velocidad a 200 solicitudes por segundo

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     trafficFilters:
       rules:
       #  Block client for 5m when it exceeds an average of 100 req/sec to origin on a time window of 10sec
       - name: limit-origin-requests-client-ip
         when:
           reqProperty: tier
           equals: 'publish'
         rateLimit:
           limit: 200 # updated rate limit
           window: 10
           count: fetches
           penalty: 300
           groupBy:
             - reqProperty: clientIp
         action: log
   ...
   ```

1. Implemente los cambios en RDE ejecutando el siguiente comando

   ```shell
   $ aio aem:rde:install -t env-config ./config
   ```

1. Verificar los cambios en el RDE


## Comandos adicionales del complemento AEM RDE

Revisemos los comandos adicionales del complemento AEM RDE para administrar e interactuar con el RDE desde el equipo local.

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

Obtenga información acerca del [ciclo de vida de desarrollo/implementación usando RDE](./development-life-cycle.md) para ofrecer características con rapidez.


## Recursos adicionales

[Documentación de comandos RDE](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments)

[Complemento CLI de Adobe I/O Runtime para interacciones con entornos de desarrollo rápido de AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configuración del proyecto AEM](https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup)

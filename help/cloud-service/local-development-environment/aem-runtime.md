---
title: Configuración del tiempo de ejecución de AEM local para el desarrollo de AEM as a Cloud Service
description: Configure el tiempo de ejecución de AEM local mediante el Jar de inicio rápido del SDK de AEM as a Cloud Service.
feature: Herramientas para desarrolladores
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Desarrollo
role: Desarrollador
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1657'
ht-degree: 1%

---


# Configuración del tiempo de ejecución de AEM local

Adobe Experience Manager (AEM) se puede ejecutar localmente mediante el Jar de inicio rápido del SDK de AEM as a Cloud Service. Esto permite a los desarrolladores implementar y probar el código, la configuración y el contenido personalizados antes de comprometerlo con el control de código fuente e implementarlo en el entorno de AEM as a Cloud Service.

Tenga en cuenta que `~` se utiliza como método abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

## Instalación de Java

Experience Manager es una aplicación Java y, por lo tanto, requiere el SDK de Java para admitir las herramientas de desarrollo.

1. [Descargar e instalar el último SDK de Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2F2Fr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Compruebe que el SDK de Java 11 esté instalado ejecutando el comando :
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Descargar el SDK de AEM as a Cloud Service

El SDK de AEM as a Cloud Service, o SDK de AEM, contiene el Jar de inicio rápido utilizado para ejecutar AEM Author y Publish localmente para el desarrollo, así como la versión compatible de las herramientas de Dispatcher.

1. Inicie sesión en [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con su Adobe ID
   + Tenga en cuenta que la organización de Adobe __debe__ aprovisionarse para AEM as a Cloud Service para descargar el SDK de AEM as a Cloud Service.
1. Vaya a la pestaña __AEM as a Cloud Service__
1. Ordenar por __Fecha de publicación__ en orden __Descendente__
1. Haga clic en la última fila de resultados del __SDK de AEM__
1. Revise y acepte el EULA y pulse el botón __Download__

## Extraiga el Jar de inicio rápido del zip del SDK de AEM

1. Descomprima el archivo `aem-sdk-XXX.zip` descargado

## Configurar el servicio local de AEM Author{#set-up-local-aem-author-service}

El servicio de creación de AEM local proporciona a los desarrolladores una experiencia local que los especialistas en marketing digital/autores de contenido compartirán para crear y administrar contenido.  El servicio de creación de AEM está diseñado como entorno de creación y previsualización, lo que permite realizar la mayoría de las validaciones del desarrollo de funciones en su contra, lo que lo convierte en un elemento vital del proceso de desarrollo local.

1. Crear la carpeta `~/aem-sdk/author`
1. Copie el archivo __Quickstart JAR__ en `~/aem-sdk/author` y cámbielo por `aem-author-p4502.jar`
1. Inicie el servicio local de AEM Author ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-author-p4502.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar el valor predeterminado para el desarrollo local para reducir la necesidad de volver a configurarla.

   *no puede* iniciar el Jar de inicio rápido de AEM as Cloud Service [haciendo doble clic en](#troubleshooting-double-click).
1. Acceda al servicio local de AEM Author en [http://localhost:4502](http://localhost:4502) en un navegador web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Configuración del servicio local de publicación de AEM

El servicio de publicación de AEM local proporciona a los desarrolladores la experiencia local que los usuarios finales de AEM tendrán, como navegar por el sitio web alojado en AEM. Un servicio de publicación de AEM local es importante, ya que se integra con las [herramientas de Dispatcher](./dispatcher-tools.md) del SDK de AEM y permite a los desarrolladores realizar pruebas de humo y ajustar la experiencia de cara al usuario final final.

1. Crear la carpeta `~/aem-sdk/publish`
1. Copie el archivo __Quickstart JAR__ en `~/aem-sdk/publish` y cámbielo por `aem-publish-p4503.jar`
1. Inicie el servicio de publicación de AEM local ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-publish-p4503.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar el valor predeterminado para el desarrollo local para reducir la necesidad de volver a configurarla.

   *no puede* iniciar el Jar de inicio rápido de AEM as Cloud Service [haciendo doble clic en](#troubleshooting-double-click).
1. Acceda al servicio de publicación local de AEM en [http://localhost:4503](http://localhost:4503) en un navegador web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Simular la distribución de contenido {#content-distribution}

En un verdadero entorno de Cloud Service, el contenido se distribuye desde el servicio de creación al servicio de publicación mediante [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) y la canalización de Adobe. La [Canalización de Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) es un microservicio aislado disponible solo en el entorno de la nube.

Durante el desarrollo, puede ser deseable simular la distribución de contenido mediante el servicio Autor y Publicación local. Esto se puede lograr habilitando los agentes de replicación heredados.

>[!NOTE]
>
> Los agentes de replicación solo están disponibles para su uso en el JAR de inicio rápido local y proporcionan solo una simulación de distribución de contenido.

1. Inicie sesión en el servicio **Author** y vaya a [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Haga clic en **Agente predeterminado (publicar)** para abrir el agente de replicación predeterminado.
1. Haga clic en **Editar** para abrir la configuración del agente.
1. En la pestaña **Settings**, actualice los campos siguientes:

   + **Enabled**  - check true
   + **ID de usuario del agente** : deje este campo vacío

   ![Configuración del agente de replicación: configuración](assets/aem-runtime/settings-config.png)

1. En la pestaña **Transport** , actualice los campos siguientes:

   + **URI**  -  `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Usuario**  -  `admin`
   + **Contraseña**  -  `admin`

   ![Configuración del agente de replicación: transporte](assets/aem-runtime/transport-config.png)

1. Haga clic en **Ok** para guardar la configuración y habilitar el **Default** Agente de replicación.
1. Ahora puede realizar cambios en el contenido del servicio Autor y publicarlo en el servicio Publicar .

![Publicar página](assets/aem-runtime/publish-page-changes.png)

## Modos de inicio de Jar de inicio rápido

El nombre del Jar de inicio rápido, `aem-<tier>_<environment>-p<port number>.jar` especifica cómo se iniciará. Una vez que AEM se ha iniciado en un nivel específico, se puede crear o publicar, no se puede cambiar al nivel alternativo. Para ello, la carpeta `crx-Quickstart` generada durante la primera ejecución debe eliminarse y el Jar de inicio rápido debe volver a ejecutarse. El entorno y los puertos se pueden cambiar, pero requieren que se detenga/inicie la instancia local de AEM.

Los cambios en los entornos, `dev`, `stage` y `prod`, pueden ser útiles para los desarrolladores para garantizar que las configuraciones específicas del entorno se definan y resuelven correctamente en AEM. Se recomienda que el desarrollo local se realice principalmente en el modo predeterminado de ejecución del entorno `dev`.

Las permutaciones disponibles son las siguientes:

+ `aem-author-p4502.jar`
   + Como autor en el modo de ejecución de desarrollo en el puerto 4502
+ `aem-author_dev-p4502.jar`
   + Como Autor en el modo de ejecución Dev en el puerto 4502 (igual que `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Como autor en modo de ejecución de ensayo en el puerto 4502
+ `aem-author_prod-p4502.jar`
   + Como Autor en modo de ejecución de producción en el puerto 4502
+ `aem-publish-p4503.jar`
   + Como autor en el modo de ejecución de desarrollo en el puerto 4503
+ `aem-publish_dev-p4503.jar`
   + Como Autor en el modo de ejecución Dev en el puerto 4503 (igual que `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + Como autor en modo de ejecución de ensayo en el puerto 4503
+ `aem-publish_prod-p4503.jar`
   + Como autor en modo de ejecución de producción en el puerto 4503

Tenga en cuenta que el número de puerto puede ser cualquier puerto disponible en la máquina de desarrollo local, aunque por convención:

+ El puerto __4502__ se utiliza para el __servicio Autor de AEM local__
+ El puerto __4503__ se utiliza para el __servicio AEM Publish local__

Para cambiar estas opciones es posible que sea necesario realizar ajustes en las configuraciones del SDK de AEM

## Detención de un tiempo de ejecución local de AEM

Para detener un tiempo de ejecución local de AEM, ya sea Autor de AEM o Servicio de publicación, abra la ventana de línea de comandos que se utilizó para iniciar el tiempo de ejecución de AEM y pulse `Ctrl-C`. Espere a que AEM se cierre. Cuando se complete el proceso de apagado, el símbolo del sistema de la línea de comandos estará disponible.

## Tareas opcionales de configuración de tiempo de ejecución de AEM local

+ __Las variables de entorno de configuración OSGi y las__ variables secretas se establecen  [especialmente para el tiempo de ejecución](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development) local de AEM, en lugar de administrarlas mediante la CLI de aio.

## Cuándo actualizar el Jar de inicio rápido

Actualice el SDK de AEM al menos una vez al mes, o poco después, el último jueves de cada mes, que es la cadencia de la versión de las &quot;versiones de funciones&quot; de AEM as a Cloud Service.

>[!WARNING]
>
> Actualizar el Jar de inicio rápido a una nueva versión requiere reemplazar el entorno de desarrollo local completo, lo que provoca la pérdida de todo el código, la configuración y el contenido en los repositorios locales de AEM. Asegúrese de que cualquier código, configuración o contenido que no se deba destruir se comprometa de forma segura con Git o se exporta desde la instancia local de AEM como paquetes AEM.

### Cómo evitar la pérdida de contenido al actualizar el SDK de AEM

La actualización del SDK de AEM crea de forma eficaz un nuevo tiempo de ejecución de AEM, que incluye un nuevo repositorio, lo que significa que se pierden los cambios realizados en un repositorio anterior del SDK de AEM. Las siguientes son estrategias viables para ayudar a mantener el contenido entre las actualizaciones del SDK de AEM, y se pueden utilizar de forma discreta o concertada:

1. Cree un paquete de contenido dedicado a contener contenido de &quot;muestra&quot; para ayudar en el desarrollo y mantenerlo en Git. Cualquier contenido que se mantenga mediante las actualizaciones del SDK de AEM se conserva en este paquete y se vuelve a implementar después de actualizar el SDK de AEM.
1. Utilice [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la directiva `includepaths` para copiar contenido del repositorio anterior del SDK de AEM al nuevo repositorio del SDK de AEM.
1. Haga una copia de seguridad de cualquier contenido mediante el Administrador de paquetes AEM y paquetes de contenido en el SDK anterior de AEM y vuelva a instalarlos en el nuevo SDK de AEM.

Recuerde que, con los enfoques anteriores para mantener el código entre las actualizaciones del SDK de AEM, indica un anti-patrón de desarrollo. El código no desechable debe originarse en el IDE de desarrollo y fluir al SDK de AEM mediante implementaciones.

## Solución de problemas

## Al hacer doble clic en el archivo Jar de inicio rápido se produce un error{#troubleshooting-double-click}

Al hacer doble clic en el Jar de inicio rápido para iniciar, se muestra un modo de error que impide que AEM se inicie localmente.

![Solución de problemas: haga doble clic en el archivo Jar de inicio rápido](./assets/aem-runtime/troubleshooting__double-click.png)

Esto se debe a que el Jar de inicio rápido de AEM as a Cloud Service no admite hacer doble clic en el Jar de inicio rápido para iniciar AEM localmente. En su lugar, debe ejecutar el archivo Jar desde esa línea de comandos.

Para iniciar el servicio AEM Author, `cd` en el directorio que contiene el Jar de inicio rápido y ejecute el comando:

`$ java -jar aem-author-p4502.jar`

o bien, para iniciar el servicio AEM Publish, `cd` en el directorio que contiene el Jar de inicio rápido y ejecute el comando:

`$ java -jar aem-author-p4503.jar`

## Iniciar el Jar de inicio rápido desde la línea de comandos anula{#troubleshooting-java-8} inmediatamente

Al iniciar el Jar de inicio rápido desde la línea de comandos, el proceso se interrumpe inmediatamente y el servicio AEM no se inicia, con el siguiente error:

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

Esto se debe a que AEM as a Cloud Service requiere Java SDK 11 y está ejecutando una versión diferente, muy probablemente Java 8. Para resolver este problema, descargue e instale [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2F2Fr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Una vez instalado el SDK 11 de Java, compruebe que sea la versión activa ejecutando lo siguiente desde la línea de comandos.

Una vez instalado el SDK de Java 11, compruebe que es la versión activa ejecutando el comando desde la línea de comandos:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Recursos adicionales

+ [Descargar SDK de AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Documentación de Experience Manager Dispatcher](https://docs.adobe.com/content/help/es-ES/experience-manager-dispatcher/using/dispatcher.html)

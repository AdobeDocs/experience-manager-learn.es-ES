---
title: Configuración de Local AEM Runtime para AEM como desarrollo de Cloud Service
description: Configure Local AEM Runtime con la AEM como Jar de inicio rápido de un SDK de Cloud Service.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 1%

---


# Configurar el tiempo de ejecución AEM local

Adobe Experience Manager (AEM) se puede ejecutar localmente mediante el AEM como Jar Quickstart de un SDK de Cloud Service. Esto permite que los desarrolladores implementen y prueben el código personalizado, la configuración y el contenido antes de comprometerlo con el control de código fuente e implementarlo en un AEM como entorno de Cloud Service.

Tenga en cuenta que `~` se utiliza como método abreviado para el Directorio del usuario. En Windows, este es el equivalente de `%HOMEPATH%`.

>[!VIDEO](https://video.tv.adobe.com/v/32551/?quality=12&learn=on)

>[!NOTE]
>
> Este vídeo muestra cómo instalar y ejecutar una instancia local de Adobe Experience Manager en tan solo unos minutos con el inicio rápido local del SDK de AEM. Este vídeo muestra cómo iniciar el inicio rápido local del SDK de AEM haciendo clic con el doble en el archivo Jar de inicio rápido, aunque esto no funcionará en Java 8 que esté instalado en el equipo. De forma alternativa, el inicio rápido local del SDK de AEM se puede iniciar desde la línea de comandos mediante el `java -jar ...` comando, tal como se [describe en esta página](#set-up-local-aem-author-service).

## Instalar Java

Experience Manager es una aplicación Java y, por lo tanto, requiere el SDK de Java para admitir la herramienta de desarrollo.

1. [Descargar e instalar el SDK de Java más reciente 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14)
1. Verifique que Java 11 SDK esté instalado ejecutando el comando:
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Descargar el SDK de AEM como Cloud Service

El AEM como SDK de Cloud Service o SDK de AEM contiene el Jar de inicio rápido utilizado para ejecutar AEM Author y Publicar localmente para el desarrollo, así como la versión compatible de las herramientas de Dispatcher.

1. Inicie sesión en [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con su Adobe ID
   + Tenga en cuenta que la organización de Adobe __debe__ estar aprovisionada para AEM como Cloud Service para descargar la AEM como un SDK Cloud Service.
1. Vaya a la ficha __AEM como Cloud Service__
1. Ordenar por fecha ____ de publicación en orden __De bajada__
1. Haga clic en la última fila de resultados del SDK ____ AEM
1. Revisar y aceptar el EULA y tocar el botón __Descargar__

## Extracción de la barra de inicio rápido del zip del SDK de AEM

1. Descomprima el archivo `aem-sdk-XXX.zip` descargado
1. Asegúrese de que el archivo __license.properties__ del desarrollador Experience Manager esté disponible

Tenga en cuenta que los mismos archivos de Jar de inicio rápido y license.properties se utilizan para el inicio _tanto_ de AEM Author como de los servicios de publicación.

## Configuración del servicio local de AEM Author{#set-up-local-aem-author-service}

El servicio local de creación de AEM proporciona a los desarrolladores una experiencia local que los creadores de contenido/especialistas en marketing digital compartirán para crear y administrar contenido.  AEM Author Service está diseñado como un entorno de creación y previsualización, lo que permite realizar la mayoría de las validaciones del desarrollo de funciones en su contra, lo que lo convierte en un elemento vital del proceso de desarrollo local.

1. Crear la carpeta `~/aem-sdk/author`
1. Copie el archivo JAR __de__ inicio rápido en `~/aem-sdk/author` y cambie su nombre a `aem-author-p4502.jar`
1. Copie el archivo __license.properties__ en  `~/aem-sdk/author`
1. Inicio del servicio local de creación de AEM ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-author-p4502.jar`
      + Proporcione la contraseña de administrador como `admin`. Se acepta cualquier contraseña de administrador, pero se recomienda utilizar el valor predeterminado para el desarrollo local a fin de reducir la necesidad de volver a configurar.

   El inicio de la AEM *no se puede* realizar como Jar de inicio rápido [del Cloud Service haciendo clic](#troubleshooting-double-click)con el doble.
1. Acceda al servicio local de creación de AEM en [http://localhost:4502](http://localhost:4502) en un navegador web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\author
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cp ../license.properties ~/aem-sdk/author
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Configuración del servicio local de AEM Publish

El servicio de publicación AEM local proporciona a los desarrolladores la experiencia local que los usuarios finales de la AEM tendrán, como explorar el sitio web alojado en AEM. Un servicio de publicación AEM local es importante ya que se integra con las herramientas [de](./dispatcher-tools.md) Dispatcher del SDK de AEM y permite a los desarrolladores probar y perfeccionar la experiencia final de cara al usuario final.

1. Crear la carpeta `~/aem-sdk/publish`
1. Copie el archivo JAR __de__ inicio rápido en `~/aem-sdk/publish` y cambie su nombre a `aem-publish-p4503.jar`
1. Copie el archivo __license.properties__ en  `~/aem-sdk/publish`
1. Inicio del servicio de publicación AEM local mediante la ejecución de lo siguiente desde la línea de comandos:
   + `java -jar aem-publish-p4503.jar`
      + Proporcione la contraseña de administrador como `admin`. Se acepta cualquier contraseña de administrador, pero se recomienda utilizar el valor predeterminado para el desarrollo local a fin de reducir la necesidad de volver a configurar.

   El inicio de la AEM *no se puede* realizar como Jar de inicio rápido [del Cloud Service haciendo clic](#troubleshooting-double-click)con el doble.
1. Acceda al servicio de publicación local de AEM en [http://localhost:4503](http://localhost:4503) desde un navegador web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\publish
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cp ../license.properties ~/aem-sdk/publish
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Modos de inicio de Jar de inicio rápido

El nombre de la barra de inicio rápido `aem-<tier>_<environment>-p<port number>.jar` especifica cómo se inicio. Una vez AEM se ha iniciado en un nivel, un autor o una publicación específicos, no se puede cambiar al nivel alternativo. Para ello, se debe eliminar la `crx-Quickstart` carpeta generada durante la primera ejecución y volver a ejecutar la barra de inicio rápido. Entorno y puertos se pueden cambiar, pero requieren detención/inicio de la instancia de AEM local.

Cambiar entornos `dev`, `stage` y `prod`, puede resultar útil para los desarrolladores a fin de garantizar que las configuraciones específicas del entorno se definan y resuelvan correctamente por AEM. Se recomienda que el desarrollo local se realice principalmente en el modo de ejecución de `dev` entorno predeterminado.

Las permutaciones disponibles son las siguientes:

+ `aem-author-p4502.jar`
   + Como autor en el modo de ejecución Dev en el puerto 4502
+ `aem-author_dev-p4502.jar`
   + Como autor en el modo de ejecución Dev en el puerto 4502 (igual que `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Como autor en el modo de ejecución provisional en el puerto 4502
+ `aem-author_prod-p4502.jar`
   + Como autor en el modo de ejecución de producción en el puerto 4502
+ `aem-publish-p4503.jar`
   + Como autor en el modo de ejecución Dev en el puerto 4503
+ `aem-publish_dev-p4503.jar`
   + Como autor en el modo de ejecución Dev en el puerto 4503 (igual que `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + Como autor en el modo de ejecución provisional en el puerto 4503
+ `aem-publish_prod-p4503.jar`
   + Como autor en el modo de ejecución de producción en el puerto 4503

Tenga en cuenta que el número de puerto puede ser cualquier puerto disponible en la máquina de desarrollo local, sin embargo por convención:

+ El puerto __4502__ se utiliza para el servicio AEM Author __local__
+ El puerto __4503__ se utiliza para el servicio de publicación de AEM __local__

Cambiar estas opciones puede requerir ajustes en AEM configuraciones del SDK

## Detener un tiempo de ejecución de AEM local

Para detener un tiempo de ejecución de AEM local, ya sea AEM Author o el servicio Publicar, abra la ventana de línea de comandos que se utilizó para el inicio del AEM en tiempo de ejecución y toque `Ctrl-C`. Espere a que AEM cierre. Cuando se complete el proceso de apagado, estará disponible el símbolo del sistema de la línea de comandos.

## Cuándo actualizar la barra de inicio rápido

Actualice el SDK de AEM al menos mensualmente el último jueves de cada mes o poco después, que es la cadencia de lanzamiento de AEM como &quot;versiones de funciones&quot; de Cloud Service.

>[!WARNING]
>
> La actualización de la barra de inicio rápido a una nueva versión requiere reemplazar el entorno de desarrollo local completo, lo que resulta en una pérdida de todo el código, la configuración y el contenido en los repositorios de AEM locales. Asegúrese de que cualquier código, configuración o contenido que no se deba destruir se comprometa de forma segura con Git o se exporte desde la instancia de AEM local como Paquetes AEM.

### Cómo evitar la pérdida de contenido al actualizar el SDK de AEM

La actualización del SDK de AEM está creando efectivamente un nuevo tiempo de ejecución de AEM, incluido un nuevo repositorio, lo que significa que se perderán los cambios realizados en un repositorio anterior del SDK de AEM. Las siguientes son estrategias viables para ayudar a mantener el contenido entre AEM actualizaciones del SDK y se pueden utilizar de forma discreta o concertada:

1. Cree un paquete de contenido dedicado a contener contenido de &quot;muestra&quot; para ayudar al desarrollo y mantenerlo en Git. Cualquier contenido que debería persistir a través de AEM actualizaciones del SDK se mantendría en este paquete y se volvería a implementar después de actualizar el SDK de AEM.
1. Utilice [roak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la `includepaths` directiva para copiar contenido del repositorio AEM SDK anterior al nuevo repositorio AEM SDK.
1. Realice una copia de seguridad de cualquier contenido mediante AEM administrador de paquetes y paquetes de contenido en el SDK de AEM anterior, y vuelva a instalarlos en el nuevo SDK de AEM.

Recuerde que, con los enfoques anteriores para mantener el código entre AEM actualizaciones del SDK, indica un patrón de desarrollo anti-patrón. El código no desechable debe originarse en el IDE de desarrollo y fluir al SDK de AEM mediante implementaciones.

## Solución de problemas

## Al hacer clic en el archivo Jar de inicio rápido con el doble se produce un error{#troubleshooting-double-click}

Al hacer clic con el doble en el inicio de la barra de inicio rápido, se muestra un modal de error que impide que la AEM se inicie localmente.

![Resolución de problemas: Doble y haga clic en el archivo Jar de inicio rápido](./assets/aem-runtime/troubleshooting__double-click.png)

Esto se debe a que AEM como Cloud Service Jar de inicio rápido no admite hacer clic en el doble de la Jar de inicio rápido para AEM localmente. En su lugar, debe ejecutar el archivo Jar desde esa línea de comandos.

Para inicio del servicio AEM Author, `cd` vaya al directorio que contiene la barra de inicio rápido y ejecute el comando:

`$ java -jar aem-author-p4502.jar`

o bien, para inicio del servicio AEM Publish, `cd` en el directorio que contiene la barra de inicio rápido y ejecute el comando:

`$ java -jar aem-author-p4503.jar`

## El inicio de la barra de inicio rápido desde la línea de comandos se anula inmediatamente{#troubleshooting-java-8}

Al iniciar la barra de inicio rápido desde la línea de comandos, el proceso se anula inmediatamente y el servicio de AEM no se inicio, con el siguiente error:

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

Esto se debe a que AEM como Cloud Service requiere Java SDK 11 y está ejecutando una versión diferente, probablemente Java 8. Para resolver este problema, descargue e instale [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14).
Una vez que se haya instalado Java SDK 11, compruebe que es la versión activa ejecutando lo siguiente desde la línea de comandos.

Una vez que Java 11 SDK esté instalado, verifique que sea la versión activa ejecutando el comando desde la línea de comandos:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Recursos adicionales

+ [Descargar SDK AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Documentación del despachante de Experience Manager](https://docs.adobe.com/content/help/es-ES/experience-manager-dispatcher/using/dispatcher.html)

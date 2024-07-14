---
title: AEM Configuración del SDK de la local para el desarrollo de AEM as a Cloud Service
description: AEM Configure el tiempo de ejecución del SDK de la local mediante el Jar de inicio rápido del SDK de AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 9%

---

# Configuración del SDK de AEM local {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Tiempo de ejecución local de AEM"
>abstract="Adobe Experience Manager (AEM) se puede ejecutar localmente mediante el Jar de inicio rápido del SDK de AEM as a Cloud Service. Esto permite a los desarrolladores implementar y probar el código, la configuración y el contenido personalizados antes de comprometerlo con el control de código fuente e implementarlo en el entorno de AEM as a Cloud Service."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=es" text="SDK de AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html" text="Descargar el SDK de AEM as a Cloud Service"

Adobe Experience Manager (AEM) se puede ejecutar localmente mediante el Jar de inicio rápido del SDK de AEM as a Cloud Service. Esto permite a los desarrolladores implementar y probar el código, la configuración y el contenido personalizados antes de comprometerlo con el control de código fuente e implementarlo en el entorno de AEM as a Cloud Service.

Tenga en cuenta que `~` se usa como abreviatura del directorio del usuario. En Windows, es el equivalente de `%HOMEPATH%`.

## Instale Java™

Experience Manager es una aplicación Java™ y, por lo tanto, requiere el SDK de Java™ de Oracle para admitir las herramientas de desarrollo.

1. [Descargue e instale el SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) más reciente de Java™
1. Compruebe que el SDK de Oracle Java™ 11 está instalado ejecutando el comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## Descargar el SDK de AEM as a Cloud Service

El SDK de AEM as a Cloud Service AEM AEM, o SDK de, contiene el Jar de inicio rápido que se utiliza para ejecutar Author y Publish de manera local para el desarrollo, así como la versión compatible de las herramientas de Dispatcher.

1. Inicie sesión en [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con su Adobe ID
   + Tenga en cuenta que la organización de Adobe __debe__ estar aprovisionada para que AEM as a Cloud Service descargue el SDK de AEM as a Cloud Service.
1. Vaya a la pestaña __AEM as a Cloud Service__
1. Ordenar por __Fecha de publicación__ en orden __Descendente__
1. AEM Haga clic en la última fila de resultados del __SDK__
1. Revise y acepte el EULA y pulse el botón __Descargar__

## AEM Extraiga el Jar de inicio rápido del zip del SDK de la

1. Descomprima el archivo `aem-sdk-XXX.zip` descargado

## AEM Configurar el servicio local de autor de{#set-up-local-aem-author-service}

AEM El servicio de creación de contenido local de la comunidad de proporciona a los desarrolladores una experiencia local que los especialistas en marketing digital/los autores de contenido compartirán para crear y administrar contenido.  AEM El servicio de creación de está diseñado como entorno de creación y previsualización, lo que permite que la mayoría de las validaciones del desarrollo de funciones se puedan realizar en él, lo que lo convierte en un elemento vital del proceso de desarrollo local.

1. Crear la carpeta `~/aem-sdk/author`
1. Copie el archivo __Quickstart JAR__ a `~/aem-sdk/author` y renómbrelo a `aem-author-p4502.jar`
1. AEM Inicie el servicio local de creación de ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-author-p4502.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar la predeterminada para el desarrollo local con el fin de reducir la necesidad de volver a configurar.

   AEM Usted *no puede* iniciar el Jar de inicio rápido [de Cloud Service de la haciendo doble clic](#troubleshooting-double-click).
1. AEM Acceda al servicio local de creación de en [http://localhost:4502](http://localhost:4502) en un explorador web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## AEM Configuración del servicio local de Publish

AEM El servicio local de Publish AEM AEM proporciona a los desarrolladores la experiencia local que tendrán los usuarios finales de la, como explorar el sitio web alojado en el que se encuentra el usuario de la aplicación de forma que pueda acceder a la documentación de la aplicación de forma rápida y sencilla. AEM Un servicio local de Publish AEM es importante ya que se integra con las [herramientas de Dispatcher](./dispatcher-tools.md) del SDK de la comunidad de usuarios, y permite a los desarrolladores probar y afinar la experiencia final que enfrenta el usuario final.

1. Crear la carpeta `~/aem-sdk/publish`
1. Copie el archivo __Quickstart JAR__ a `~/aem-sdk/publish` y renómbrelo a `aem-publish-p4503.jar`
1. AEM Inicie el servicio local de Publish de ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-publish-p4503.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar la predeterminada para el desarrollo local con el fin de reducir la necesidad de volver a configurar.

   AEM Usted *no puede* iniciar el Jar de inicio rápido [de Cloud Service de la haciendo doble clic](#troubleshooting-double-click).
1. AEM Acceda al servicio local de Publish de la en [http://localhost:4503](http://localhost:4503) en un explorador web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## AEM Configuración de servicios locales de en modo de prelanzamiento

AEM El tiempo de ejecución de la versión local se puede iniciar en [modo de prelanzamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=es), lo que permite a los desarrolladores compilar basándose en las funciones de la próxima versión de AEM as a Cloud Service. AEM La versión preliminar se habilita pasando el argumento `-r prerelease` en el primer inicio del tiempo de ejecución de la versión local de la aplicación de tiempo de ejecución de la. AEM AEM Esto se puede utilizar tanto con los servicios locales de creación de como con los servicios de Publish de.


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux®]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## Simular distribución de contenido {#content-distribution}

En un entorno de Cloud Service real, el contenido se distribuye desde el servicio de creación al servicio de Publish mediante [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) y la canalización de Adobe. La [Canalización de Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) es un microservicio aislado disponible solamente en el entorno de la nube.

Durante el desarrollo, puede ser deseable simular la distribución de contenido mediante el servicio local Author y Publish. Esto se puede lograr habilitando los agentes de replicación heredados.

>[!NOTE]
>
> Los agentes de replicación solo están disponibles para su uso en el JAR de inicio rápido local y proporcionan solo una simulación de distribución de contenido.

1. Inicie sesión en el servicio **Author** y vaya a [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Haga clic en **Agente predeterminado (publicar)** para abrir el agente de replicación predeterminado.
1. Haga clic en **Editar** para abrir la configuración del agente.
1. En la ficha **Configuración**, actualice los campos siguientes:

   + **Habilitado** - comprobar verdadero
   + **Id. de usuario agente** - Deje este campo vacío

   ![Configuración del agente de replicación - Configuración](assets/aem-runtime/settings-config.png)

1. En la ficha **Transporte**, actualice los campos siguientes:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Usuario** - `admin`
   + **Contraseña** - `admin`

   ![Configuración del agente de replicación - Transporte](assets/aem-runtime/transport-config.png)

1. Haga clic en **Aceptar** para guardar la configuración y habilitar el agente de replicación **Predeterminado**.
1. Ahora puede realizar cambios en el contenido en el servicio de creación y publicarlos en el servicio de Publish.

![Página de Publish](assets/aem-runtime/publish-page-changes.png)

## Modos de inicio rápido del Jar

El nombre del Jar de inicio rápido `aem-<tier>_<environment>-p<port number>.jar` especifica cómo se iniciará. AEM Una vez que se ha iniciado la creación o publicación en un nivel específico, no se puede cambiar al nivel alternativo. Para ello, se debe eliminar la carpeta `crx-Quickstart` generada durante la primera ejecución y Quickstart Jar se debe ejecutar de nuevo. AEM El entorno y los puertos se pueden cambiar, pero requieren la detención/inicio de la instancia de la instancia de la local.

AEM Cambiar entornos, `dev`, `stage` y `prod`, puede ser útil para los desarrolladores, a fin de garantizar que las configuraciones específicas del entorno se definan y resuelvan correctamente por parte de los usuarios de la solución de problemas de la aplicación de la aplicación de tipo de datos (). Se recomienda que el desarrollo local se realice principalmente en el modo de ejecución del entorno predeterminado `dev`.

Las permutaciones disponibles son las siguientes:

| Nombre de archivo Jar de inicio rápido | Descripción del modo |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Como autor en el modo de ejecución Dev en el puerto 4502 |
| `aem-author_dev-p4502.jar` | Como autor en el modo de ejecución de desarrollo en el puerto 4502 (igual que `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Como autor en el modo de ejecución de ensayo en el puerto 4502 |
| `aem-author_prod-p4502.jar` | Como autor en el modo de ejecución de producción en el puerto 4502 |
| `aem-publish-p4503.jar` | Como Publish en el modo de ejecución Dev en el puerto 4503 |
| `aem-publish_dev-p4503.jar` | Como Publish en el modo de ejecución Dev en el puerto 4503 (igual que `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Como Publish en el modo de ejecución de ensayo en el puerto 4503 |
| `aem-publish_prod-p4503.jar` | Como Publish en el modo de ejecución de producción en el puerto 4503 |

Tenga en cuenta que el número de puerto puede ser cualquier puerto disponible en la máquina de desarrollo local, aunque por convención:

+ AEM El puerto __4502__ se usa para el servicio __local de creación de informes__
+ AEM El puerto __4503__ se usa para el __servicio local de Publish de la__

AEM Para cambiarlos, es posible que sea necesario realizar ajustes en las configuraciones del SDK de la

## AEM Detención de un tiempo de ejecución de local

AEM AEM Para detener un tiempo de ejecución de la local, ya sea el servicio Autor o Publish AEM, abra la ventana de línea de comandos que se utilizó para iniciar el tiempo de ejecución de la y pulse `Ctrl-C`. AEM Espere a que se cierre el sistema de. Cuando se completa el proceso de apagado, el símbolo del sistema está disponible.

## AEM Tareas de configuración de tiempo de ejecución locales opcionales

+ AEM __Las variables de entorno de configuración OSGi y las variables secretas__ están [especialmente configuradas para el tiempo de ejecución local de la](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development), en lugar de administrarlas mediante la CLI de aio.

## Cuándo actualizar el Jar de inicio rápido

AEM Actualice el SDK de la al menos una vez al mes, o poco después, el último jueves de cada mes, que es la cadencia de lanzamiento de las &quot;versiones de funciones&quot; de AEM as a Cloud Service.

>[!WARNING]
>
> AEM La actualización del Jar de inicio rápido a una nueva versión requiere reemplazar todo el entorno de desarrollo local, lo que provoca la pérdida de todo el código, la configuración y el contenido de los repositorios de la base de datos de la aplicación de inicio rápido (QUICKstart JAR) local. AEM AEM Asegúrese de que cualquier código, configuración o contenido que no se deba destruir se conserva de forma segura con Git o se exporta desde la instancia de la local como paquetes de la aplicación.

### AEM Evitar la pérdida de contenido al actualizar el SDK de la

AEM AEM AEM Actualizar el SDK de la es crear de manera efectiva un tiempo de ejecución de la completamente nuevo, incluido un repositorio nuevo, lo que significa que se pierden todos los cambios realizados en un repositorio de un SDK de la API anterior. AEM Las siguientes son estrategias viables para ayudar en la persistencia del contenido entre actualizaciones de SDK de la, y se pueden utilizar de forma discreta o conjunta:

1. Cree un paquete de contenido dedicado a contener contenido de &quot;muestra&quot; para ayudar en el desarrollo y mantenerlo en Git. AEM AEM Cualquier contenido que deba persistir a través de actualizaciones del SDK de la se mantendrá en este paquete y se volverá a implementar después de actualizar el SDK de la.
1. AEM AEM Use [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la directiva `includepaths` para copiar contenido del repositorio anterior del SDK de la en el nuevo repositorio del SDK de la nueva versión.
1. AEM AEM AEM Haga una copia de seguridad de cualquier contenido mediante el Administrador de paquetes de contenido y los paquetes de contenido de la versión anterior del SDK de la de datos y vuelva a instalarlos en el nuevo SDK de la versión de.

AEM Recuerde, el uso de los enfoques anteriores para mantener el código entre las actualizaciones del SDK de la indica un antipatrón de desarrollo. AEM El código no desechable debe originarse en el IDE de desarrollo y fluir hacia el SDK de la aplicación a través de implementaciones de SDK de la aplicación.

## Resolución de problemas

### Al hacer doble clic en el archivo Jar de inicio rápido, se produce un error{#troubleshooting-double-click}

AEM Al hacer doble clic en el Jar de inicio rápido para el inicio, se muestra un modal de error que impide que el inicio se realice de forma local a la vez que se hace de forma local.

![Solución de problemas: haga doble clic en el archivo Jar de inicio rápido](./assets/aem-runtime/troubleshooting__double-click.png)

Esto se debe a que el Jar de inicio rápido de AEM as a Cloud Service AEM no admite hacer doble clic en el Jar de inicio rápido para iniciar la aplicación de forma local. En su lugar, debe ejecutar el archivo Jar desde esa línea de comandos.

AEM Para iniciar el servicio de autor de la, `cd` en el directorio que contiene el Jar de inicio rápido y ejecute el comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

AEM o bien, para iniciar el servicio de Publish, `cd` en el directorio que contiene el Jar de inicio rápido y ejecutar el comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]

### El inicio del Jar de inicio rápido desde la línea de comandos se anula inmediatamente{#troubleshooting-java-8}

AEM Al iniciar el Jar de inicio rápido desde la línea de comandos, el proceso se interrumpe inmediatamente y el servicio no se inicia, con el siguiente error:

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

Esto se debe a que AEM as a Cloud Service requiere Java™ SDK 11 y está ejecutando una versión diferente, muy probablemente Java™ 8. Para resolver este problema, descargue e instale [Oracle Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).

Una vez instalado el SDK de Oracle Java™ 11, compruebe que es la versión activa ejecutando el comando desde la línea de comandos:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

## Recursos adicionales

+ AEM [Descargar SDK de](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Documentación de Dispatcher del Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es)

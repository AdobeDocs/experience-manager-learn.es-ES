---
title: Configurar AEM SDK local para el desarrollo de AEM as a Cloud Service
description: Configure el tiempo de ejecución local de AEM SDK mediante el Jar de inicio rápido de AEM as a Cloud Service SDK.
feature: Developer Tools
version: Experience Manager as a Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: 99e3cadc71ca4e26f9e4034085788dfc5407d1bb
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

Experience Manager es una aplicación Java™ y, por lo tanto, requiere Oracle Java™ SDK para admitir las herramientas de desarrollo.

1. [Descargue e instale el último Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Compruebe que Oracle Java™ 11 SDK está instalado ejecutando el siguiente comando:

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

## Descargar AEM as a Cloud Service SDK

AEM as a Cloud Service SDK, o AEM SDK, contiene el Jar de inicio rápido que se utiliza para ejecutar AEM Author y Publish localmente para el desarrollo, así como la versión compatible de las herramientas de Dispatcher.

1. Inicie sesión en [https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html](https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html) con su Adobe ID
   + Tenga en cuenta que su organización de Adobe __debe__ estar aprovisionada para que AEM as a Cloud Service descargue AEM as a Cloud Service SDK.
1. Vaya a la pestaña __AEM as a Cloud Service__
1. Ordenar por __Fecha de publicación__ en orden __Descendente__
1. Haz clic en la última fila de resultados de __AEM SDK__
1. Revise y acepte el EULA y pulse el botón __Descargar__

## Extraiga el Jar de inicio rápido del zip de AEM SDK

1. Descomprima el archivo `aem-sdk-XXX.zip` descargado

## Configurar el servicio local de AEM Author{#set-up-local-aem-author-service}

El servicio de creación de AEM local proporciona a los desarrolladores una experiencia local que los especialistas en marketing digital/los autores de contenido compartirán para crear y administrar contenido.  El servicio de creación de AEM está diseñado como entorno de creación y previsualización, lo que permite que la mayoría de las validaciones del desarrollo de funciones se puedan realizar en él, lo que lo convierte en un elemento vital del proceso de desarrollo local.

1. Crear la carpeta `~/aem-sdk/author`
1. Copie el archivo __Quickstart JAR__ a `~/aem-sdk/author` y renómbrelo a `aem-author-p4502.jar`
1. Inicie el servicio de AEM Author local ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-author-p4502.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar la predeterminada para el desarrollo local con el fin de reducir la necesidad de volver a configurar.

   Usted *no puede* iniciar AEM as a Cloud Service Quickstart Jar [haciendo doble clic](#troubleshooting-double-click).
1. Acceda al servicio de autor local de AEM en [http://localhost:4502](http://localhost:4502) en un explorador web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## Configuración del servicio de publicación de AEM local

El servicio local de publicación de AEM proporciona a los desarrolladores la experiencia local que tendrán los usuarios finales de AEM, como explorar el sitio web alojado en AEM. Un servicio de publicación local de AEM es importante porque se integra con las [herramientas de Dispatcher](./dispatcher-tools.md) de AEM SDK y permite a los desarrolladores realizar pruebas exhaustivas y ajustar la experiencia final del usuario final.

1. Crear la carpeta `~/aem-sdk/publish`
1. Copie el archivo __Quickstart JAR__ a `~/aem-sdk/publish` y renómbrelo a `aem-publish-p4503.jar`
1. Inicie el servicio de publicación de AEM local ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-publish-p4503.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar la predeterminada para el desarrollo local con el fin de reducir la necesidad de volver a configurar.

   Usted *no puede* iniciar AEM as a Cloud Service Quickstart Jar [haciendo doble clic](#troubleshooting-double-click).
1. Acceda al servicio de publicación de AEM local en [http://localhost:4503](http://localhost:4503) en un explorador web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## Configure los servicios locales de AEM en el modo de prelanzamiento

El tiempo de ejecución local de AEM se puede iniciar en [modo de prelanzamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=es), lo que permite a los desarrolladores compilar basándose en las funciones de la próxima versión de AEM as a Cloud Service. La versión preliminar se habilita pasando el argumento `-r prerelease` en el primer inicio del tiempo de ejecución de AEM local. Esto se puede utilizar con los servicios locales de AEM Author y AEM Publish.


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

En un entorno Cloud Service real, el contenido se distribuye desde el servicio de creación al de publicación mediante [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) y la canalización de Adobe. La [Canalización de Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=es#content-distribution) es un microservicio aislado disponible solamente en el entorno de la nube.

Durante el desarrollo, puede ser deseable simular la distribución de contenido mediante el servicio local de creación y publicación. Esto se puede lograr habilitando los agentes de replicación heredados.

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
1. Ahora puede realizar cambios en el contenido en el servicio de creación y publicarlos en el servicio de publicación.

![Publicar página](assets/aem-runtime/publish-page-changes.png)

## Modos de inicio rápido del Jar

El nombre del Jar de inicio rápido `aem-<tier>_<environment>-p<port number>.jar` especifica cómo se iniciará. Una vez que AEM se ha iniciado en un nivel específico, crearlo o publicarlo, no se puede cambiar al nivel alternativo. Para ello, se debe eliminar la carpeta `crx-Quickstart` generada durante la primera ejecución y Quickstart Jar se debe ejecutar de nuevo. El entorno y los puertos se pueden cambiar, aunque requieren la detención/inicio de la instancia local de AEM.

Cambiar entornos, `dev`, `stage` y `prod`, puede ser útil para los desarrolladores, ya que garantiza que AEM define y resuelve correctamente las configuraciones específicas del entorno. Se recomienda que el desarrollo local se realice principalmente en el modo de ejecución del entorno predeterminado `dev`.

Las permutaciones disponibles son las siguientes:

| Nombre de archivo Jar de inicio rápido | Descripción del modo |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Como autor en el modo de ejecución Dev en el puerto 4502 |
| `aem-author_dev-p4502.jar` | Como autor en el modo de ejecución de desarrollo en el puerto 4502 (igual que `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Como autor en el modo de ejecución de ensayo en el puerto 4502 |
| `aem-author_prod-p4502.jar` | Como autor en el modo de ejecución de producción en el puerto 4502 |
| `aem-publish-p4503.jar` | Como publicar en modo de ejecución de desarrollo en el puerto 4503 |
| `aem-publish_dev-p4503.jar` | Como publicar en modo de ejecución de desarrollo en el puerto 4503 (igual que `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Como publicar en modo de ejecución de ensayo en el puerto 4503 |
| `aem-publish_prod-p4503.jar` | Como publicar en el modo de ejecución de producción en el puerto 4503 |

Tenga en cuenta que el número de puerto puede ser cualquier puerto disponible en la máquina de desarrollo local, aunque por convención:

+ El puerto __4502__ se usa para el __servicio local de AEM Author__
+ El puerto __4503__ se usa para el __servicio local de publicación de AEM__

Para cambiar estos ajustes, es posible que sea necesario realizar ajustes en las configuraciones de AEM SDK

## Detención de un tiempo de ejecución de AEM local

Para detener un tiempo de ejecución de AEM local, ya sea el servicio de AEM Author o Publish, abra la ventana de línea de comandos que se utilizó para iniciar el tiempo de ejecución de AEM y pulse `Ctrl-C`. Espere a que se apague AEM. Cuando se completa el proceso de apagado, el símbolo del sistema está disponible.

## Tareas de configuración del tiempo de ejecución local de AEM opcionales

+ __Las variables de entorno de configuración OSGi y las variables secretas__ están [especialmente configuradas para el tiempo de ejecución local de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=es#local-development), en lugar de administrarlas mediante la CLI de aio.

## Cuándo actualizar el Jar de inicio rápido

Actualice AEM SDK al menos una vez al mes, o poco después, el último jueves de cada mes, que es la cadencia de lanzamiento de las &quot;versiones de funciones&quot; de AEM as a Cloud Service.

>[!WARNING]
>
> La actualización del Jar de inicio rápido a una nueva versión requiere reemplazar todo el entorno de desarrollo local, lo que provoca la pérdida de todo el código, la configuración y el contenido de los repositorios locales de AEM. Asegúrese de que cualquier código, configuración o contenido que no se deba destruir se conserva de forma segura con Git o se exporta desde la instancia local de AEM como paquetes de AEM.

### Evitar la pérdida de contenido al actualizar AEM SDK

La actualización de AEM SDK crea de manera efectiva un tiempo de ejecución de AEM completamente nuevo, que incluye un repositorio nuevo, lo que significa que se pierde cualquier cambio realizado en un repositorio de AEM SDK anterior. Las siguientes son estrategias viables para ayudar a mantener el contenido entre las actualizaciones de AEM SDK y se pueden utilizar de forma discreta o conjunta:

1. Cree un paquete de contenido dedicado a contener contenido de &quot;muestra&quot; para ayudar en el desarrollo y mantenerlo en Git. Cualquier contenido que deba persistir a través de las actualizaciones de AEM SDK se mantendrá en este paquete y se volverá a implementar después de actualizar AEM SDK.
1. Use [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la directiva `includepaths` para copiar contenido del repositorio anterior de AEM SDK en el nuevo repositorio de AEM SDK.
1. Haga una copia de seguridad de cualquier contenido mediante el Administrador de paquetes de AEM y los paquetes de contenido del SDK de AEM anterior y vuelva a instalarlos en el nuevo SDK de AEM.

Recuerde, el uso de los enfoques anteriores para mantener el código entre las actualizaciones de AEM SDK indica un antipatrón de desarrollo. El código no desechable debe originarse en el IDE de desarrollo y fluir a AEM SDK a través de implementaciones.

## Solución de problemas

### Al hacer doble clic en el archivo Jar de inicio rápido, se produce un error{#troubleshooting-double-click}

Al hacer doble clic en el Jar de inicio rápido para el inicio, se muestra un modal de error que impide que AEM se inicie localmente.

![Solución de problemas: haga doble clic en el archivo Jar de inicio rápido](./assets/aem-runtime/troubleshooting__double-click.png)

Esto se debe a que el Jar de inicio rápido de AEM as a Cloud Service no admite hacer doble clic en el Jar de inicio rápido para iniciar AEM localmente. En su lugar, debe ejecutar el archivo Jar desde esa línea de comandos.

Para iniciar el servicio de AEM Author, `cd` en el directorio que contiene el Jar de inicio rápido y ejecute el comando:

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

o bien, para iniciar el servicio Publicación de AEM, `cd` en el directorio que contiene el Jar de inicio rápido y ejecute el comando:

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

Al iniciar el Jar de inicio rápido desde la línea de comandos, el proceso se interrumpe inmediatamente y el servicio de AEM no se inicia, con el siguiente error:

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

Una vez instalado Oracle Java™ 11 SDK, compruebe que es la versión activa ejecutando el comando desde la línea de comandos:

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

+ [Descargar AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Documentación de Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es)

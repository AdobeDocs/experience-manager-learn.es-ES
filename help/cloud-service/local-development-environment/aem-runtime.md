---
title: Configuración del tiempo de ejecución de AEM local para AEM desarrollo as a Cloud Service
description: Configure el tiempo de ejecución del AEM local mediante el Jar de inicio rápido del SDK as a Cloud Service de AEM.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: 3a9615177acb5475d9b2b4ef22907c11e7da2bf7
workflow-type: tm+mt
source-wordcount: '1801'
ht-degree: 2%

---

# Configuración del tiempo de ejecución de AEM local

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Tiempo de ejecución de AEM local"
>abstract="Adobe Experience Manager (AEM) se puede ejecutar localmente mediante el Jar de inicio rápido del SDK as a Cloud Service de AEM. Esto permite a los desarrolladores implementar y probar el código, la configuración y el contenido personalizados antes de comprometerlo con el control de código fuente e implementarlo en un entorno as a Cloud Service AEM."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=es" text="SDK de AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html" text="Descargar AEM SDK as a Cloud Service"

Adobe Experience Manager (AEM) se puede ejecutar localmente mediante el Jar de inicio rápido del SDK as a Cloud Service de AEM. Esto permite a los desarrolladores implementar y probar el código, la configuración y el contenido personalizados antes de comprometerlo con el control de código fuente e implementarlo en un entorno as a Cloud Service AEM.

Tenga en cuenta que `~` se utiliza como abreviatura para el Directorio del usuario. En Windows, es el equivalente de `%HOMEPATH%`.

## Instalación de Java

Experience Manager es una aplicación Java y, por lo tanto, requiere el SDK de Java para admitir las herramientas de desarrollo.

1. [Descargar e instalar el último SDK de Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Compruebe que el SDK de Java 11 esté instalado ejecutando el comando :
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Descargar el SDK as a Cloud Service AEM

El SDK as a Cloud Service AEM, o SDK AEM, contiene el Jar de inicio rápido utilizado para ejecutar AEM Author y Publish localmente para el desarrollo, así como la versión compatible de las herramientas de Dispatcher.

1. Iniciar sesión en [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) con su Adobe ID
   + Tenga en cuenta que la organización de Adobe __must__ esté aprovisionado para AEM as a Cloud Service a descargar el SDK as a Cloud Service de AEM.
1. Vaya a la __AEM as a Cloud Service__ ficha
1. Ordenar por __Fecha de publicación__ en __Descendente__ pedido
1. Haga clic en la última __SDK AEM__ fila de resultados
1. Revise y acepte el EULA y pulse el botón __Descargar__ botón

## Extraer el Jar de inicio rápido del zip del SDK de AEM

1. Descomprima el `aem-sdk-XXX.zip` file

## Configuración del servicio local de AEM Author{#set-up-local-aem-author-service}

El servicio de creación de AEM local proporciona a los desarrolladores una experiencia local que los especialistas en marketing digital/autores de contenido compartirán para crear y administrar contenido.  El servicio de creación de AEM está diseñado como entorno de creación y previsualización, lo que permite realizar la mayoría de las validaciones del desarrollo de funciones en su contra, lo que lo convierte en un elemento vital del proceso de desarrollo local.

1. Crear la carpeta `~/aem-sdk/author`
1. Copie el __JAR de inicio rápido__ a  `~/aem-sdk/author` y cambie el nombre a `aem-author-p4502.jar`
1. Inicie el servicio local de AEM Author ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-author-p4502.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar el valor predeterminado para el desarrollo local para reducir la necesidad de volver a configurarla.

   You *cannot* iniciar AEM como Cloud Service Jar de inicio rápido [haciendo doble clic en](#troubleshooting-double-click).
1. Acceda al servicio local de AEM Author en [http://localhost:4502](http://localhost:4502) en un explorador Web

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

El servicio de publicación de AEM local proporciona a los desarrolladores la experiencia local que los usuarios finales del AEM tendrán, como navegar por el sitio web alojado en AEM. Un servicio de publicación local de AEM es importante, ya que se integra con AEM SDK [Herramientas de Dispatcher](./dispatcher-tools.md) y permite a los desarrolladores realizar pruebas de humo y ajustar la experiencia final de cara al usuario final.

1. Crear la carpeta `~/aem-sdk/publish`
1. Copie el __JAR de inicio rápido__ a  `~/aem-sdk/publish` y cambie el nombre a `aem-publish-p4503.jar`
1. Inicie el servicio de publicación de AEM local ejecutando lo siguiente desde la línea de comandos:
   + `java -jar aem-publish-p4503.jar`
      + Proporcione la contraseña de administrador como `admin`. Cualquier contraseña de administrador es aceptable, pero se recomienda utilizar el valor predeterminado para el desarrollo local para reducir la necesidad de volver a configurarla.

   You *cannot* iniciar AEM como Cloud Service Jar de inicio rápido [haciendo doble clic en](#troubleshooting-double-click).
1. Acceda al servicio de publicación local de AEM en [http://localhost:4503](http://localhost:4503) en un explorador Web

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

## Configuración de los servicios de AEM locales en el modo de prelanzamiento

El tiempo de ejecución de AEM local se puede iniciar en [modo de prelanzamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=es) permitir a un desarrollador compilar según las funciones de la próxima versión del as a Cloud Service AEM. La versión previa se habilita pasando la variable `-r prerelease` en el primer inicio del motor de ejecución de AEM local. Esto se puede utilizar con los servicios locales de AEM Author y AEM Publish.

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

## Simular la distribución de contenido {#content-distribution}

En un entorno de Cloud Service verdadero, el contenido se distribuye desde el servicio de creación al servicio de publicación mediante [Distribución de contenido de Sling](https://sling.apache.org/documentation/bundles/content-distribution.html) y la canalización de Adobe. La variable [Canalización de Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) es un microservicio aislado disponible solo en el entorno de la nube.

Durante el desarrollo, puede ser deseable simular la distribución de contenido mediante el servicio Autor y Publicación local. Esto se puede lograr habilitando los agentes de replicación heredados.

>[!NOTE]
>
> Los agentes de replicación solo están disponibles para su uso en el JAR de inicio rápido local y proporcionan solo una simulación de distribución de contenido.

1. Inicie sesión en el **Autor** y vaya a [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Haga clic en **Agente predeterminado (publicar)** para abrir el agente de replicación predeterminado.
1. Haga clic en **Editar** para abrir la configuración del agente.
1. En el **Configuración** , actualice los campos siguientes:

   + **Habilitado** - comprobar verdadero
   + **Id De Usuario Del Agente** - Deje este campo vacío

   ![Configuración del agente de replicación: configuración](assets/aem-runtime/settings-config.png)

1. En el **Transporte** , actualice los campos siguientes:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Usuario** - `admin`
   + **Contraseña** - `admin`

   ![Configuración del agente de replicación: transporte](assets/aem-runtime/transport-config.png)

1. Haga clic en **Ok** para guardar la configuración y habilitar la variable **Predeterminado** Agente de replicación.
1. Ahora puede realizar cambios en el contenido del servicio Autor y publicarlo en el servicio Publicar .

![Publicar página](assets/aem-runtime/publish-page-changes.png)

## Modos de inicio de Jar de inicio rápido

El nombre del Jar de inicio rápido, `aem-<tier>_<environment>-p<port number>.jar` especifica cómo se iniciará. Una vez AEM se ha iniciado en un nivel específico, se puede crear o publicar, no se puede cambiar al nivel alternativo. Para ello, el `crx-Quickstart` la carpeta generada durante la primera ejecución debe eliminarse, y el Jar de inicio rápido debe volver a ejecutarse. El entorno y los puertos se pueden cambiar, aunque requieren la detención/inicio de la instancia de AEM local.

Cambiar entornos, `dev`, `stage` y `prod`, puede resultar útil para los desarrolladores de para garantizar que las configuraciones específicas del entorno se definan y resuelvan correctamente por AEM. Se recomienda que el desarrollo local se realice principalmente frente al valor predeterminado `dev` modo de ejecución del entorno.

Las permutaciones disponibles son las siguientes:

| Nombre de archivo del Jar de inicio rápido | Descripción del modo |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Como autor en el modo de ejecución de desarrollo en el puerto 4502 |
| `aem-author_dev-p4502.jar` | Como Autor en el modo de ejecución Dev en el puerto 4502 (igual que `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Como autor en modo de ejecución de ensayo en el puerto 4502 |
| `aem-author_prod-p4502.jar` | Como Autor en modo de ejecución de producción en el puerto 4502 |
| `aem-publish-p4503.jar` | Como publicado en el modo de ejecución de desarrollo en el puerto 4503 |
| `aem-publish_dev-p4503.jar` | Como Publicar en el modo de ejecución de desarrollo en el puerto 4503 (igual que `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Como Publicado en modo de ejecución de ensayo en el puerto 4503 |
| `aem-publish_prod-p4503.jar` | Como Publicar en modo de ejecución de producción en el puerto 4503 |

Tenga en cuenta que el número de puerto puede ser cualquier puerto disponible en la máquina de desarrollo local, aunque por convención:

+ Puerto __4502__ se usa para __servicio local de AEM Author__
+ Puerto __4503__ se usa para __servicio local de AEM Publish__

Para cambiar estas opciones es posible que sea necesario realizar ajustes en AEM configuraciones del SDK

## Detención de un tiempo de ejecución de AEM local

Para detener un tiempo de ejecución de AEM local, ya sea AEM Author o Publish, abra la ventana de línea de comandos que se utilizó para iniciar el tiempo de ejecución de AEM y pulse `Ctrl-C`. Espere a que AEM cierre. Cuando se complete el proceso de apagado, el símbolo del sistema de la línea de comandos estará disponible.

## Tareas opcionales de configuración de tiempo de ejecución de AEM local

+ __Variables de entorno de configuración OSGi y variables secretas__ are [especialmente configurado para el tiempo de ejecución local de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development), en lugar de administrarlas usando la CLI de aio.

## Cuándo actualizar el Jar de inicio rápido

Actualice el SDK de AEM al menos mensualmente el último jueves de cada mes o poco después, que es la cadencia de lanzamiento de AEM &quot;versiones de funciones&quot; as a Cloud Service.

>[!WARNING]
>
> Actualizar el Jar de inicio rápido a una nueva versión requiere reemplazar el entorno de desarrollo local completo, lo que resulta en una pérdida de todo el código, la configuración y el contenido en los repositorios de AEM locales. Asegúrese de que cualquier código, configuración o contenido que no se deba destruir se comprometa de forma segura con Git o se exporta desde la instancia de AEM local como Paquetes AEM.

### Cómo evitar la pérdida de contenido al actualizar el SDK de AEM

La actualización del SDK de AEM está creando de forma efectiva un nuevo tiempo de ejecución de AEM, incluido un nuevo repositorio, lo que significa que se pierden los cambios realizados en un repositorio anterior del SDK de AEM. Las siguientes son estrategias viables para ayudar a mantener el contenido entre AEM actualizaciones del SDK y se pueden utilizar de forma discreta o concertada:

1. Cree un paquete de contenido dedicado a contener contenido de &quot;muestra&quot; para ayudar en el desarrollo y mantenerlo en Git. Cualquier contenido que se deba mantener mediante AEM actualizaciones del SDK se mantendría en este paquete y se volvería a implementar después de actualizar el SDK de AEM.
1. Uso [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) con la variable `includepaths` , para copiar contenido del repositorio AEM SDK anterior al nuevo repositorio AEM SDK.
1. Haga una copia de seguridad de cualquier contenido mediante AEM Administrador de paquetes y paquetes de contenido en el SDK de AEM anterior y vuelva a instalarlos en el nuevo SDK de AEM.

Recuerde que, con los enfoques anteriores para mantener el código entre AEM actualizaciones del SDK, indica un anti-patrón de desarrollo. El código no desechable debe originarse en el IDE de desarrollo y fluir a AEM SDK mediante implementaciones.

## Solución de problemas

### Al hacer doble clic en el archivo Jar de inicio rápido se produce un error{#troubleshooting-double-click}

Al hacer doble clic en el Jar de inicio rápido para iniciar, se muestra un modo de error que impide que el AEM se inicie localmente.

![Solución de problemas: haga doble clic en el archivo Jar de inicio rápido](./assets/aem-runtime/troubleshooting__double-click.png)

Esto se debe a que AEM Jar de inicio rápido as a Cloud Service no admite hacer doble clic en el Jar de inicio rápido para iniciar AEM localmente. En su lugar, debe ejecutar el archivo Jar desde esa línea de comandos.

Para iniciar el servicio de AEM Author, `cd` en el directorio que contiene el Jar de inicio rápido y ejecute el comando:

`$ java -jar aem-author-p4502.jar`

o, para iniciar el servicio AEM Publish, `cd` en el directorio que contiene el Jar de inicio rápido y ejecute el comando:

`$ java -jar aem-publish-p4503.jar`

### El inicio del Jar de inicio rápido desde la línea de comandos se anula inmediatamente.{#troubleshooting-java-8}

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

Esto se debe a que AEM as a Cloud Service requiere Java SDK 11 y está ejecutando una versión diferente, muy probablemente Java 8. Para resolver este problema, descargue e instale [SDK 11 de Java de oracle](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Una vez instalado el SDK 11 de Java, compruebe que sea la versión activa ejecutando lo siguiente desde la línea de comandos.

Una vez instalado el SDK de Java 11, compruebe que es la versión activa ejecutando el comando desde la línea de comandos:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Recursos adicionales

+ [Descargar AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Descargar Docker](https://www.docker.com/)
+ [Documentación de Dispatcher de Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es)

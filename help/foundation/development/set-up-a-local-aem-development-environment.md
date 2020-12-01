---
title: Configuración de un Entorno de desarrollo de AEM local
description: Guía para configurar un desarrollo local para Adobe Experience Manager, AEM. Abarca temas importantes de instalación local, Apache Maven, entornos de desarrollo integrados y depuración/resolución de problemas. Se analizan el desarrollo con Eclipse IDE, CRXDE-Lite, Visual Studio Code e IntelliJ.
version: 6.4, 6.5
feature: maven-archetype
topics: development
activity: develop
audience: developer
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '2590'
ht-degree: 1%

---


# Configuración de un Entorno de desarrollo de AEM local

Guía para configurar un desarrollo local para Adobe Experience Manager, AEM. Abarca temas importantes de instalación local, Apache Maven, entornos de desarrollo integrados y depuración/resolución de problemas. Se analiza el desarrollo con **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] y[!DNL IntelliJ]**.

## Información general

La configuración de un entorno de desarrollo local es el primer paso para el desarrollo de Adobe Experience Manager o AEM. Tómese el tiempo para configurar un entorno de desarrollo de calidad para aumentar su productividad y escribir mejor código, más rápido. Podemos dividir un entorno de desarrollo local AEM en cuatro áreas:

* Instancias de AEM locales
* [!DNL Apache Maven] proyecto
* Entornos de desarrollo integrado (IDE)
* Solución de problemas

## Instalar instancias de AEM locales

Cuando nos referimos a una instancia de AEM local, hablamos de una copia de Adobe Experience Manager que se está ejecutando en el equipo personal de un desarrollador. ***El desarrollo de*** AllAEM debe realizar el inicio escribiendo y ejecutando código en una instancia de AEM local.

Si es nuevo en AEM, se pueden instalar dos modos de ejecución básicos: ***Autor*** y ***Publicar***. El ***Autor*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) es el entorno que los especialistas en mercadotecnia digital utilizarán para crear y administrar contenido. Al desarrollar **la mayor parte** del tiempo, implementará código en una instancia de Autor. Esto le permite crear nuevas páginas, así como agregar y configurar componentes. AEM Sites es un CMS de creación WYSIWYG y, por lo tanto, la mayoría de CSS y JavaScript se pueden probar con una instancia de creación.

También es *código de prueba crítico* en una instancia local ***Publish***. La instancia de ***Publish*** es el entorno AEM con el que interactuarán los visitantes del sitio Web. Aunque la instancia ***Publish*** es la misma pila de tecnología que la instancia ***Author***, hay algunas distinciones importantes con configuraciones y permisos. El código debe *siempre* probarse con una instancia local ***Publish*** antes de pasar a entornos de nivel superior.

### Etapas

1. Asegúrese de que [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) está instalado.
   * Preferir [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout lista&amp;p.offset=0&amp;p.limit=14) para AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8)  para versiones AEM anteriores a AEM 6.5
2. Obtenga una copia de la [AEM barra de inicio rápido y una [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Cree una estructura de carpetas en el equipo como la siguiente:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Cambie el nombre del [!DNL QuickStart] JAR a ***aem-author-p4502.jar*** y colóquelo debajo del directorio `/author`. Añada el archivo ***[!DNL license.properties]*** debajo del directorio `/author`.
5. Haga una copia del JAR [!DNL QuickStart], cambie su nombre a ***aem-publish-p4503.jar*** y colóquelo debajo del directorio `/publish`. Añada una copia del archivo ***[!DNL license.properties]*** debajo del directorio `/publish`.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Haga clic con el doble en el archivo ***aem-author-p4502.jar*** para instalar la instancia **Author**. Esto inicio la instancia de creación, que se ejecuta en el puerto **4502** del equipo local.

   Haga clic con el doble en el archivo ***aem-publish-p4503.jar*** para instalar la instancia de **Publish**. Esto inicio la instancia de Publish, que se ejecuta en el puerto **4503** del equipo local.

   >[!NOTE]
   >
   >Según el hardware de su equipo de desarrollo, puede ser difícil tener una instancia de **Autor y Publicar** ejecutándose al mismo tiempo. Raramente necesita ejecutar ambos simultáneamente en una configuración local.

   Para obtener más información, consulte [Implementación y mantenimiento de una instancia de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Instalar Apache Maven

***[!DNL Apache Maven]*** es una herramienta para administrar el procedimiento de compilación e implementación para proyectos basados en Java. AEM es una plataforma basada en Java y [!DNL Maven] es la manera estándar de administrar el código de un proyecto AEM. Cuando decimos ***AEM proyecto Maven*** o sólo su ***proyecto AEM***, nos referimos a un proyecto Maven que incluye todo el *código personalizado* para su sitio.

Todos los proyectos AEM deben crearse a partir de la versión más reciente del **[!DNL AEM Project Archetype]**: [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). El [!DNL AEM Project Archetype] creará un bootstrap de un proyecto AEM con algún código de muestra y contenido. El [!DNL AEM Project Archetype] también incluye **[!DNL AEM WCM Core Components]** configurado para utilizarse en el proyecto.

>[!CAUTION]
>
>Al iniciar un nuevo proyecto, se recomienda utilizar la versión más reciente del arquetipo. Tenga en cuenta que hay varias versiones del arquetipo y que no todas las versiones son compatibles con versiones anteriores de AEM.

### Etapas

1. Descargar [Apache Maven](https://maven.apache.org/download.cgi)
2. Instale [Apache Maven](https://maven.apache.org/install.html) y asegúrese de que la instalación se haya agregado a la línea de comandos `PATH`.
   * [!DNL macOS] los usuarios pueden instalar Maven mediante  [Homebrew](https://brew.sh/)
3. Compruebe que **[!DNL Maven]** está instalado abriendo un nuevo terminal de línea de comandos y ejecutando lo siguiente:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. Añada el perfil **[!DNL adobe-public]** en el archivo [!DNL Maven] [settings.xml](https://maven.apache.org/settings.html) para agregar automáticamente **[!DNL repo.adobe.com]** al proceso de generación de lotes.

5. Cree un archivo con el nombre `settings.xml` en `~/.m2/settings.xml` si no existe ya.

6. Añada el perfil **[!DNL adobe-public]** en el archivo `settings.xml` basado en [las instrucciones aquí](https://repo.adobe.com/).

   A continuación se muestra un ejemplo `settings.xml`. *Tenga en cuenta que es importante la convención de nombres  `settings.xml` y la ubicación debajo del  `.m2` directorio del usuario.*

   ```xml
   <settings xmlns="https://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://maven.apache.org/SETTINGS/1.0.0
                         https://maven.apache.org/xsd/settings-1.0.0.xsd">
   <profiles>
    <!-- ====================================================== -->
    <!-- A D O B E   P U B L I C   P R O F I L E                -->
    <!-- ====================================================== -->
        <profile>
            <id>adobe-public</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <releaseRepository-Id>adobe-public-releases</releaseRepository-Id>
                <releaseRepository-Name>Adobe Public Releases</releaseRepository-Name>
                <releaseRepository-URL>https://repo.adobe.com/nexus/content/groups/public</releaseRepository-URL>
            </properties>
            <repositories>
                <repository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
   </profiles>
    <activeProfiles>
        <activeProfile>adobe-public</activeProfile>
    </activeProfiles>
   </settings>
   ```

7. Compruebe que el perfil **adobe-public** está activo ejecutando el siguiente comando:

   ```shell
   $ mvn help:effective-settings
   ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Si no ve el **[!DNL adobe-public]** es una indicación de que no se hace referencia a la repo de Adobe en el archivo `~/.m2/settings.xml`. Revise los pasos anteriores y compruebe que el archivo settings.xml hace referencia a la repo de Adobe.

## Configurar un Entorno de desarrollo integrado

Un entorno de desarrollo integrado o IDE es una aplicación que combina un editor de texto, compatibilidad con sintaxis y herramientas de creación. Según el tipo de desarrollo que esté realizando, un IDE puede ser preferible a otro. Independientemente del IDE, será importante poder insertar ***código*** periódicamente en una instancia de AEM local para probarlo. También será importante que ocasionalmente ***extraiga*** configuraciones de una instancia de AEM local en su proyecto AEM para poder persistir en un sistema de administración de control de código fuente como Git.

A continuación se muestran algunos de los IDE más populares que se utilizan con AEM desarrollo con los vídeos correspondientes que muestran la integración con una instancia de AEM local.

### [!DNL Eclipse] IDE

El **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** es uno de los IDE más populares para el desarrollo de Java, en gran parte porque es de código abierto y ***libre***! Adobe proporciona un complemento, **[[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**, para [!DNL Eclipse], a fin de facilitar el desarrollo con una interfaz gráfica de usuario agradable para sincronizar el código con una instancia de AEM local. El IDE [!DNL Eclipse] se recomienda para desarrolladores nuevos que AEM en gran parte debido a la compatibilidad con GUI de [!DNL AEM Developer Tools].

#### Instalación y configuración

1. Descargue e instale el IDE [!DNL Eclipse] para [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga las instrucciones para instalar el complemento [!DNL AEM Developer Tools]: [https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importar proyecto Maven
* 01:24 - Generar e implementar código fuente con Maven
* 04:33 - Cambios en el código push con AEM herramienta para desarrolladores
* 10:55 - Extraer cambios de código con AEM herramienta para desarrolladores
* 13:12 - Uso de las herramientas de depuración integradas de Eclipse

### IntelliJ IDEA

El **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** es un potente IDE para el desarrollo profesional de Java. [!DNL IntelliJ IDEA] viene en dos sabores, una  ****** [!DNL Community] libre edición y una  [!DNL Ultimate] versión comercial (de pago). La versión gratuita [!DNL Community] de [!DNL IntellIJ IDEA] es suficiente para un mayor desarrollo AEM, pero [!DNL Ultimate] [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Descargue e instale el [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instalar [!DNL Repo] (herramienta de línea de comandos): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Importar proyecto Maven
* 05:47 - Genere e implemente código fuente con Maven
* 08:17 - Cambios push con Repo
* 14:39 - Cambios de extracción con Repo
* 17:25 - Uso de las herramientas de depuración integradas de IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio ](https://code.visualstudio.com/)** Code se ha convertido rápidamente en una herramienta favorita para  ***los desarrolladores*** front-end con compatibilidad mejorada con JavaScript  [!DNL Intellisense]y depuración de exploradores. **[!DNL Visual Studio Code]** es de código abierto, libre, con muchas extensiones poderosas. [!DNL Visual Studio Code] puede configurarse para integrarse con AEM con la ayuda de una herramienta de Adobe,  **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** También hay varias extensiones admitidas por la comunidad que se pueden instalar para integrar con AEM.

[!DNL Visual Studio Code] es una buena opción para desarrolladores de front-end que escribirán principalmente código CSS/LESS y JavaScript para crear AEM bibliotecas de clientes. Es posible que esta herramienta no sea la mejor opción para los nuevos desarrolladores de AEM, ya que las definiciones de nodos (cuadros de diálogo, componentes) deberán editarse en XML sin procesar. Existen varias extensiones de Java disponibles para [!DNL Visual Studio Code], sin embargo, si se prefiere principalmente el desarrollo de Java [!DNL Eclipse IDE] o [!DNL IntelliJ].

#### Vínculos importantes

* [****](https://code.visualstudio.com/Download) **DescargarCódigo de Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** : herramienta similar a FTP para contenido JCR
* **[aemfeed](https://aemfed.io/)** : acelere su flujo de trabajo AEM front-end
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** : extensión* de comunidad admitida para código de Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importar proyecto Maven
* 00:53 - Genere e implemente código fuente con Maven
* 04:03 - Cambios en el código push con la herramienta de línea de comandos Repo
* 08:29 - Cambios en el código de extracción con la herramienta de línea de comandos Repo
* 10:40 - Cambios en el código push con la herramienta de alimentación por correo electrónico
* 14:24 - Resolución de problemas, reconstruir bibliotecas de clientes

### [!DNL CRXDE Lite]

[CRXDE ](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) Litoral es una vista basada en navegador del repositorio de AEM. [!DNL CRXDE Lite] está integrado en AEM y permite a los desarrolladores realizar tareas de desarrollo estándar como editar archivos, definir componentes, cuadros de diálogo y plantillas. [!DNL CRXDE Lite] no está  ****** pensado para ser un entorno de desarrollo completo, pero es muy eficaz como herramienta de depuración. [!DNL CRXDE Lite] resulta útil para ampliar o simplemente comprender el código de producto fuera de la base de código. [!DNL CRXDE Lite] proporciona una poderosa vista del repositorio y una manera de probar y administrar los permisos de manera eficaz.

[!DNL CRXDE Lite] siempre debe usarse junto con otros IDE para probar y depurar código pero nunca como la herramienta de desarrollo principal. Cuenta con compatibilidad con sintaxis limitada, sin funciones de autocompletar e integración limitada con sistemas de administración de control de código fuente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Solución de problemas

***Ayuda!*** ¡Mi código no funciona! Como con todo el desarrollo, habrá veces (probablemente muchas), en las que el código no funcionará según lo esperado. AEM es una plataforma poderosa, pero con bueno poder... viene la buena complejidad. A continuación se presentan algunos puntos de partida de alto nivel en lo que respecta a la resolución de problemas y el seguimiento de problemas (pero lejos de una lista exhaustiva de las cosas que pueden salir mal):

### Verificar implementación de código

Un buen primer paso, al encontrar un problema, es verificar que el código se haya implementado e instalado correctamente en AEM.

1. **Compruebe  [!UICONTROL Package]** Manager para asegurarse de que el paquete de código se ha cargado e instalado:  [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Compruebe la marca de hora para comprobar que el paquete se ha instalado recientemente.
1. Si realiza actualizaciones incrementales de archivos con una herramienta como [!DNL Repo] o [!DNL AEM Developer Tools], **marque[!DNL CRXDE Lite]** que el archivo se ha insertado en la instancia de AEM local y que el contenido del archivo se ha actualizado: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Compruebe que el paquete se** carga si se producen problemas relacionados con el código Java en un paquete OSGi. Abra la [!UICONTROL Consola Web de Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) y busque el paquete. Asegúrese de que el paquete tiene un estado **[!UICONTROL Activo]**. Consulte a continuación para obtener más información relacionada con la resolución de problemas de un paquete en un estado **[!UICONTROL Instalado]**.

#### Comprobación de los registros

AEM es una plataforma chatty y registra mucha información útil en el **error.log**. Se puede encontrar **error.log** donde se ha instalado AEM: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Una técnica útil para rastrear problemas es agregar sentencias de registro en el código Java:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

De forma predeterminada, **error.log** está configurado para registrar *[!DNL INFO]* sentencias. Si desea cambiar el nivel de registro, puede hacerlo si ingresa a [!UICONTROL Log Support]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). También puede encontrar que el **error.log** es demasiado parloteo. Puede utilizar la [!UICONTROL Compatibilidad con registros] para configurar las sentencias de registro para un paquete Java específico. Se trata de una práctica recomendada para los proyectos, con el fin de separar fácilmente los problemas de código personalizado de los problemas de la plataforma de AEM OOTB.

![Registrando configuración en AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### El paquete está en estado Instalado {#bundle-active}

Todos los paquetes (excepto Fragments) deben estar en un estado **[!UICONTROL Activo]**. Si ve el paquete de código en un estado [!UICONTROL Instalado], hay un problema que debe resolverse. La mayoría de las veces se trata de un problema de dependencia:

![Error del paquete en AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

En la captura de pantalla anterior, el [!DNL WKND Core bundle] es un estado [!UICONTROL Instalado]. Esto se debe a que el paquete espera una versión de `com.adobe.cq.wcm.core.components.models` diferente de la disponible en la instancia de AEM.

Una herramienta útil que se puede utilizar es el [!UICONTROL Buscador de dependencias]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Añada el nombre del paquete Java para inspeccionar qué versión está disponible en la instancia de AEM:

![Componentes principales](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando con el ejemplo anterior, podemos ver que la versión instalada en la instancia de AEM es **12.2** vs **12.6** que el paquete esperaba. Desde allí puede trabajar hacia atrás y ver si las dependencias [!DNL Maven] de AEM coinciden con las dependencias [!DNL Maven] del proyecto AEM. En el ejemplo anterior, [!DNL Core Components] **v2.2.0** está instalado en la instancia de AEM, pero el paquete de código se creó con una dependencia de **v2.2.2**, de ahí el motivo del problema de dependencia.

#### Verificar registro de modelos Sling {#osgi-component-sling-models}

AEM componentes siempre deben estar respaldados por un [!DNL Sling Model] para encapsular cualquier lógica empresarial y garantizar que el script de procesamiento HTL permanezca limpio. Si se producen problemas en los que no se puede encontrar el modelo Sling, puede resultar útil comprobar el [!DNL Sling Models] desde la consola: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Esto le indicará si el modelo Sling se ha registrado y a qué tipo de recurso (la ruta de componente) está vinculado.

![Estado del modelo Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Muestra el registro de un [!DNL Sling Model], `BylineImpl` que está vinculado a un tipo de recurso de componente de `wknd/components/content/byline`.

#### Problemas de CSS o JavaScript

Para la mayoría de los problemas de CSS y JavaScript, el uso de las herramientas de desarrollo del explorador es la manera más eficaz de solucionar los problemas. Para reducir el problema cuando se desarrolla con una instancia de autor AEM, es útil realizar una vista de la página &quot;tal como se ha publicado&quot;.

![Problemas de CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Abra el menú [!UICONTROL Propiedades de la página] y haga clic en [!UICONTROL Vista como Publicada]. Se abrirá la página sin el editor de AEM y con un parámetro de consulta establecido en **wcmmode=disabled**. Esto deshabilitará de forma efectiva la interfaz de usuario de creación de AEM y facilitará la solución de problemas y la depuración de problemas del front-end.

Otro problema que se encuentra con frecuencia al desarrollar código de front-end es que CSS/JS antiguos o obsoletos se están cargando. Como primer paso, asegúrese de que se ha borrado el historial del explorador y, si es necesario, de que se produzca un inicio en los navegadores incógnito o en una nueva sesión.

#### Depuración de bibliotecas de cliente

Con diferentes métodos de categorías e incrustaciones para incluir varias bibliotecas de cliente, puede resultar complicado solucionar problemas. AEM expone varias herramientas para ayudar con esto. Una de las herramientas más importantes es [!UICONTROL Reconstruir bibliotecas de clientes], lo que obligará a AEM a volver a compilar cualquier archivo LESS y generar el CSS.

* [Libros](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  de volcado: Lista todas las bibliotecas de cliente registradas en la instancia de AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Salida](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  de prueba: permite al usuario ver la salida HTML esperada de clientlib incluye en función de la categoría. &lt;host>/libs/granite/ui/content/dumplibs.test
* [Validación](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  de dependencias de bibliotecas: resalta las dependencias o categorías incrustadas que no se pueden encontrar. &lt;host>/libs/granite/ui/content/dumplibs.validate
* [Reconstruir bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  de cliente: permite al usuario forzar a AEM a reconstruir todas las bibliotecas de cliente o invalidar la caché de las bibliotecas de cliente. Esta herramienta es especialmente eficaz cuando se desarrolla con LESS, ya que esto puede obligar a AEM a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y, a continuación, actualizar la página en lugar de volver a compilar todas las bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![Depuración de Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Si constantemente tiene que invalidar la caché mediante la herramienta [!UICONTROL Reconstruir bibliotecas de clientes], puede que valga la pena realizar una reconstrucción única de todas las bibliotecas de clientes. Esto puede tardar unos 15 minutos, pero normalmente elimina cualquier problema de almacenamiento en caché en el futuro.

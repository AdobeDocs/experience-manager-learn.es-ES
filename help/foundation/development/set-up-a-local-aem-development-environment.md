---
title: Configuración de un Entorno de desarrollo de AEM local
description: Guía para configurar un desarrollo local para Adobe Experience Manager, AEM. Abarca temas importantes de instalación local, Apache Maven, entornos de desarrollo integrados y depuración/solución de problemas. Se trata el desarrollo con Eclipse IDE, CRXDE-Lite, Visual Studio Code e IntelliJ.
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '2658'
ht-degree: 2%

---


# Configuración de un Entorno de desarrollo de AEM local

Guía para configurar un desarrollo local para Adobe Experience Manager, AEM. Abarca temas importantes de instalación local, Apache Maven, entornos de desarrollo integrados y depuración/solución de problemas. Se analiza el desarrollo con **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] y[!DNL IntelliJ]**.

## Información general

La configuración de un entorno de desarrollo local es el primer paso al desarrollar para Adobe Experience Manager o AEM. Tómese el tiempo necesario para configurar un entorno de desarrollo de calidad con el fin de aumentar su productividad y escribir mejor código más rápido. Podemos dividir un entorno de desarrollo local AEM en 4 áreas:

* Instancias de AEM locales
* [!DNL Apache Maven] proyecto
* Entornos de desarrollo integrados (IDE)
* Solución de problemas

## Instalar instancias AEM locales

Cuando nos referimos a una instancia de AEM local, hablamos de una copia de Adobe Experience Manager que se está ejecutando en el equipo personal de un desarrollador. ****** El desarrollo de todo AEM debe comenzar escribiendo y ejecutando código en una instancia de AEM local.

Si es nuevo en AEM, se pueden instalar dos modos de ejecución básicos: ***Autor*** y ***Publicar***. El ***Author*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) es el entorno que los especialistas en marketing digital utilizarán para crear y administrar el contenido. Al desarrollar **la mayoría** del tiempo, implementará código en una instancia de Autor. Esto le permite crear nuevas páginas, así como añadir y configurar componentes. AEM Sites es un CMS WYSIWYG de creación y, por lo tanto, la mayoría de CSS y JavaScript se pueden probar con una instancia de creación.

También es código de prueba *crítico* frente a una instancia local ***Publish***. La instancia ***Publish*** es el entorno AEM con el que interactuarán los visitantes del sitio web. Aunque la instancia ***Publish*** es la misma pila de tecnología que la instancia ***Author***, hay algunas distinciones importantes con configuraciones y permisos. El código debe *siempre* probarse con una instancia ***Publish*** local antes de promocionarse a entornos de nivel superior.

### Etapas

1. Asegúrese de que [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) está instalado.
   * Prefiera [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=14) para AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) para versiones AEM anteriores a AEM 6.5
2. Obtenga una copia del [AEM Jar de inicio rápido y a [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Cree una estructura de carpetas en el equipo como la siguiente:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Cambie el nombre del JAR [!DNL QuickStart] a ***aem-author-p4502.jar*** y colóquelo debajo del directorio `/author`. Añada el archivo ***[!DNL license.properties]*** debajo del directorio `/author`.
5. Haga una copia del JAR [!DNL QuickStart], renómbrelo a ***aem-publish-p4503.jar*** y colóquelo debajo del directorio `/publish`. Agregue una copia del archivo ***[!DNL license.properties]*** debajo del directorio `/publish`.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Haga doble clic en el archivo ***aem-author-p4502.jar*** para instalar la instancia **Author**. Esto iniciará la instancia de autor, ejecutándose en el puerto **4502** del equipo local.

   Haga doble clic en el archivo ***aem-publish-p4503.jar*** para instalar la instancia **Publish**. Esto iniciará la instancia de publicación, que se ejecuta en el puerto **4503** del equipo local.

   >[!NOTE]
   >
   >Dependiendo del hardware de su equipo de desarrollo, puede ser difícil tener una instancia **Author y Publish** ejecutándose al mismo tiempo. Raramente necesita ejecutar ambos simultáneamente en una configuración local.

   Para obtener más información, consulte [Implementación y mantenimiento de una instancia de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Instalar Apache Maven

***[!DNL Apache Maven]*** es una herramienta para administrar el procedimiento de compilación e implementación para proyectos basados en Java. AEM es una plataforma basada en Java y [!DNL Maven] es la manera estándar de administrar el código de un proyecto AEM. Cuando decimos ***AEM proyecto Maven*** o simplemente su ***AEM proyecto***, nos referimos a un proyecto Maven que incluye todo el código *personalizado* de su sitio.

Todos los proyectos AEM deben crearse a partir de la última versión de **[!DNL AEM Project Archetype]**: [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). El [!DNL AEM Project Archetype] creará un bootstrap de un proyecto AEM con código de muestra y contenido. El [!DNL AEM Project Archetype] también incluye **[!DNL AEM WCM Core Components]** configurado para utilizarse en el proyecto.

>[!CAUTION]
>
>Al iniciar un nuevo proyecto, se recomienda utilizar la versión más reciente del tipo de archivo. Tenga en cuenta que hay varias versiones del tipo de archivo y que no todas las versiones son compatibles con versiones anteriores de AEM.

### Etapas

1. Descargar [Apache Maven](https://maven.apache.org/download.cgi)
2. Instale [Apache Maven](https://maven.apache.org/install.html) y asegúrese de que la instalación se haya agregado a la línea de comandos `PATH`.
   * [!DNL macOS] Los usuarios pueden instalar Maven mediante  [Homebrew](https://brew.sh/)
3. Compruebe que **[!DNL Maven]** está instalado abriendo un nuevo terminal de línea de comandos y ejecutando lo siguiente:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. Agregue el perfil **[!DNL adobe-public]** a su archivo [!DNL Maven] [settings.xml](https://maven.apache.org/settings.html) para agregar automáticamente **[!DNL repo.adobe.com]** al proceso de compilación de maven.

5. Cree un archivo con el nombre `settings.xml` en `~/.m2/settings.xml` si aún no existe.

6. Añada el perfil **[!DNL adobe-public]** al archivo `settings.xml` en función de [las instrucciones aquí](https://repo.adobe.com/).

   A continuación se muestra una muestra `settings.xml`. *Tenga en cuenta que es importante la convención de nombres  `settings.xml` y la colocación debajo del  `.m2` directorio del usuario.*

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

7. Compruebe que el perfil **adobe-public** esté activo ejecutando el siguiente comando:

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

   Si no ve el **[!DNL adobe-public]** esto indica que no se hace referencia al repositorio de Adobe correctamente en su archivo `~/.m2/settings.xml`. Vuelva a los pasos anteriores y compruebe que el archivo settings.xml hace referencia al repositorio de Adobe.

## Configurar un entorno de desarrollo integrado

Un entorno de desarrollo integrado o IDE es una aplicación que combina un editor de texto, compatibilidad con sintaxis y herramientas de compilación. Dependiendo del tipo de desarrollo que esté realizando, un IDE podría ser preferible sobre otro. Independientemente del IDE, será importante poder insertar el código ***periódicamente*** en una instancia de AEM local para probarlo. También es importante extraer ocasionalmente ***configuraciones*** de una instancia de AEM local al proyecto de AEM para poder persistir en un sistema de administración de control de código fuente como Git.

A continuación, se muestran algunos de los IDE más populares que se utilizan con AEM desarrollo con los vídeos correspondientes que muestran la integración con una instancia de AEM local.

>[!NOTE]
>
> El proyecto WKND se ha actualizado de forma predeterminada para que funcione en AEM como Cloud Service. Se ha actualizado para que sea [compatible con 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Cuando utilice un IDE, asegúrese de marcar `classic` en la pestaña Maven Profile .

![Pestaña Maven Profile](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Perfil de maven IntelliJ*

### [!DNL Eclipse] IDE

El **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** es uno de los IDE más populares para el desarrollo de Java, en gran parte porque es de código abierto y ***gratuito***. Adobe proporciona un plugin, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**, para [!DNL Eclipse], que permite un desarrollo más fácil con una buena GUI para sincronizar código con una instancia de AEM local. El [!DNL Eclipse] IDE es recomendado para desarrolladores que no tienen experiencia en AEM, en gran parte debido a la compatibilidad con GUI de [!DNL AEM Developer Tools].

#### Instalación y configuración

1. Descargue e instale el [!DNL Eclipse] IDE para [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga las instrucciones para instalar el complemento [!DNL AEM Developer Tools]: [https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importar proyecto Maven
* 01:24 - Generar e implementar código fuente con Maven
* 04:33 - Cambios en el código push con AEM herramienta para desarrolladores
* 10:55 - Extraer cambios de código con AEM herramienta para desarrolladores
* 13:12 - Uso de las herramientas de depuración integradas de Eclipse

### IntelliJ IDEA

El **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** es un potente IDE para el desarrollo profesional de Java. [!DNL IntelliJ IDEA] viene en dos sabores, una  ****** [!DNL Community] edición libre y una  [!DNL Ultimate] versión comercial (de pago). La versión libre [!DNL Community] de [!DNL IntellIJ IDEA] es suficiente para un desarrollo más AEM, pero [!DNL Ultimate] [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Descargue e instale el [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instale [!DNL Repo] (herramienta de línea de comandos): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00: Importar proyecto de Maven
* 05:47 - Generar e implementar código fuente con Maven
* 08:17 - Cambios push con Repo
* 14:39 - Cambios de extracción con Repo
* 17:25 - Uso de las herramientas de depuración integradas de IntelliJ IDEA

### [!DNL Visual Studio Code]

**[El código de Visual Studio ](https://code.visualstudio.com/)** se ha convertido rápidamente en una herramienta favorita para  ***desarrolladores de front-end con compatibilidad con JavaScript mejorada***   [!DNL Intellisense]y con depuración de explorador. **[!DNL Visual Studio Code]** es de código abierto, gratuito, con muchas extensiones potentes. [!DNL Visual Studio Code] se puede configurar para integrarlo con AEM con la ayuda de una herramienta de Adobe,  **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** También hay varias extensiones compatibles con la comunidad que se pueden instalar para integrar con AEM.

[!DNL Visual Studio Code] es una buena opción para desarrolladores de front-end que escribirán principalmente código CSS/LESS y JavaScript para crear bibliotecas de cliente AEM. Esta herramienta puede no ser la mejor opción para los desarrolladores de AEM nuevos, ya que las definiciones de nodos (cuadros de diálogo, componentes) deberán editarse en XML sin procesar. Hay varias extensiones de Java disponibles para [!DNL Visual Studio Code], sin embargo, si se prefiere principalmente hacer desarrollo de Java [!DNL Eclipse IDE] o [!DNL IntelliJ] puede ser preferible.

#### Vínculos importantes

* [****](https://code.visualstudio.com/Download) **Descargar código de Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** : herramienta parecida a FTP para contenido JCR
* **[aemfeed](https://aemfed.io/)** : acelere su flujo de trabajo front-end AEM
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** : extensión compatible con la comunidad* para código de Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importar proyecto Maven
* 00:53 - Generar e implementar código fuente con Maven
* 04:03 - Cambios en el código push con la herramienta de línea de comandos Repo
* 08:29 - Extraer cambios de código con la herramienta de línea de comandos Repo
* 10:40 - Cambios en el código push con la herramienta aemfeed
* 14:24 - Resolución de problemas, reconstrucción de bibliotecas de cliente

### [!DNL CRXDE Lite]

[CRXDE ](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) Lites una vista del repositorio de AEM basada en navegador. [!DNL CRXDE Lite] está incrustado en AEM y permite a un desarrollador realizar tareas de desarrollo estándar como editar archivos, definir componentes, cuadros de diálogo y plantillas. [!DNL CRXDE Lite] no  ****** pretende ser un entorno de desarrollo completo, pero es muy eficaz como herramienta de depuración. [!DNL CRXDE Lite] es útil para ampliar o simplemente comprender el código de producto fuera de la base de código. [!DNL CRXDE Lite] proporciona una vista poderosa del repositorio y una forma de probar y administrar los permisos de forma eficaz.

[!DNL CRXDE Lite] siempre debe usarse junto con otros IDE para probar y depurar código, pero nunca como la herramienta de desarrollo principal. Tiene compatibilidad con sintaxis limitada, no capacidades de autocompletar e integración limitada con sistemas de administración de control de código fuente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Solución de problemas

***Ayuda!*** ¡Mi código no funciona! Al igual que con todo el desarrollo, habrá momentos (probablemente muchos) en los que el código no funciona como se espera. AEM es una plataforma poderosa, pero con bueno poder... viene la complejidad buena. A continuación se presentan algunos puntos de partida de alto nivel en lo que respecta a la resolución de problemas y el seguimiento de problemas (pero lejos de una lista exhaustiva de cosas que pueden salir mal):

### Verificar implementación de código

Un buen primer paso, al encontrar un problema, es verificar que el código se haya implementado e instalado correctamente en AEM.

1. **Compruebe  [!UICONTROL el]** Administrador de paquetes para asegurarse de que el paquete de código se ha cargado e instalado:  [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Compruebe la marca de tiempo para comprobar que el paquete se ha instalado recientemente.
1. Si realiza actualizaciones de archivos incrementales con una herramienta como [!DNL Repo] o [!DNL AEM Developer Tools], **compruebe[!DNL CRXDE Lite]** que el archivo se ha insertado en la instancia de AEM local y que el contenido del archivo se ha actualizado: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Compruebe que el paquete se** cargue si aparece algún problema relacionado con el código Java en un paquete OSGi. Abra la [!UICONTROL Consola web de Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) y busque su paquete. Asegúrese de que el paquete tiene el estado **[!UICONTROL Active]**. Consulte a continuación para obtener más información relacionada con la resolución de problemas de un paquete en un estado **[!UICONTROL Installed]**.

#### Comprobación de los registros

AEM es una plataforma chatty y registra mucha información útil en el **error.log**. Se puede encontrar el **error.log** donde se ha instalado AEM: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Una técnica útil para rastrear problemas es agregar instrucciones de registro en el código Java:

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

De forma predeterminada, el archivo **error.log** está configurado para registrar las instrucciones *[!DNL INFO]*. Si desea cambiar el nivel de registro, puede hacerlo accediendo a [!UICONTROL Log Support]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). También puede encontrar que el **error.log** es demasiado parloteo. Puede utilizar el [!UICONTROL Log Support] para configurar las sentencias de registro únicamente para un paquete Java específico. Esta es una práctica recomendada para los proyectos, para separar fácilmente los problemas de código personalizado de los problemas de la plataforma de AEM OOTB.

![Configuración de registro en AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### El paquete está en estado de instalación {#bundle-active}

Todos los paquetes (excepto Fragmentos) deben estar en estado **[!UICONTROL Activo]**. Si ve el paquete de código en un estado [!UICONTROL Installed], hay un problema que debe resolverse. La mayoría de las veces se trata de un problema de dependencia:

![Error de paquete en AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

En la captura de pantalla anterior, [!DNL WKND Core bundle] es un estado [!UICONTROL Installed]. Esto se debe a que el paquete espera una versión de `com.adobe.cq.wcm.core.components.models` diferente de la disponible en la instancia de AEM.

Una herramienta útil que se puede utilizar es el [!UICONTROL Buscador de dependencias]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Añada el nombre del paquete Java para inspeccionar qué versión está disponible en la instancia de AEM:

![Componentes principales](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando con el ejemplo anterior, podemos ver que la versión instalada en la instancia de AEM es **12.2** frente a **12.6** que el paquete esperaba. Desde allí puede trabajar hacia atrás y ver si las dependencias [!DNL Maven] de AEM coinciden con las dependencias [!DNL Maven] del proyecto de AEM. En el ejemplo anterior [!DNL Core Components] **v2.2.0** está instalado en la instancia de AEM, pero el paquete de código se creó con una dependencia de **v2.2.2**, de ahí el motivo del problema de dependencia.

#### Verificación del registro de los modelos Sling {#osgi-component-sling-models}

AEM componentes siempre deben estar respaldados por un [!DNL Sling Model] para encapsular cualquier lógica empresarial y garantizar que el script de renderización HTL permanezca limpio. Si se producen problemas en los que no se encuentra el modelo Sling, puede ser útil comprobar el [!DNL Sling Models] desde la consola: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Esto le indicará si el modelo de Sling se ha registrado y a qué tipo de recurso (la ruta del componente) está vinculado.

![Estado del modelo Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Muestra el registro de un [!DNL Sling Model], `BylineImpl` que está vinculado a un tipo de recurso de componente `wknd/components/content/byline`.

#### Problemas de CSS o JavaScript

Para la mayoría de los problemas de CSS y JavaScript, el uso de las herramientas de desarrollo del explorador es la forma más eficaz de solucionar problemas. Para reducir el problema al desarrollar con una instancia de autor AEM, es útil ver la página &quot;tal y como aparece publicado&quot;.

![Problemas de CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Abra el menú [!UICONTROL Propiedades de página] y haga clic en [!UICONTROL Ver tal y como aparece publicado]. Se abrirá la página sin el editor de AEM y con un parámetro de consulta establecido en **wcmmode=disabled**. Esto deshabilitará la interfaz de usuario de creación de AEM y facilitará la resolución de problemas/depuración de los problemas del front-end.

Otro problema que se encuentra con frecuencia al desarrollar código front-end es que CSS/JS antiguos o obsoletos se están cargando. Como primer paso, asegúrese de que el historial del explorador se haya borrado y, si es necesario, inicie un incógnito en los navegadores o en una nueva sesión.

#### Depuración de bibliotecas de cliente

Con diferentes métodos de categorías e incrustaciones para incluir varias bibliotecas de cliente, puede resultar complicado solucionar problemas. AEM expone varias herramientas para ayudarle con esto. Una de las herramientas más importantes es [!UICONTROL Reconstruir bibliotecas de cliente], lo que obligará a AEM a volver a compilar cualquier archivo LESS y generar el CSS.

* [Bibliotecas de volcado](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : enumera todas las bibliotecas de cliente registradas en la instancia de AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Salida de prueba](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permite al usuario ver la salida HTML esperada de clientlib incluye en función de la categoría. &lt;host>/libs/granite/ui/content/dumplibs.test
* [Validación de dependencias de bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : resalta las dependencias o categorías incrustadas que no se pueden encontrar. &lt;host>/libs/granite/ui/content/dumplibs.validate
* [Reconstruir bibliotecas de cliente](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : permite al usuario forzar a la AEM a reconstruir todas las bibliotecas de cliente o invalidar la caché de las bibliotecas de cliente. Esta herramienta es especialmente eficaz cuando se desarrolla con LESS, ya que esto puede forzar a AEM a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y luego realizar una actualización de página en comparación con la reconstrucción de todas las bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![Depuración de Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Si tiene que invalidar constantemente la caché utilizando la herramienta [!UICONTROL Reconstruir bibliotecas de cliente], puede que valga la pena hacer una única reconstrucción de todas las bibliotecas de cliente. Esto puede tardar unos 15 minutos, pero normalmente elimina cualquier problema de almacenamiento en caché en el futuro.

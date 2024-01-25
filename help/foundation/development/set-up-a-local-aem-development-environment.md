---
title: AEM Configuración de un entorno de desarrollo de Local
description: Aprenda a configurar un entorno de desarrollo local para Experience Manager. Familiarícese con la instalación local, Apache Maven, los entornos de desarrollo integrados y la depuración y solución de problemas. Utilice Eclipse IDE, CRXDE-Lite, Visual Studio Code e IntelliJ.
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4677
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2413'
ht-degree: 0%

---

# AEM Configuración de un entorno de desarrollo de Local

Guía para configurar un desarrollo local para Adobe Experience Manager AEM,. Abarca temas importantes como la instalación local, Apache Maven, los entornos de desarrollo integrados y la depuración/solución de problemas. Desarrollo con **Eclipse IDE, CRXDE Lite, Visual Studio Code e IntelliJ** son objeto de debate.

## Información general

La configuración de un entorno de desarrollo local es el primer paso al desarrollar para Adobe Experience Manager AEM o. Tómese el tiempo necesario para configurar un entorno de desarrollo de calidad para aumentar la productividad y escribir código mejor y más rápido. AEM Podemos dividir un entorno de desarrollo local en cuatro áreas:

* AEM Instancias locales de
* [!DNL Apache Maven] proyecto
* Entornos de desarrollo integrados (IDE)
* Solución de problemas

## AEM Instalar instancias locales de la

AEM Cuando nos referimos a una instancia local de, estamos hablando de una copia de Adobe Experience Manager que se ejecuta en el equipo personal de un desarrollador. ***Todo*** AEM AEM El desarrollo de la aplicación debería comenzar escribiendo y ejecutando código en una instancia de local.

AEM Si es nuevo en la instalación de, puede instalar dos modos de ejecución básicos: ***Autor*** y ***Publish***. El ***Autor*** [modo de ejecución](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en)  es el entorno que utilizan los especialistas en marketing digital para crear y administrar contenido. Al desarrollar la mayor parte del tiempo, implementa un código en una instancia de autor. Esto le permite crear páginas y añadir y configurar componentes. AEM Sites es un CMS de creación WYSIWYG y, por lo tanto, la mayoría de CSS y JavaScript se pueden probar con una instancia de creación.

También lo es *crítico* prueba de código con un local ***Publish*** ejemplo. El ***Publish*** AEM La instancia de es el entorno en el que interactúan los visitantes del sitio web. Mientras que el ***Publish*** La instancia de es la misma pila tecnológica que la ***Autor*** Por ejemplo, hay algunas distinciones importantes con configuraciones y permisos. El código debe probarse con un local ***Publish*** antes de promocionarse a entornos de nivel superior.

### Etapas

1. Asegúrese de que Java™ está instalado.
   * Preferir [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) AEM para 6.5+ de la
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) AEM AEM para las versiones de la anteriores a la 6.5
1. Obtenga una copia del [AEM Jar y a de inicio rápido de [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=es).
1. Cree una estructura de carpetas en el equipo como la siguiente:

```plain
~/aem-sdk
    /author
    /publish
```

1. Cambie el nombre del [!DNL QuickStart] JAR a ***aem-author-p4502.jar*** y colóquelo debajo de la `/author` directorio. Añada el ***[!DNL license.properties]*** debajo del archivo `/author` directorio.

1. Haga una copia del [!DNL QuickStart] JAR, cambie el nombre a ***aem-publish-p4503.jar*** y colóquelo debajo de la `/publish` directorio. Añada una copia del ***[!DNL license.properties]*** debajo del archivo `/publish` directorio.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Haga doble clic en ***aem-author-p4502.jar*** para instalar el **Autor** ejemplo. Esto inicia la instancia de autor, que se ejecuta en el puerto **4502** en el equipo local.

Haga doble clic en ***aem-publish-p4503.jar*** para instalar el **Publish** ejemplo. Esto inicia la instancia de publicación, que se ejecuta en el puerto **4503** en el equipo local.

>[!NOTE]
>
>Dependiendo del hardware de su equipo de desarrollo, puede ser difícil tener un **Crear y publicar** instancia de que se ejecuta al mismo tiempo. En raras ocasiones es necesario ejecutar ambos simultáneamente en una configuración local.

### Uso de la línea de comandos

AEM Una alternativa a hacer doble clic en el archivo JAR es iniciar la aplicación desde la línea de comandos o crear una secuencia de comandos (`.bat` o `.sh`) según el tipo de sistema operativo local. A continuación se muestra un ejemplo del comando de ejemplo:

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

Aquí, el `-X` son opciones de JVM y `-D` son propiedades de marco de trabajo adicionales. Para obtener más información, consulte [AEM Implementación y mantenimiento de una instancia de](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=es) y [Más opciones disponibles en el archivo de inicio rápido](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Instalar Apache Maven

***[!DNL Apache Maven]*** es una herramienta para administrar el procedimiento de generación e implementación para proyectos basados en Java. AEM es una plataforma basada en Java y [!DNL Maven] AEM es la forma estándar de administrar el código de un proyecto de. Cuando decimos ***AEM Proyecto de Maven*** o solo su ***AEM Proyecto de***, nos referimos a un proyecto Maven que incluye todas las *personalizado* código del sitio.

AEM Todos los proyectos de deben crearse a partir de la versión más reciente de **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). El [!DNL AEM Project Archetype] AEM proporciona un bootstrap de un proyecto de con algunos ejemplos de código y contenido. El [!DNL AEM Project Archetype] también incluye **[!DNL AEM WCM Core Components]** configurado para utilizarse en el proyecto.

>[!CAUTION]
>
>Al iniciar un nuevo proyecto, se recomienda utilizar la versión más reciente del tipo de archivo. AEM Tenga en cuenta que hay varias versiones del tipo de archivo y que no todas las versiones son compatibles con versiones anteriores de la aplicación de la versión de la aplicación de tipo de archivo.

### Etapas

1. Descargar [Apache Maven](https://maven.apache.org/download.cgi)
2. Instalar [Apache Maven](https://maven.apache.org/install.html?lang=es) y asegúrese de que la instalación se ha agregado a la línea de comandos `PATH`.
   * [!DNL macOS] Los usuarios de pueden instalar Maven mediante [Homebrew](https://brew.sh/)
3. Compruebe que **[!DNL Maven]** se instala abriendo un nuevo terminal de línea de comandos y ejecutando lo siguiente:

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> En, la adición pasada de `adobe-public` El perfil de Maven era necesario para señalar `nexus.adobe.com` AEM para descargar artefactos de la. AEM Todos los artefactos están ahora disponibles a través de Maven Central y el `adobe-public` no se necesita el perfil.

## Configurar un entorno de desarrollo integrado

Un entorno de desarrollo integrado o IDE es una aplicación que combina un editor de texto, compatibilidad con sintaxis y herramientas de generación. Dependiendo del tipo de desarrollo que esté realizando, es posible que un IDE sea preferible sobre otro. Independientemente del IDE, es importante poder realizar actualizaciones periódicamente ***push*** AEM codifique en una instancia local de para probarla. Es importante que a veces ***extraer*** AEM AEM Configuraciones de una instancia de local en el proyecto de para persistir en un sistema de administración de control de código fuente como Git.

AEM AEM A continuación, se muestran algunos de los IDE más populares que se utilizan con el desarrollo de recursos con vídeos correspondientes que muestran la integración con una instancia de local.

>[!NOTE]
>
> AEM El proyecto WKND se ha actualizado a los valores predeterminados para que funcione en el as a Cloud Service de la. Se ha actualizado para que sea [compatible con versiones anteriores de 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). AEM Si se utiliza la versión 6.5 o 6.4, se debe anexar la variable `classic` de perfil a cualquier comando de Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Cuando utilice un IDE, asegúrese de comprobar lo siguiente `classic` en la pestaña Perfil de Maven.

![Pestaña Perfil de Maven](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Perfil de IntelliJ Maven*

### [!DNL Eclipse] IDE

El **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** es uno de los IDE más populares para el desarrollo de Java™, en gran parte porque es de código abierto y ***gratuito***! El Adobe proporciona un complemento, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**, para [!DNL Eclipse] AEM para permitir un desarrollo más sencillo con una buena interfaz gráfica de usuario para sincronizar el código con una instancia local de la. El [!DNL Eclipse] AEM Se recomienda IDE para desarrolladores nuevos en el mundo de la computación, debido en gran parte a la compatibilidad con la interfaz gráfica de usuario que ofrece el usuario. [!DNL AEM Developer Tools].

#### Instalación y configuración

1. Descargue e instale [!DNL Eclipse] IDE para [!DNL Java™ EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga las instrucciones para instalar el [!DNL AEM Developer Tools] complemento: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30: Importar el proyecto Maven
* 01:24: Crear e implementar código fuente con Maven
* AEM 04:33: Cambios en el código push con la herramienta para desarrolladores de
* AEM 10:55: cambios en el código de extracción con la herramienta para desarrolladores de
* 13:12: Uso de las herramientas de depuración integradas de Eclipse

### IntelliJ IDEA

El **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** es un potente IDE para el desarrollo profesional de Java™. [!DNL IntelliJ IDEA] viene en dos sabores, un ***gratuito*** [!DNL Community] edición y anuncio (de pago) [!DNL Ultimate] versión. La libertad [!DNL Community] versión de [!DNL IntellIJ IDEA] AEM es suficiente para un desarrollo más, sin embargo, la [!DNL Ultimate] [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Descargue e instale [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instalar [!DNL Repo] (herramienta de línea de comandos): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00: Importar el proyecto Maven
* 05:47 - Crear e implementar código fuente con Maven
* 08:17 - Empuje los cambios con el repositorio
* 14:39 - Extraer cambios con el repositorio
* 17:25 - Uso de las herramientas de depuración integradas de IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Código de Visual Studio](https://code.visualstudio.com/)** se ha convertido rápidamente en una herramienta favorita para ***desarrolladores front-end*** con compatibilidad mejorada con JavaScript, [!DNL Intellisense]y compatibilidad con la depuración del explorador. **[!DNL Visual Studio Code]** es de código abierto, gratuito, con muchas extensiones potentes. [!DNL Visual Studio Code] AEM se puede configurar para integrarse con la ayuda de una herramienta de Adobe de, con la que se puede crear una interfaz de usuario de. **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** AEM También hay varias extensiones compatibles con la comunidad que se pueden instalar para integrarse con el servicio de integración de.

[!DNL Visual Studio Code] AEM es una excelente opción para desarrolladores de front-end que principalmente escriben código CSS/LESS y JavaScript para crear bibliotecas de cliente de la aplicación de forma. AEM Esta herramienta puede no ser la mejor opción para los nuevos desarrolladores de, ya que las definiciones de nodo (cuadros de diálogo, componentes) deben editarse en XML sin procesar. Hay varias extensiones Java™ disponibles para [!DNL Visual Studio Code], sin embargo, si se realiza principalmente el desarrollo de Java™ [!DNL Eclipse IDE] o [!DNL IntelliJ] puede ser preferible.

#### Vínculos importantes

* [**Descargar**](https://code.visualstudio.com/Download) **Código de Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - Herramienta similar a FTP para contenido JCR
* **[aemfed](https://aemfed.io/)** AEM - Acelere el flujo de trabajo front-end de la
* **[AEM Sincronización de](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Compatible con la comunidad&#42; Extensión para código de Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30: Importar el proyecto Maven
* 00:53: Crear e implementar código fuente con Maven
* 04:03 - Cambios en el código push con la herramienta de línea de comandos Repo
* 08:29 - Cambios en el código de extracción con la herramienta de línea de comandos Repo
* 10:40: cambios en el código push con la herramienta aemfed
* 14:24: Solución de problemas, reconstrucción de bibliotecas de cliente

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) AEM es una vista basada en explorador del repositorio de. [!DNL CRXDE Lite] AEM está incrustado en el entorno de trabajo y permite a un desarrollador realizar tareas de desarrollo estándar como editar archivos, definir componentes, cuadros de diálogo y plantillas. [!DNL CRXDE Lite] es ***no*** pretende ser un entorno de desarrollo completo, pero es eficaz como herramienta de depuración. [!DNL CRXDE Lite] es útil cuando se amplía o simplemente se comprende el código de producto fuera de la base de código. [!DNL CRXDE Lite] proporciona una vista completa del repositorio y una forma de probar y administrar permisos de forma eficaz.

[!DNL CRXDE Lite] debe utilizarse con otros IDE para probar y depurar código, pero nunca como herramienta de desarrollo principal. Tiene compatibilidad con sintaxis limitada, no tiene capacidades de autocompletar e integración limitada con sistemas de administración de control de código fuente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Solución de problemas

***¡Ayuda!*** ¡Mi código no funciona! Al igual que con todo el desarrollo, hay momentos (probablemente muchos) en que el código no funciona como se espera. AEM Es una plataforma potente, pero con gran poder... viene una gran complejidad. A continuación se presentan algunos puntos de partida de alto nivel para la resolución de problemas y el seguimiento de problemas (pero lejos de una lista exhaustiva de cosas que pueden salir mal):

### Verificar implementación de código

AEM Un buen primer paso, cuando se produce un problema, es verificar que el código se haya implementado e instalado correctamente en el servidor de correo de OSGiOSs de AEMs de AEMs.

1. **Marque [!UICONTROL Administrador de paquetes]** para asegurarse de que el paquete de código se ha cargado e instalado: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Compruebe la marca de tiempo para comprobar que el paquete se ha instalado recientemente.
1. Si realiza actualizaciones de archivos incrementales con una herramienta como [!DNL Repo] o [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** AEM Compruebe que el archivo se ha insertado en la instancia de la instancia local de la y que se ha actualizado el contenido del archivo: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Compruebe que el paquete esté cargado** si ve problemas relacionados con el código Java™ en un paquete OSGi. Abra el [!UICONTROL Consola web de Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) y busque su paquete. Asegúrese de que el paquete tenga un **[!UICONTROL Activo]** estado. Consulte a continuación para obtener más información relacionada con la resolución de problemas de un paquete en una **[!UICONTROL Instalado]** estado.

#### Comprobación de los registros

AEM es una plataforma de chat y registra información útil en el **error.log**. El **error.log** AEM se puede encontrar donde se ha instalado la: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Una técnica útil para rastrear problemas es agregar instrucciones de registro en el código Java™:

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

De forma predeterminada, la variable **error.log** está configurado para registrar *[!DNL INFO]* extractos. Si desea cambiar el nivel de registro, puede hacerlo accediendo a [!UICONTROL Compatibilidad con registros]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). También puede encontrar que la variable **error.log** es demasiado hablador. Puede usar el complemento [!UICONTROL Compatibilidad con registros] para configurar sentencias de registro solo para un paquete Java™ especificado. AEM Se trata de una práctica recomendada para los proyectos con el fin de separar fácilmente los problemas de código personalizado de los problemas de la plataforma de la aplicación de código personalizado de los problemas de la plataforma de la de OOTB.

![AEM Registrando configuración en el](./assets/set-up-a-local-aem-development-environment/logging.png)

#### El paquete está en estado Instalado {#bundle-active}

Todos los paquetes (excepto los fragmentos) deben estar en un **[!UICONTROL Activo]** estado. Si ve su paquete de código en un [!UICONTROL Instalado] estado, entonces hay un problema que debe resolverse. La mayoría de las veces se trata de un problema de dependencia:

![AEM Error de paquete en la](assets/set-up-a-local-aem-development-environment/bundle-error.png)

En la captura de pantalla anterior, la variable [!DNL WKND Core bundle] es un [!UICONTROL Instalado] estado. Esto se debe a que el paquete espera una versión diferente de `com.adobe.cq.wcm.core.components.models` AEM de lo que está disponible en la instancia de.

Una herramienta útil que se puede utilizar es la [!UICONTROL Buscador de dependencias]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). AEM Añada el nombre del paquete Java™ para inspeccionar qué versión está disponible en la instancia de:

![Componentes principales](assets/set-up-a-local-aem-development-environment/core-components.png)

AEM Siguiendo con el ejemplo anterior, podemos ver que la versión instalada en la instancia de la instancia de la instancia de la aplicación es la siguiente: **12,2** vs **12,6** que el paquete estaba esperando. A partir de ahí, puede trabajar hacia atrás y ver si la variable [!DNL Maven] AEM dependencias en la coincidencia de la [!DNL Maven] AEM dependencias en el proyecto de la. En, en el ejemplo anterior [!DNL Core Components] **Versión 2.2.0** AEM se instala en la instancia de la instancia de la, pero el paquete de código se creó con una dependencia de **Versión 2.2.2**, de ahí el motivo del problema de dependencia.

#### Verificar el registro de modelos Sling {#osgi-component-sling-models}

AEM Los componentes de la deben estar respaldados por un [!DNL Sling Model] para encapsular cualquier lógica empresarial y asegurarse de que el script de procesamiento HTL permanece limpio. Si tiene problemas en los que no se encuentra el modelo Sling, puede resultar útil comprobar la [!DNL Sling Models] desde la consola: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Esto le indica si el modelo Sling se ha registrado y a qué tipo de recurso (la ruta del componente) está vinculado.

![Estado del modelo Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Muestra el registro de un [!DNL Sling Model], `BylineImpl` que está vinculado a un tipo de recurso de componente de `wknd/components/content/byline`.

#### Problemas de CSS o JavaScript

Para la mayoría de los problemas de CSS y JavaScript, el uso de las herramientas de desarrollo del explorador es la forma más eficaz de solucionar. AEM Para reducir el problema al desarrollar con una instancia de autor de, resulta útil ver la página &quot;tal y como aparece publicado&quot;.

![Problemas de CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Abra el [!UICONTROL Propiedades de página] y haga clic en [!UICONTROL Ver como aparece publicado]. AEM Esto abre la página sin el Editor de y con un parámetro de consulta establecido en **wcmmode=disabled**. AEM Esto deshabilita de forma efectiva la IU de creación de la y facilita la solución de problemas y la depuración de problemas del front-end.

Otro problema que se suele encontrar al desarrollar código front-end es que se está cargando CSS/JS antiguo o obsoleto. Como primer paso, asegúrese de que se ha borrado el historial del explorador y, si es necesario, inicie una sesión nueva o unos exploradores de incógnito.

#### Depuración de bibliotecas de cliente

Con los diferentes métodos de categorías e incrustaciones para incluir varias bibliotecas de cliente, puede resultar engorroso solucionar problemas. AEM La expone varias herramientas para ayudarle con esto. Una de las herramientas más importantes es [!UICONTROL Reconstruir bibliotecas de cliente] AEM que fuerza a los usuarios a volver a compilar cualquier archivo LESS y generar el CSS.

* [Volcar bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.html) AEM - Enumera todas las bibliotecas de cliente registradas en la instancia de. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Resultado de prueba](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permite al usuario ver la salida de HTML esperada de clientlib includes en función de la categoría. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Validación de dependencias de bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : resalta las dependencias o categorías incrustadas que no se pueden encontrar. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Reconstruir bibliotecas de cliente](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) AEM : permite a un usuario forzar a los usuarios a que reconstruyan todas las bibliotecas de cliente o a que invaliden la caché de las bibliotecas de cliente. AEM Esta herramienta es eficaz cuando se desarrolla con LESS, ya que esto puede obligar a los usuarios a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y luego realizar una actualización de página que volver a crear todas las bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Depuración De Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Si tiene que invalidar constantemente la caché utilizando [!UICONTROL Reconstruir bibliotecas de cliente] , tal vez valga la pena reconstruir una vez todas las bibliotecas de clientes. Esto puede tardar alrededor de 15 minutos, pero generalmente elimina cualquier problema de almacenamiento en caché en el futuro.

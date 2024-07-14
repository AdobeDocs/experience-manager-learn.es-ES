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
duration: 4537
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2413'
ht-degree: 0%

---

# AEM Configuración de un entorno de desarrollo de Local

Guía para configurar un desarrollo local para Adobe Experience Manager AEM,. Abarca temas importantes como la instalación local, Apache Maven, los entornos de desarrollo integrados y la depuración/solución de problemas. Se describe el desarrollo con **Eclipse IDE, CRXDE Lite, Visual Studio Code e IntelliJ**.

## Información general

La configuración de un entorno de desarrollo local es el primer paso al desarrollar para Adobe Experience Manager AEM o. Tómese el tiempo necesario para configurar un entorno de desarrollo de calidad para aumentar la productividad y escribir código mejor y más rápido. AEM Podemos dividir un entorno de desarrollo local en cuatro áreas:

* AEM Instancias locales de
* [!DNL Apache Maven] proyecto
* Entornos de desarrollo integrados (IDE)
* Resolución de problemas

## AEM Instalar instancias locales de la

AEM Cuando nos referimos a una instancia local de, estamos hablando de una copia de Adobe Experience Manager que se ejecuta en el equipo personal de un desarrollador. AEM AEM ***Todo*** el desarrollo de la debe comenzar escribiendo y ejecutando código en una instancia de la instancia de la instancia local de la aplicación de la aplicación de código de la aplicación de código de la aplicación de código de la aplicación de código de.

AEM Si eres nuevo en el modo de ejecución, hay dos modos de ejecución básicos que se pueden instalar: ***Author*** y ***Publish***. El ***autor*** [modo de ejecución](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en) es el entorno que los especialistas en marketing digital utilizan para crear y administrar contenido. Al desarrollar la mayor parte del tiempo, implementa un código en una instancia de autor. Esto le permite crear páginas y añadir y configurar componentes. AEM Sites es un CMS de creación WYSIWYG y, por lo tanto, la mayoría de CSS y JavaScript se pueden probar con una instancia de creación.

También es *crítico* código de prueba contra una instancia local de ***Publish***. La instancia de ***Publish AEM*** es el entorno de con el que interactúan los visitantes del sitio web. Aunque la instancia de ***Publish*** es la misma pila de tecnología que la instancia de ***Autor***, hay algunas distinciones importantes con configuraciones y permisos. El código debe probarse en una instancia local de ***Publish*** antes de promocionarse a entornos de nivel superior.

### Etapas

1. Asegúrese de que Java™ está instalado.
   * AEM Preferir [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) para la versión 6.5 o posterior de la versión de la aplicación de la aplicación de la aplicación de datos de la versión 6.5
   * AEM AEM [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) para las versiones de la anteriores a la 6.5
1. AEM Obtener una copia del [Jar de inicio rápido de la y un [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=es).
1. Cree una estructura de carpetas en el equipo como la siguiente:

```plain
~/aem-sdk
    /author
    /publish
```

1. Cambie el nombre del JAR [!DNL QuickStart] a ***aem-author-p4502.jar*** y colóquelo debajo del directorio `/author`. Agregue el archivo ***[!DNL license.properties]*** debajo del directorio `/author`.

1. Haga una copia del JAR [!DNL QuickStart], renómbrelo a ***aem-publish-p4503.jar*** y colóquelo debajo del directorio `/publish`. Agregue una copia del archivo ***[!DNL license.properties]*** debajo del directorio `/publish`.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Haga doble clic en el archivo ***aem-author-p4502.jar*** para instalar la instancia de **Author**. Esto inicia la instancia de autor, que se ejecuta en el puerto **4502** del equipo local.

Haga doble clic en el archivo ***aem-publish-p4503.jar*** para instalar la instancia de **Publish**. Esto inicia la instancia de Publish, que se ejecuta en el puerto **4503** del equipo local.

>[!NOTE]
>
>Según el hardware de su equipo de desarrollo, puede resultar difícil tener una instancia de **Author y otra de Publish** ejecutándose al mismo tiempo. En raras ocasiones es necesario ejecutar ambos simultáneamente en una configuración local.

### Uso de la línea de comandos

AEM Una alternativa a hacer doble clic en el archivo JAR es iniciar la aplicación desde la línea de comandos o crear un script (`.bat` o `.sh`) según el sabor del sistema operativo local. A continuación se muestra un ejemplo del comando de ejemplo:

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

AEM Aquí, `-X` son opciones de JVM y `-D` son propiedades de marco de trabajo adicionales. Para obtener más información, consulte [Implementación y mantenimiento de una instancia de](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=es) y [Más opciones disponibles en el archivo de inicio rápido](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Instalar Apache Maven

***[!DNL Apache Maven]*** es una herramienta para administrar el procedimiento de generación e implementación de proyectos basados en Java. AEM AEM es una plataforma basada en Java y [!DNL Maven] es la forma estándar de administrar el código de un proyecto de. AEM AEM Cuando decimos ***Proyecto Maven*** o solo tu ***Proyecto Maven***, nos referimos a un proyecto Maven que incluye todo el código *personalizado* para tu sitio.

AEM Todos los proyectos deben compilarse a partir de la versión más reciente de **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). AEM El [!DNL AEM Project Archetype] proporciona un bootstrap de un proyecto de con código y contenido de ejemplo. El [!DNL AEM Project Archetype] también incluye **[!DNL AEM WCM Core Components]** configurado para usarse en el proyecto.

>[!CAUTION]
>
>Al iniciar un nuevo proyecto, se recomienda utilizar la versión más reciente del tipo de archivo. AEM Tenga en cuenta que hay varias versiones del tipo de archivo y que no todas las versiones son compatibles con versiones anteriores de la aplicación de la versión de la aplicación de tipo de archivo.

### Etapas

1. Descargar [Apache Maven](https://maven.apache.org/download.cgi)
2. Instale [Apache Maven](https://maven.apache.org/install.html?lang=es) y asegúrese de que la instalación se haya agregado a su línea de comandos `PATH`.
   * [!DNL macOS] usuarios pueden instalar Maven usando [Homebrew](https://brew.sh/)
3. Compruebe que **[!DNL Maven]** está instalado abriendo un nuevo terminal de línea de comandos y ejecutando lo siguiente:

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
> AEM En, la adición anterior de `adobe-public` perfil de Maven era necesaria para apuntar `nexus.adobe.com` para descargar artefactos de la. AEM Ahora todos los artefactos de la están disponibles a través de Maven Central y el perfil `adobe-public` no es necesario.

## Configurar un entorno de desarrollo integrado

Un entorno de desarrollo integrado o IDE es una aplicación que combina un editor de texto, compatibilidad con sintaxis y herramientas de generación. Dependiendo del tipo de desarrollo que esté realizando, es posible que un IDE sea preferible sobre otro. AEM Independientemente del IDE, es importante poder ***insertar*** código periódicamente en una instancia local de para probarlo. AEM AEM Es importante ***extraer*** configuraciones ocasionalmente de una instancia de local a su proyecto de para poder conservarlas en un sistema de administración de control de código fuente como Git.

AEM AEM A continuación, se muestran algunos de los IDE más populares que se utilizan con el desarrollo de recursos con vídeos correspondientes que muestran la integración con una instancia de local.

>[!NOTE]
>
> El proyecto WKND se ha actualizado para que funcione de forma predeterminada en AEM as a Cloud Service. Se ha actualizado para que sea [compatible con versiones anteriores de 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). AEM Si utiliza la versión 6.5 o 6.4 de la aplicación, anexe el perfil `classic` a cualquier comando de Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Cuando utilice un IDE, asegúrese de marcar `classic` en la pestaña de su perfil de Maven.

![Ficha de perfil de Maven](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Perfil Maven de IntelliJ*

### [!DNL Eclipse] IDE

El **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** es uno de los IDE más populares para el desarrollo de Java™, en gran parte porque es de código abierto y ***gratuito***. El Adobe AEM proporciona un complemento, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**, para [!DNL Eclipse] con el fin de facilitar el desarrollo con una interfaz gráfica de usuario adecuada para sincronizar el código con una instancia local de la instancia de la. AEM El IDE [!DNL Eclipse] se recomienda para desarrolladores nuevos en la práctica de la interfaz de usuario debido en gran parte a la compatibilidad con la interfaz de usuario de [!DNL AEM Developer Tools].

#### Instalación y configuración

1. Descargue e instale el IDE [!DNL Eclipse] para [!DNL Java™ EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga las instrucciones para instalar el complemento [!DNL AEM Developer Tools]: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30: Importar el proyecto Maven
* 01:24: Crear e implementar código fuente con Maven
* AEM 04:33: Cambios en el código push con la herramienta para desarrolladores de
* AEM 10:55: cambios en el código de extracción con la herramienta para desarrolladores de
* 13:12: Uso de las herramientas de depuración integradas de Eclipse

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)** es un IDE potente para el desarrollo profesional de Java™. [!DNL IntelliJ IDEA] viene en dos versiones, una ***edición [!DNL Community]*** gratis y una versión [!DNL Ultimate] comercial (de pago). AEM La versión gratuita [!DNL Community] de [!DNL IntellIJ IDEA] es suficiente para un desarrollo más amplio, pero [!DNL Ultimate] [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Descargar e instalar [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instalar [!DNL Repo] (herramienta de línea de comandos): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00: Importar el proyecto Maven
* 05:47 - Crear e implementar código fuente con Maven
* 08:17 - Empuje los cambios con el repositorio
* 14:39 - Extraer cambios con el repositorio
* 17:25 - Uso de las herramientas de depuración integradas de IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** se ha convertido rápidamente en una herramienta favorita para ***desarrolladores de front-end*** con compatibilidad mejorada con JavaScript, [!DNL Intellisense] y compatibilidad con depuración de exploradores. **[!DNL Visual Studio Code]** es de código abierto, gratuito, con muchas extensiones potentes. AEM [!DNL Visual Studio Code] se puede configurar para que se integre con los recursos de la herramienta de Adobe de trabajo, **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).AEM** También hay varias extensiones admitidas por la comunidad que se pueden instalar para integrarse con el servicio de integración de.

[!DNL Visual Studio Code] es una buena opción para los desarrolladores de front-end que principalmente escriben código CSS/LESS y JavaScript AEM para crear bibliotecas de cliente de. AEM Esta herramienta puede no ser la mejor opción para los nuevos desarrolladores de, ya que las definiciones de nodo (cuadros de diálogo, componentes) deben editarse en XML sin procesar. Hay varias extensiones de Java™ disponibles para [!DNL Visual Studio Code]; sin embargo, si se usan principalmente las extensiones Java™, se puede preferir el desarrollo [!DNL Eclipse IDE] o [!DNL IntelliJ].

#### Vínculos importantes

* [**Descargar**](https://code.visualstudio.com/Download) **código de Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)**: herramienta similar a FTP para contenido JCR
* AEM **[aemfed](https://aemfed.io/)**: Acelere el flujo de trabajo del front-end de la interfaz de usuario de la red de distribución de correo electrónico
* AEM **[Sincronizar ](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Comunidad admitida&#42; extensión para código de Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30: Importar el proyecto Maven
* 00:53: Crear e implementar código fuente con Maven
* 04:03 - Cambios en el código push con la herramienta de línea de comandos Repo
* 08:29 - Cambios en el código de extracción con la herramienta de línea de comandos Repo
* 10:40: cambios en el código push con la herramienta aemfed
* 14:24: Solución de problemas, reconstrucción de bibliotecas de cliente

### [!DNL CRXDE Lite]

[CRXDE Lite AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) es una vista basada en explorador del repositorio de la. AEM [!DNL CRXDE Lite] está incrustado en el y permite a un desarrollador realizar tareas de desarrollo estándar como editar archivos, definir componentes, cuadros de diálogo y plantillas. [!DNL CRXDE Lite] ***no*** pretende ser un entorno de desarrollo completo, pero es una herramienta de depuración eficaz. [!DNL CRXDE Lite] resulta útil para ampliar o simplemente comprender el código de producto fuera de la base de código. [!DNL CRXDE Lite] proporciona una vista completa del repositorio y una forma de probar y administrar permisos de forma eficaz.

[!DNL CRXDE Lite] debe usarse con otros IDE para probar y depurar código, pero nunca como herramienta de desarrollo principal. Tiene compatibilidad con sintaxis limitada, no tiene capacidades de autocompletar e integración limitada con sistemas de administración de control de código fuente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Resolución de problemas

***¡Ayuda!*** ¡Mi código no funciona! Al igual que con todo el desarrollo, hay momentos (probablemente muchos) en que el código no funciona como se espera. AEM Es una plataforma potente, pero con gran poder... viene una gran complejidad. A continuación se presentan algunos puntos de partida de alto nivel para la resolución de problemas y el seguimiento de problemas (pero lejos de una lista exhaustiva de cosas que pueden salir mal):

### Verificar implementación de código

AEM Un buen primer paso, cuando se produce un problema, es verificar que el código se haya implementado e instalado correctamente en el servidor de correo de OSGiOSs de AEMs de AEMs.

1. **Compruebe el [!UICONTROL Administrador de paquetes]** para asegurarse de que el paquete de código se haya cargado e instalado: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Compruebe la marca de tiempo para comprobar que el paquete se ha instalado recientemente.
1. AEM Si se realizan actualizaciones incrementales de archivos con una herramienta como [!DNL Repo] o [!DNL AEM Developer Tools], **compruebe[!DNL CRXDE Lite]** que el archivo se ha insertado en la instancia de archivo local y que el contenido del archivo se ha actualizado: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Compruebe que el paquete se ha cargado** si ve problemas relacionados con el código Java™ en un paquete OSGi. Abra la [!UICONTROL consola web de Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) y busque su paquete. Asegúrese de que el paquete tenga un estado **[!UICONTROL Activo]**. Consulte a continuación para obtener más información relacionada con la solución de problemas de un paquete en estado **[!UICONTROL Instalado]**.

#### Comprobación de los registros

AEM es una plataforma de conversación y registra información útil en **error.log**. AEM El archivo **error.log** se encuentra donde se ha instalado el archivo de registro de errores: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

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

De manera predeterminada, **error.log** está configurado para registrar *[!DNL INFO]* instrucciones. Si desea cambiar el nivel de registro, vaya a [!UICONTROL Compatibilidad con registros]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). También es posible que descubra que **error.log** es demasiado parlanchín. Puede usar la [!UICONTROL compatibilidad con registros] para configurar las instrucciones de registro para un paquete Java™ especificado. AEM Se trata de una práctica recomendada para los proyectos con el fin de separar fácilmente los problemas de código personalizado de los problemas de la plataforma de la aplicación de código personalizado de los problemas de la plataforma de la de OOTB.

AEM ![Registrando la configuración en el archivo de registro {10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000](./assets/set-up-a-local-aem-development-environment/logging.png)

#### El paquete está en estado Instalado {#bundle-active}

Todos los paquetes (excepto los fragmentos) deben estar en estado **[!UICONTROL Activo]**. Si ve su paquete de código en un estado [!UICONTROL Instalado], hay un problema que debe resolverse. La mayoría de las veces se trata de un problema de dependencia:

AEM ![Error de paquete en el paquete de trabajo {100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000](assets/set-up-a-local-aem-development-environment/bundle-error.png)

En la captura de pantalla anterior, [!DNL WKND Core bundle] tiene el estado [!UICONTROL Instalado]. AEM Esto se debe a que el paquete espera una versión de `com.adobe.cq.wcm.core.components.models` diferente a la disponible en la instancia de la.

Una herramienta útil que se puede usar es [!UICONTROL Buscador de dependencias]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). AEM Añada el nombre del paquete Java™ para inspeccionar qué versión está disponible en la instancia de:

![Componentes principales](assets/set-up-a-local-aem-development-environment/core-components.png)

AEM Siguiendo con el ejemplo anterior, podemos ver que la versión instalada en la instancia de la instancia de la es **12.2** frente a **12.6** que el paquete esperaba. AEM AEM A partir de ahí, puede trabajar hacia atrás y ver si las dependencias [!DNL Maven] en coinciden con las dependencias [!DNL Maven] en el proyecto de. AEM En, el ejemplo anterior [!DNL Core Components] **v2.2.0** está instalado en la instancia de la instancia de la, pero el paquete de código se creó con una dependencia en **v2.2.2**, de ahí el motivo del problema de dependencia.

#### Verificar el registro de modelos Sling {#osgi-component-sling-models}

AEM Los componentes de la deben estar respaldados por un [!DNL Sling Model] para encapsular cualquier lógica empresarial y garantizar que el script de procesamiento HTL permanezca limpio. Si tiene problemas en los que no se encuentra el modelo Sling, puede resultar útil comprobar [!DNL Sling Models] desde la consola: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Esto le indica si el modelo Sling se ha registrado y a qué tipo de recurso (la ruta del componente) está vinculado.

![Estado del modelo Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Muestra el registro de un(a) [!DNL Sling Model], `BylineImpl` que está enlazado(a) a un tipo de recurso de componente de `wknd/components/content/byline`.

#### Problemas de CSS o JavaScript

Para la mayoría de los problemas de CSS y JavaScript, el uso de las herramientas de desarrollo del explorador es la forma más eficaz de solucionar. AEM Para reducir el problema al desarrollar con una instancia de autor de, resulta útil ver la página &quot;tal y como aparece publicado&quot;.

![Problemas de CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Abra el menú [!UICONTROL Propiedades de la página] y haga clic en [!UICONTROL Ver tal y como se publicó]. AEM Esto abre la página sin el Editor de consultas y con un parámetro de consulta establecido en **wcmmode=disabled**. AEM Esto deshabilita de forma efectiva la IU de creación de la y facilita la solución de problemas y la depuración de problemas del front-end.

Otro problema que se suele encontrar al desarrollar código front-end es que se está cargando CSS/JS antiguo o obsoleto. Como primer paso, asegúrese de que se ha borrado el historial del explorador y, si es necesario, inicie una sesión nueva o unos exploradores de incógnito.

#### Depuración de bibliotecas de cliente

Con los diferentes métodos de categorías e incrustaciones para incluir varias bibliotecas de cliente, puede resultar engorroso solucionar problemas. AEM La expone varias herramientas para ayudarle con esto. AEM Una de las herramientas más importantes es [!UICONTROL Reconstruir bibliotecas de cliente], lo que obliga a los usuarios a volver a compilar archivos LESS y generar el archivo CSS.

* AEM [Bibliotecas de volcado](http://localhost:4502/libs/granite/ui/content/dumplibs.html): Enumera todas las bibliotecas de cliente registradas en la instancia de. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Salida de prueba](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html): permite al usuario ver la salida de HTML esperada de clientlib includes según la categoría. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Validación de dependencias de bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html): resalta las dependencias o categorías incrustadas que no se pueden encontrar. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* AEM [Reconstruir bibliotecas de cliente](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html): permite al usuario forzar a los usuarios a que reconstruyan todas las bibliotecas de cliente o a que invaliden la caché de las bibliotecas de cliente. AEM Esta herramienta es eficaz cuando se desarrolla con LESS, ya que esto puede obligar a los usuarios a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y luego realizar una actualización de página que volver a crear todas las bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Depurando Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Si constantemente tiene que invalidar la caché usando la herramienta [!UICONTROL Reconstruir bibliotecas de cliente], tal vez valga la pena reconstruir una sola vez todas las bibliotecas de cliente. Esto puede tardar alrededor de 15 minutos, pero generalmente elimina cualquier problema de almacenamiento en caché en el futuro.

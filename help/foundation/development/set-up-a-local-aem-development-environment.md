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
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2582'
ht-degree: 2%

---

# Configuración de un Entorno de desarrollo de AEM local

Guía para configurar un desarrollo local para Adobe Experience Manager, AEM. Abarca temas importantes de instalación local, Apache Maven, entornos de desarrollo integrados y depuración/solución de problemas. Desarrollo con **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] y[!DNL IntelliJ]** se discuten.

## Información general

La configuración de un entorno de desarrollo local es el primer paso al desarrollar para Adobe Experience Manager o AEM. Tómese el tiempo necesario para configurar un entorno de desarrollo de calidad con el fin de aumentar su productividad y escribir mejor código más rápido. Podemos dividir un entorno de desarrollo local AEM en 4 áreas:

* Instancias de AEM locales
* [!DNL Apache Maven] proyecto
* Entornos de desarrollo integrados (IDE)
* Solución de problemas

## Instalar instancias AEM locales

Cuando nos referimos a una instancia de AEM local, hablamos de una copia de Adobe Experience Manager que se está ejecutando en el equipo personal de un desarrollador. ***Todo*** AEM desarrollo debe comenzar escribiendo y ejecutando código en una instancia de AEM local.

Si es nuevo en AEM, se pueden instalar dos modos de ejecución básicos: ***Autor*** y ***Publicación***. La variable ***Autor*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  es el entorno que los especialistas en marketing digital utilizarán para crear y administrar contenido. Al desarrollar **Most** de tiempo, va a implementar código en una instancia de Autor. Esto le permite crear nuevas páginas, así como añadir y configurar componentes. AEM Sites es un CMS WYSIWYG de creación y, por lo tanto, la mayoría de CSS y JavaScript se pueden probar con una instancia de creación.

También es *crítico* probar código con un ***Publicación*** instancia. La variable ***Publicación*** es el entorno AEM con el que interactuarán los visitantes del sitio web. Mientras que la variable ***Publicación*** instancia es la misma pila de tecnología que la ***Autor*** Por ejemplo, hay algunas distinciones importantes con configuraciones y permisos. El código debería *always* se pruebe con un ***Publicación*** antes de promocionarse a entornos de nivel superior.

### Etapas

1. Asegúrese [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) está instalado.
   * Preferir [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=14) para AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) para versiones de AEM anteriores a AEM 6.5
2. Obtenga una copia de [AEM Jar de inicio rápido y un [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Cree una estructura de carpetas en el equipo como la siguiente:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Cambiar el nombre de [!DNL QuickStart] JAR a ***aem-author-p4502.jar*** y colóquelo debajo del `/author` directorio. Agregue la variable ***[!DNL license.properties]*** debajo del `/author` directorio.
5. Haga una copia de [!DNL QuickStart] JAR, cambiar el nombre a ***aem-publish-p4503.jar*** y colóquelo debajo del `/publish` directorio. Agregue una copia de la ***[!DNL license.properties]*** debajo del `/publish` directorio.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Haga doble clic en el botón ***aem-author-p4502.jar*** para instalar el **Autor** instancia. Esto iniciará la instancia de autor, ejecutándose en el puerto **4502** en el equipo local.

   Haga doble clic en el botón ***aem-publish-p4503.jar*** para instalar el **Publicación** instancia. Esto iniciará la instancia de publicación, ejecutándose en el puerto **4503** en el equipo local.

   >[!NOTE]
   >
   >Dependiendo del hardware de su máquina de desarrollo, puede ser difícil tener un **Creación y publicación** se está ejecutando al mismo tiempo. Raramente necesita ejecutar ambos simultáneamente en una configuración local.

   Para obtener más información, consulte [Implementación y mantenimiento de una instancia de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Instalar Apache Maven

***[!DNL Apache Maven]*** es una herramienta para administrar el procedimiento de compilación e implementación para proyectos basados en Java. AEM es una plataforma basada en Java y [!DNL Maven] es la forma estándar de administrar el código de un proyecto AEM. Cuando decimos ***AEM proyecto Maven*** o solo su ***AEM proyecto***, nos referimos a un proyecto de Maven que incluye todas las *custom* código para el sitio.

Todos los AEM proyectos deben basarse en la última versión de la **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). La variable [!DNL AEM Project Archetype] creará un bootstrap de un proyecto AEM con código de muestra y contenido. La variable [!DNL AEM Project Archetype] también incluye **[!DNL AEM WCM Core Components]** configurado para su uso en el proyecto.

>[!CAUTION]
>
>Al iniciar un nuevo proyecto, se recomienda utilizar la versión más reciente del tipo de archivo. Tenga en cuenta que hay varias versiones del tipo de archivo y que no todas las versiones son compatibles con versiones anteriores de AEM.

### Etapas

1. Descargar [Maven Apache](https://maven.apache.org/download.cgi)
2. Instalar [Maven Apache](https://maven.apache.org/install.html) y asegúrese de que la instalación se haya añadido a la línea de comandos `PATH`.
   * [!DNL macOS] los usuarios pueden instalar Maven mediante [Homebrew](https://brew.sh/)
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
   > En el pasado, la adición de `adobe-public` Se necesitaba un perfil Maven para señalar `nexus.adobe.com` para descargar AEM artefactos. Todos los artefactos AEM están ahora disponibles a través de Maven Central y la `adobe-public` no es necesario.

## Configurar un entorno de desarrollo integrado

Un entorno de desarrollo integrado o IDE es una aplicación que combina un editor de texto, compatibilidad con sintaxis y herramientas de compilación. Dependiendo del tipo de desarrollo que esté realizando, un IDE podría ser preferible sobre otro. Independientemente del IDE, será importante poder ser capaz de ***push*** a una instancia de AEM local para probarla. También será importante que ***extraer*** configuraciones desde una instancia de AEM local en el proyecto de AEM para poder persistir en un sistema de administración de control de código fuente como Git.

A continuación, se muestran algunos de los IDE más populares que se utilizan con AEM desarrollo con los vídeos correspondientes que muestran la integración con una instancia de AEM local.

>[!NOTE]
>
> El proyecto WKND se ha actualizado de forma predeterminada para que funcione en AEM as a Cloud Service. Se ha actualizado para que se [compatible con 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Si utiliza AEM 6.5 o 6.4, añada la variable `classic` perfil a cualquier comando Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Cuando utilice un IDE, asegúrese de comprobar `classic` en la ficha Maven Profile .

![Pestaña Maven Profile](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Perfil de maven IntelliJ*

### [!DNL Eclipse] IDE

La variable **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** es uno de los IDE más populares para el desarrollo de Java, en gran parte porque es de código abierto y ***gratuito***! Adobe proporciona un complemento, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**, para [!DNL Eclipse] para permitir un desarrollo más sencillo con una buena GUI para sincronizar el código con una instancia de AEM local. La variable [!DNL Eclipse] Se recomienda IDE para desarrolladores nuevos para AEM en gran parte debido a la compatibilidad con GUI de [!DNL AEM Developer Tools].

#### Instalación y configuración

1. Descargue e instale el [!DNL Eclipse] IDE para [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga las instrucciones para instalar el [!DNL AEM Developer Tools] complemento: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importar proyecto Maven
* 01:24 - Generar e implementar código fuente con Maven
* 04:33 - Cambios en el código push con AEM herramienta para desarrolladores
* 10:55 - Extraer cambios de código con AEM herramienta para desarrolladores
* 13:12 - Uso de las herramientas de depuración integradas de Eclipse

### IntelliJ IDEA

La variable **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** es un potente IDE para el desarrollo profesional de Java. [!DNL IntelliJ IDEA] viene con dos sabores, un ***gratuito*** [!DNL Community] edición y comercial (de pago) [!DNL Ultimate] versión. El gratuito [!DNL Community] versión de [!DNL IntellIJ IDEA] es suficiente para un desarrollo más AEM, pero la variable [!DNL Ultimate] [amplía su conjunto de capacidades](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Descargue e instale el [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instalar [!DNL Repo] (herramienta de línea de comandos): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00: Importar proyecto de Maven
* 05:47 - Generar e implementar código fuente con Maven
* 08:17 - Cambios push con Repo
* 14:39 - Cambios de extracción con Repo
* 17:25 - Uso de las herramientas de depuración integradas de IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Código de Visual Studio](https://code.visualstudio.com/)** se ha convertido rápidamente en una herramienta favorita para ***desarrolladores de front-end*** con compatibilidad mejorada con JavaScript, [!DNL Intellisense], y compatibilidad con la depuración del explorador. **[!DNL Visual Studio Code]** es de código abierto, gratuito, con muchas extensiones potentes. [!DNL Visual Studio Code] se puede configurar para integrarlo con AEM con la ayuda de una herramienta de Adobe, **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** También hay varias extensiones compatibles con la comunidad que se pueden instalar para integrar con AEM.

[!DNL Visual Studio Code] es una buena opción para desarrolladores de front-end que escribirán principalmente código CSS/LESS y JavaScript para crear bibliotecas de cliente AEM. Esta herramienta puede no ser la mejor opción para los desarrolladores de AEM nuevos, ya que las definiciones de nodos (cuadros de diálogo, componentes) deberán editarse en XML sin procesar. Hay varias extensiones de Java disponibles para [!DNL Visual Studio Code], sin embargo, si se realiza principalmente el desarrollo de Java [!DNL Eclipse IDE] o [!DNL IntelliJ] puede ser preferible.

#### Vínculos importantes

* [**Descargar**](https://code.visualstudio.com/Download) **Código de Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - Herramienta parecida a FTP para contenido JCR
* **[aemfeed](https://aemfed.io/)** : acelere el flujo de trabajo AEM front-end
* **[Sincronización de AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Extensión compatible con la comunidad* para código de Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importar proyecto Maven
* 00:53 - Generar e implementar código fuente con Maven
* 04:03 - Cambios en el código push con la herramienta de línea de comandos Repo
* 08:29 - Extraer cambios de código con la herramienta de línea de comandos Repo
* 10:40 - Cambios en el código push con la herramienta aemfeed
* 14:24 - Resolución de problemas, reconstrucción de bibliotecas de cliente

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) es una vista basada en explorador del repositorio de AEM. [!DNL CRXDE Lite] está incrustado en AEM y permite a un desarrollador realizar tareas de desarrollo estándar como editar archivos, definir componentes, cuadros de diálogo y plantillas. [!DNL CRXDE Lite] es ***not*** pretende ser un entorno de desarrollo completo, pero es muy eficaz como herramienta de depuración. [!DNL CRXDE Lite] es útil para ampliar o simplemente comprender el código de producto fuera de la base de código. [!DNL CRXDE Lite] proporciona una vista poderosa del repositorio y una forma de probar y administrar los permisos de forma eficaz.

[!DNL CRXDE Lite] siempre debe usarse junto con otros IDE para probar y depurar código, pero nunca como la herramienta de desarrollo principal. Tiene compatibilidad con sintaxis limitada, no capacidades de autocompletar e integración limitada con sistemas de administración de control de código fuente.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Solución de problemas

***Ayuda!*** ¡Mi código no funciona! Al igual que con todo el desarrollo, habrá momentos (probablemente muchos) en los que el código no funciona como se espera. AEM es una plataforma poderosa, pero con bueno poder... viene la complejidad buena. A continuación se presentan algunos puntos de partida de alto nivel en lo que respecta a la resolución de problemas y el seguimiento de problemas (pero lejos de una lista exhaustiva de cosas que pueden salir mal):

### Verificar implementación de código

Un buen primer paso, al encontrar un problema, es verificar que el código se haya implementado e instalado correctamente en AEM.

1. **Marque [!UICONTROL Administrador de paquetes]** para asegurarse de que el paquete de código se ha cargado e instalado: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Compruebe la marca de tiempo para comprobar que el paquete se ha instalado recientemente.
1. Si realiza actualizaciones de archivos incrementales con una herramienta como [!DNL Repo] o [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** que el archivo se ha insertado en la instancia de AEM local y que el contenido del archivo se ha actualizado: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Compruebe que el paquete esté cargado** si aparece algún problema relacionado con el código Java en un paquete OSGi. Abra el [!UICONTROL Consola web de Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) y busque su paquete. Asegúrese de que el paquete tiene un **[!UICONTROL Activo]** estado. Consulte a continuación para obtener más información relacionada con la resolución de problemas de un paquete en un **[!UICONTROL Installed]** estado.

#### Comprobación de los registros

AEM es una plataforma de chat y registra mucha información útil en la **error.log**. La variable **error.log** se encuentra donde se ha instalado AEM: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

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

De forma predeterminada, la variable **error.log** está configurado para el registro *[!DNL INFO]* instrucciones. Si desea cambiar el nivel de registro, puede hacerlo accediendo a [!UICONTROL Compatibilidad de registros]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). También es posible que descubra que la variable **error.log** es demasiado cháchara. Puede usar la variable [!UICONTROL Compatibilidad de registros] para configurar las sentencias de registro únicamente para un paquete Java especificado. Esta es una práctica recomendada para los proyectos, para separar fácilmente los problemas de código personalizado de los problemas de la plataforma de AEM OOTB.

![Configuración de registro en AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### El paquete está en estado de instalación {#bundle-active}

Todos los paquetes (excepto los fragmentos) deben estar en un **[!UICONTROL Activo]** estado. Si ve el paquete de código en un [!UICONTROL Installed] por lo tanto, hay un problema que debe resolverse. La mayoría de las veces se trata de un problema de dependencia:

![Error de paquete en AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

En la captura de pantalla anterior, la [!DNL WKND Core bundle] es un [!UICONTROL Installed] estado. Esto se debe a que el paquete espera una versión diferente de `com.adobe.cq.wcm.core.components.models` que está disponible en la instancia de AEM.

Una herramienta útil que se puede utilizar es la [!UICONTROL Buscador de dependencias]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Añada el nombre del paquete Java para inspeccionar qué versión está disponible en la instancia de AEM:

![Componentes principales](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando con el ejemplo anterior, podemos ver que la versión instalada en la instancia de AEM es **12,2** vs **12,6** que el paquete esperaba. Desde allí puede trabajar hacia atrás y ver si la variable [!DNL Maven] las dependencias de AEM coinciden con la variable [!DNL Maven] en el proyecto AEM. En el ejemplo anterior [!DNL Core Components] **v2.2.0** está instalado en la instancia de AEM, pero el paquete de código se creó con una dependencia de **v2.2.2**, de ahí el motivo del problema de dependencia.

#### Verificación del registro de los modelos Sling {#osgi-component-sling-models}

AEM componentes siempre deben estar respaldados por una [!DNL Sling Model] para encapsular cualquier lógica empresarial y garantizar que el script de renderización HTL permanezca limpio. Si experimenta problemas en los que no se encuentra el modelo Sling, puede resultar útil comprobar la variable [!DNL Sling Models] desde la consola: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Esto le indicará si el modelo de Sling se ha registrado y a qué tipo de recurso (la ruta del componente) está vinculado.

![Estado del modelo Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Muestra el registro de un [!DNL Sling Model], `BylineImpl` que está vinculado a un tipo de recurso de componente `wknd/components/content/byline`.

#### Problemas de CSS o JavaScript

Para la mayoría de los problemas de CSS y JavaScript, el uso de las herramientas de desarrollo del explorador es la forma más eficaz de solucionar problemas. Para reducir el problema al desarrollar con una instancia de autor AEM, es útil ver la página &quot;tal y como aparece publicado&quot;.

![Problemas de CSS o JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Abra el [!UICONTROL Propiedades de página] y haga clic en [!UICONTROL Ver tal y como aparece publicado]. Se abrirá la página sin el editor de AEM y con un parámetro de consulta establecido en **wcmmode=disabled**. Esto deshabilitará la interfaz de usuario de creación de AEM y facilitará la resolución de problemas/depuración de los problemas del front-end.

Otro problema que se encuentra con frecuencia al desarrollar código front-end es que CSS/JS antiguos o obsoletos se están cargando. Como primer paso, asegúrese de que el historial del explorador se haya borrado y, si es necesario, inicie un incógnito en los navegadores o en una nueva sesión.

#### Depuración de bibliotecas de cliente

Con diferentes métodos de categorías e incrustaciones para incluir varias bibliotecas de cliente, puede resultar complicado solucionar problemas. AEM expone varias herramientas para ayudarle con esto. Una de las herramientas más importantes es [!UICONTROL Reconstruir bibliotecas de clientes] lo que obligará a AEM a volver a compilar cualquier archivo LESS y a generar el CSS.

* [Libros volcados](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : enumera todas las bibliotecas de cliente registradas en la instancia de AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Salida de prueba](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permite a un usuario ver la salida de HTML esperada de clientlib incluye en función de la categoría. &lt;host>/libs/granite/ui/content/dumplibs.test
* [Bibliotecas Validación de dependencias](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : resalta las dependencias o categorías incrustadas que no se pueden encontrar. &lt;host>/libs/granite/ui/content/dumplibs.validate
* [Reconstruir bibliotecas de clientes](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : permite a un usuario forzar a AEM para reconstruir todas las bibliotecas de cliente o invalidar la caché de las bibliotecas de cliente. Esta herramienta es especialmente eficaz cuando se desarrolla con LESS, ya que esto puede forzar a AEM a volver a compilar el CSS generado. En general, es más eficaz Invalidar cachés y luego realizar una actualización de página en comparación con la reconstrucción de todas las bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![Depuración de Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Si tiene que invalidar constantemente la caché utilizando la variable [!UICONTROL Reconstruir bibliotecas de clientes] puede valer la pena realizar una única reconstrucción de todas las bibliotecas de cliente. Esto puede tardar unos 15 minutos, pero normalmente elimina cualquier problema de almacenamiento en caché en el futuro.

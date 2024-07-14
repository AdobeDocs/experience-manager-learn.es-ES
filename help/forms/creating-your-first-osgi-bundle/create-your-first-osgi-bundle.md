---
title: Creación de su primer paquete OSGi con AEM Forms
description: Cree su primer paquete OSGi con Maven y Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
duration: 145
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---

# Cree su primer paquete OSGi

Un paquete OSGi es un archivo Java™ que contiene código Java, recursos y un manifiesto que describe el paquete y sus dependencias. El paquete es la unidad de implementación de una aplicación. Este artículo está pensado para desarrolladores que deseen crear un servicio OSGi o un servlet con AEM Forms 6.4 o 6.5. Para crear su primer paquete OSGi, siga los siguientes pasos:


## Instalación de JDK

Instale la versión compatible de JDK. He utilizado JDK1.8. Asegúrese de haber agregado **JAVA_HOME** en las variables de entorno y de que señala a la carpeta raíz de la instalación del JDK.
Agregar %JAVA_HOME%/bin a la ruta de acceso

![origen de datos](assets/java-home.JPG)

>[!NOTE]
> No utilice JDK 15. AEM No es compatible con el servicio de asistencia de la aplicación

### Prueba de la versión de JDK

Abra una nueva ventana del símbolo del sistema y escriba: `java -version`. Debe recuperar la versión del JDK identificado por la variable `JAVA_HOME`

![origen de datos](assets/java-version.JPG)

## Instalar Maven

Maven es una herramienta de automatización de compilaciones que se utiliza principalmente para proyectos Java. Siga los siguientes pasos para instalar maven en su sistema local.

* Cree una carpeta llamada `maven` en la unidad C
* Descargar el [archivo zip binario](https://maven.apache.org/download.cgi)
* Extraiga el contenido del archivo zip en `c:\maven`
* Cree una variable de entorno llamada `M2_HOME` con el valor `C:\maven\apache-maven-3.6.0`. En mi caso, la versión de **mvn** es la 3.6.0. En el momento de escribir este artículo la última versión de Maven es 3.6.3
* Agregar `%M2_HOME%\bin` a la ruta de acceso
* Guarde los cambios
* Abra un nuevo símbolo del sistema y escriba `mvn -version`. Debería ver la versión de **mvn** tal y como se muestra en la captura de pantalla siguiente

![origen de datos](assets/mvn-version.JPG)


## Instalar Eclipse

Instalar la última versión de [eclipse](https://www.eclipse.org/downloads/)

## Creación de su primer proyecto

Archetype es un conjunto de herramientas de creación de plantillas de proyecto de Maven. Un arquetipo se define como un patrón o modelo original a partir del cual se crean todas las demás cosas del mismo tipo. El nombre encaja ya que estamos tratando de proporcionar un sistema que proporcione un medio coherente de generar proyectos Maven. Archetype ayuda a los autores a crear plantillas de proyecto Maven para los usuarios y proporciona a los usuarios los medios para generar versiones parametrizadas de esas plantillas de proyecto.
Para crear su primer proyecto de Maven, siga los siguientes pasos:

* Cree una nueva carpeta llamada `aemformsbundles` en la unidad C
* Abra un símbolo del sistema y vaya a `c:\aemformsbundles`
* Ejecute el siguiente comando en el símbolo del sistema

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Al finalizar correctamente, debería ver un mensaje de éxito de la compilación en la ventana de comandos

## Cree un proyecto Eclipse a partir de su proyecto Maven

* Cambie su directorio de trabajo a `mysite`
* Ejecutar `mvn eclipse:eclipse` desde la línea de comandos. El comando lee su archivo pom y crea proyectos Eclipse con metadatos correctos para que Eclipse entienda los tipos de proyectos, relaciones, rutas de clases, etc.

## Importe el proyecto en Eclipse

Iniciar **Eclipse**

Vaya a **Archivo -> Importar** y seleccione **Proyectos existentes de Maven** como se muestra aquí

![origen de datos](assets/import-mvn-project.JPG)

Haga clic en Siguiente

Seleccione c:\aemformsbundles\mysite haciendo clic en el botón **Examinar**

![origen de datos](assets/mysite-eclipse-project.png)

>[!NOTE]
>Puede seleccionar importar los módulos adecuados según sus necesidades. Seleccione e importe solo el módulo principal si solo va a crear código Java en el proyecto.

Haga clic en **Finalizar** para iniciar el proceso de importación

El proyecto se ha importado a Eclipse y verá un número de `mysite.xxxx` carpetas

Expanda `src/main/java` en la carpeta `mysite.core`. Esta es la carpeta en la que está escribiendo la mayor parte del código.

![origen de datos](assets/mysite-core-project.png)

## Incluir el SDK de cliente de AEMFD

Debe incluir el SDK de cliente de AEMFD en el proyecto para aprovechar los distintos servicios que se incluyen con AEM Forms. Consulte [AEMFD Client SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) para incluir el SDK de cliente apropiado en su proyecto Maven. AEM Tiene que incluir el SDK de cliente de FD de en la sección de dependencias de `pom.xml` del proyecto principal, como se muestra a continuación.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Para crear su proyecto, siga los siguientes pasos:

* Abrir **ventana del símbolo del sistema**
* Navegue hasta `c:\aemformsbundles\mysite\core`
* Ejecutar el comando `mvn clean install -PautoInstallBundle`
AEM El comando anterior crea e instala el paquete en el servidor de la que se ejecuta en `http://localhost:4502`. El paquete también está disponible en el sistema de archivos en
  `C:\AEMFormsBundles\mysite\core\target` y se pueden implementar mediante la [consola web Felix](http://localhost:4502/system/console/bundles)

## Siguientes pasos

[Crear servicio OSGi](./create-osgi-service.md)


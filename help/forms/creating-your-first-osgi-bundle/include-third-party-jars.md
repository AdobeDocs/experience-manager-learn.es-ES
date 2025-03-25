---
title: Inclusión de archivos .jar de terceros
description: Aprenda a utilizar archivos jar de terceros en su proyecto de AEM
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 53
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---

# Inclusión de paquetes de terceros en un proyecto de AEM

En este artículo explicaremos los pasos necesarios para incluir el paquete OSGi de terceros en su proyecto de AEM. A efectos del presente artículo, incluiremos [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) en nuestro proyecto de AEM.  SI el OSGi está disponible en el repositorio de Maven, incluya la dependencia del paquete en el archivo POM.xml del proyecto.

>[!NOTE]
> Se supone que el JAR de terceros es un paquete OSGi

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Si el paquete OSGi está en el sistema de archivos, cree una carpeta llamada **localjar** en el directorio base del proyecto (C:\aemformsbundles\AEMFormsProcessStep\localjar). La dependencia tendrá este aspecto

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Creación de la estructura de carpetas

Estamos agregando este paquete a nuestro proyecto de AEM **AEMFormsProcessStep** que reside en la carpeta **c:\aemformsbundles**

* Abra **filter.xml** desde la carpeta C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault de su proyecto
Tome nota del atributo root del elemento filter.

* Cree la siguiente estructura de carpetas C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* **apps/AEMFormsProcessStep-supplier-packages** es el valor de atributo raíz en filter.xml
* Actualice la sección de dependencias del POM.xml del proyecto
* Abra el símbolo del sistema. Vaya a la carpeta del proyecto (c:\aemformsbundles\AEMFormsProcessStep) en mi caso. Ejecute el siguiente comando

```java
mvn clean install -PautoInstallSinglePackage
```

Si todo va bien, el paquete se instala junto con el paquete de terceros en la instancia de AEM. Puede comprobar el paquete con [felix web console](http://localhost:4502/system/console/bundles). El paquete de terceros está disponible en la carpeta /apps del repositorio `crx`, como se muestra a continuación
![terceros](assets/custom-bundle1.png)

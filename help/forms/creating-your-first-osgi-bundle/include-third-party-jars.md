---
title: Creación de su primer paquete OSGi con AEM Forms
description: Cree su primer paquete OSGi usando Maven y Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: kt-11245
source-git-commit: 061077fb6cd8ac7b760aa30b884ced6d4d3c3b20
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Inclusión de paquetes de terceros en el proyecto AEM

En este artículo explicaremos los pasos involucrados en la inclusión del paquete OSGi de terceros en su proyecto AEM. A los efectos de este artículo, incluiremos el [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) en nuestro proyecto AEM.  SI el OSGi está disponible en el repositorio maven, incluya la dependencia del paquete en el archivo POM.xml del proyecto.

>[!NOTE]
> Se supone que el jar de terceros es un paquete OSGi

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Si su paquete OSGi está en su sistema de archivos, la dependencia se verá algo así

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Crear la estructura de carpetas

Estamos agregando este paquete a nuestro proyecto AEM **AEMFormsProcessStep** que reside en el **c:\aemformsbundles** carpeta

* Abra el **filter.xml** de C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element.

* Cree la siguiente estructura de carpetas C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* La variable **apps/AEMFormsProcessStep-provider-packages** es el valor del atributo raíz en filter.xml
* Actualizar la sección de dependencias del POM.xml del proyecto
* Abra el símbolo del sistema. Vaya a la carpeta de su proyecto (c:\aemformsbundles\AEMFormsProcessStep) en mi caso. Ejecute el siguiente comando

```java
mvn clean install -pAutoInstallSinglePackage
```

Si todo va bien, el paquete se instala junto con el paquete de terceros en la instancia de AEM. Puede comprobar si el paquete utiliza [consola web felix](http://localhost:4502/system/console/bundles). El paquete de terceros está disponible en la carpeta /apps de la `crx` repositorio como se muestra a continuación
![terceros](assets/custom-bundle1.png)
![terceros](assets/custom-bundle1.png)



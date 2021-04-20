---
title: Depuración remota del SDK de AEM
description: El inicio rápido local del SDK de AEM permite la depuración remota de Java desde su IDE, lo que le permite avanzar en la ejecución del código en directo en AEM para comprender el flujo de ejecución exacto.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Depuración remota del SDK de AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

El inicio rápido local del SDK de AEM permite la depuración remota de Java desde su IDE, lo que le permite avanzar en la ejecución del código en directo en AEM para comprender el flujo de ejecución exacto.

Para conectar un depurador remoto a AEM, el inicio rápido local del SDK de AEM debe iniciarse con parámetros específicos (`-agentlib:...`) que permitan que el IDE se conecte a él.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` especifica el puerto en el que AEM escucha para conexiones de depuración remotas y se puede cambiar a cualquier puerto disponible en el equipo de desarrollo local.
+ El último parámetro (p. ej. `aem-author-p4502.jar`) es el Jar de inicio rápido del SDK de AEM. Puede ser el servicio AEM Author (`aem-author-p4502.jar`) o el servicio AEM Publish (`aem-publish-p4503.jar`).

## Instrucciones de configuración de IDE

La mayoría de los IDE de Java son compatibles con la depuración remota de los programas Java. Sin embargo, los pasos exactos de configuración de cada IDE varían. Revise las instrucciones de configuración de depuración remota de su IDE para conocer los pasos exactos. Normalmente, las configuraciones IDE requieren:

+ El inicio rápido local del SDK de AEM del host se está escuchando, que es `localhost`.
+ El inicio rápido local del SDK de AEM del puerto está escuchando la conexión de depuración remota, que es el puerto especificado por el parámetro `address` al iniciar el inicio rápido local del SDK de AEM.
+ En ocasiones, se deben especificar los proyectos de Maven que proporcionan el código fuente para la depuración remota; este es su(s) proyecto(s) de OSGi bundle maven.

### Configuración de instrucciones

+ [Código VS Configuración de depurador remoto Java](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuración de IntelliJ IDEA Remote Debugger](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Eclipse Remote Debugger configurado](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

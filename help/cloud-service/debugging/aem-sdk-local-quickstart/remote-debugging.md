---
title: AEM Depuración remota del SDK de la
description: AEM AEM El inicio rápido local del SDK de la permite la depuración remota de Java desde su IDE, lo que le permite avanzar en la ejecución de código en directo en la práctica para comprender el flujo de ejecución exacto.
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# AEM Depuración remota del SDK de la

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

AEM AEM El inicio rápido local del SDK de la permite la depuración remota de Java desde su IDE, lo que le permite avanzar en la ejecución de código en directo en la práctica para comprender el flujo de ejecución exacto.

AEM AEM Para conectar un depurador remoto a la, el inicio rápido local del SDK de la debe iniciarse con parámetros específicos (`-agentlib:...`), lo que permite al IDE conectarse a él.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK solo admite Java 11
+ `address` AEM especifica el puerto en el que escucha el usuario para las conexiones de depuración remota y se puede cambiar a cualquier puerto disponible en el equipo de desarrollo local.
+ El último parámetro (p. ej. `aem-author-p4502.jar`AEM ) es el Jar de inicio rápido de SKD de. AEM Puede ser el servicio de autor de la (`aem-author-p4502.jar`AEM ) o el servicio Publicación de la (`aem-publish-p4503.jar`).


## Instrucciones de configuración del IDE

La mayoría de los IDE de Java son compatibles con la depuración remota de programas Java; sin embargo, los pasos exactos de configuración de cada IDE varían. Consulte las instrucciones de configuración de depuración remota del IDE para ver los pasos exactos. Normalmente, las configuraciones de IDE requieren:

+ AEM El inicio rápido local del SDK de la host está escuchando, que es `localhost`.
+ AEM El puerto de inicio rápido local del SDK de la está a la escucha de la conexión de depuración remota, que es el puerto especificado por el `address` AEM al iniciar el inicio rápido local del SDK de la.
+ En ocasiones, se deben especificar los proyectos Maven que proporcionan el código fuente para la depuración remota; este es su proyecto de proyectos Maven del paquete OSGi.

### Configuración de instrucciones

+ [Configuración del depurador remoto Java de código VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuración del depurador remoto IntelliJ IDEA](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Configuración del depurador remoto Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

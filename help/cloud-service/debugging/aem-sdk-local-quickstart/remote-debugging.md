---
title: Depuración remota del SDK de AEM
description: El inicio rápido local del SDK de AEM permite la depuración remota de Java desde su IDE, lo que le permite avanzar en la ejecución de código en directo en AEM para comprender el flujo exacto de ejecución.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Depuración remota del SDK de AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

El inicio rápido local del SDK de AEM permite la depuración remota de Java desde su IDE, lo que le permite avanzar en la ejecución de código en directo en AEM para comprender el flujo exacto de ejecución.

Para conectar un depurador remoto a AEM, el inicio rápido local del SDK de AEM debe iniciarse con parámetros específicos (`-agentlib:...`) que permitan que el IDE se conecte a él.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` especifica el puerto AEM escucha las conexiones de depuración remota y se puede cambiar a cualquier puerto disponible en el equipo de desarrollo local.
+ El último parámetro (p. ej. `aem-author-p4502.jar`) es la AEM Jar de inicio rápido SKD. Puede ser el servicio AEM Author (`aem-author-p4502.jar`) o el servicio AEM Publish (`aem-publish-p4503.jar`).

## Instrucciones de configuración de IDE

La mayoría de los IDE de Java son compatibles con la depuración remota de programas Java, aunque los pasos de configuración exactos de cada IDE varían. Revise las instrucciones de configuración de depuración remota del IDE para conocer los pasos exactos. Las configuraciones IDE generalmente requieren:

+ El inicio rápido local del SDK AEM host está escuchando, que es `localhost`.
+ El puerto AEM inicio rápido local del SDK está escuchando la conexión de depuración remota, que es el puerto especificado por el parámetro `address` al iniciar el inicio rápido local del SDK AEM.
+ En ocasiones, se deben especificar los proyectos de Maven que proporcionan el código fuente para la depuración remota; este es su(s) proyecto(s) de compilación OSGi.

### Configurar instrucciones

+ [Configuración del depurador remoto JavaScript de código VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuración del depurador remoto IntelliJ IDEA](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Configuración del depurador remoto de Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

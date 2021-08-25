---
title: Depuración remota del SDK de AEM
description: El inicio rápido local del SDK de AEM permite la depuración remota de Java desde el IDE, lo que le permite avanzar en la ejecución del código en directo en AEM para comprender el flujo de ejecución exacto.
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Depuración remota del SDK de AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

El inicio rápido local del SDK de AEM permite la depuración remota de Java desde el IDE, lo que le permite avanzar en la ejecución del código en directo en AEM para comprender el flujo de ejecución exacto.

Para conectar un depurador remoto a AEM, el inicio rápido local del SDK de AEM debe iniciarse con parámetros específicos (`-agentlib:...`) que permitan que el IDE se conecte a él.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` especifica el puerto AEM escucha para conexiones de depuración remotas y se puede cambiar a cualquier puerto disponible en el equipo de desarrollo local.
+ El último parámetro (p. ej. `aem-author-p4502.jar`) es el Jar de inicio rápido AEM SKD. Puede ser el servicio AEM Author (`aem-author-p4502.jar`) o el servicio AEM Publish (`aem-publish-p4503.jar`).

## Instrucciones de configuración de IDE

La mayoría de los IDE de Java son compatibles con la depuración remota de los programas Java. Sin embargo, los pasos exactos de configuración de cada IDE varían. Revise las instrucciones de configuración de depuración remota de su IDE para conocer los pasos exactos. Normalmente, las configuraciones IDE requieren:

+ El inicio rápido local del SDK de AEM host está escuchando, que es `localhost`.
+ El puerto AEM inicio rápido local del SDK está escuchando la conexión de depuración remota, que es el puerto especificado por el parámetro `address` al iniciar el inicio rápido local del SDK AEM.
+ En ocasiones, se deben especificar los proyectos de Maven que proporcionan el código fuente para la depuración remota; este es su(s) proyecto(s) de OSGi bundle maven.

### Configuración de instrucciones

+ [Código VS Configuración de depurador remoto Java](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuración de IntelliJ IDEA Remote Debugger](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse Remote Debugger configurado](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

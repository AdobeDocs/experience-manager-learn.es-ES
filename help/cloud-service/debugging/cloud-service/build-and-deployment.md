---
title: Creación e implementaciones
description: Adobe Cloud Manager facilita la creación de código y las implementaciones para AEM como Cloud Service. Pueden producirse errores durante los pasos del proceso de compilación, lo que requiere una acción para resolverlos. Esta guía explica los errores comunes en la implementación y cómo abordarlos mejor.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
translation-type: tm+mt
source-git-commit: b9fb3cb0c12afcabf4a92ded3d7d330ac9d229d6
workflow-type: tm+mt
source-wordcount: '2537'
ht-degree: 0%

---


# Depuración AEM como una compilación e implementación de Cloud Service

Adobe Cloud Manager facilita la creación de código y las implementaciones para AEM como Cloud Service. Pueden producirse errores durante los pasos del proceso de compilación, lo que requiere una acción para resolverlos. Esta guía explica los errores comunes en la implementación y cómo abordarlos mejor.

![Canalización de compilación de administración de nube](./assets/build-and-deployment/build-pipeline.png)

## Validación

El paso de validación simplemente garantiza que las configuraciones básicas del Administrador de nube sean válidas. Los errores de validación comunes incluyen:

### El entorno está en un estado no válido

+ __Mensaje de error:__ El entorno está en un estado no válido.
   ![El entorno está en un estado no válido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ el entorno de destinatario de la canalización se encuentra en un estado de transición en el que no puede aceptar nuevas compilaciones.
+ __Resolución:__ espere a que el estado se resuelva en un estado en ejecución (o actualice disponible). Si se está eliminando el entorno, vuelva a crear el entorno o elija otro entorno para generarlo.

### No se encuentra el entorno asociado con la canalización

+ __Mensaje de error:__ El entorno se marca como eliminado.
   ![El entorno se marca como eliminado](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ Se ha eliminado el entorno que está configurado para usar la canalización.
Aunque se vuelva a crear un nuevo entorno con el mismo nombre, Cloud Manager no volverá a asociar automáticamente la canalización a ese mismo entorno con el mismo nombre.
+ __Resolución:__ Edite la configuración de la canalización y vuelva a seleccionar el entorno en el que se va a implementar.

### No se encuentra la rama Git asociada con la tubería

+ __Mensaje de error:Canalización__ no válida: XXXXXX. Motivo=Branch=xxxx no encontrado en el repositorio.
   ![Canalización no válida: XXXXXX. Motivo=Branch=xxxx no encontrado en el repositorio](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ Se ha eliminado la rama Git que está configurada para utilizar la canalización.
+ __Resolución:__ Vuelva a crear la rama Git que falta con el mismo nombre o vuelva a configurar la canalización para que se genere a partir de una rama diferente existente.

## Prueba de unidad y compilación

![Prueba de generación y unidades](./assets/build-and-deployment/build-and-unit-testing.png)

La fase de generación y prueba unitaria realiza una compilación Maven (`mvn clean package`) del proyecto extraído de la rama Git configurada de la canalización.

Los errores detectados en esta fase deben ser reproducibles al construir el proyecto localmente, con las siguientes excepciones:

+ Se utiliza una dependencia válida no disponible en [Maven Central](https://search.maven.org/) y el repositorio Maven que contiene la dependencia es:
   + Inaccesible desde Cloud Manager, como un repositorio privado interno de Maven o el repositorio de Maven requiere autenticación y se han proporcionado credenciales incorrectas.
   + No está registrado explícitamente en el `pom.xml` proyecto. Tenga en cuenta que, al incluir repositorios Maven, se desaconseja ya que aumenta los tiempos de compilación.
+ Las pruebas unitarias fallan debido a problemas de tiempo. Esto puede ocurrir cuando las pruebas unitarias distinguen la distribución de los tiempos. Un indicador sólido depende de `.sleep(..)` en el código de prueba.
+ Uso de complementos Maven no admitidos.

## Análisis de código

![Análisis de código](./assets/build-and-deployment/code-scanning.png)

El análisis de código realiza una análisis de código estático mediante una combinación de optimizaciones específicas de AEM y Java.

El análisis de código resulta en un error de compilación si existe una vulnerabilidad de seguridad crítica en el código. Se pueden anular menos infracciones, pero se recomienda que se corrijan. Tenga en cuenta que el análisis de código es imperfecto y puede resultar en [falsos positivos](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/understand-test-results.html#dealing-with-false-positives).

Para resolver problemas de análisis de código, descargue el informe con formato CSV proporcionado por Cloud Manager mediante el botón **Descargar detalles** y revise las entradas.

Para obtener más información, consulte AEM reglas específicas, consulte las [reglas de digitalización de código específicas de AEM personalizadas de Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Generar imágenes

![Generar imágenes](./assets/build-and-deployment/build-images.png)

La imagen de compilación es responsable de combinar los artefactos de código creados creados en el paso de la prueba de creación y unidad con la versión de AEM, para formar un único artefacto implementable.

Aunque se encuentran problemas de compilación y compilación de código durante la prueba de compilación y unidad, puede haber problemas estructurales o de configuración identificados al intentar combinar el artefacto de compilación personalizada con la versión de AEM.

### Configuraciones de OSGi de duplicado

Cuando varias configuraciones OSGi se resuelven mediante runmode para el entorno de AEM destinatario, el paso Generar imagen falla con el error:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ el paquete completo del proyecto de AEM contiene varios paquetes de código y más de uno de los paquetes de código proporciona la misma configuración OSGi, lo que provoca un conflicto, por lo que el paso Generar imagen no puede decidir qué se debe usar, con lo que se produce un error en la compilación. Tenga en cuenta que esto no se aplica a las configuraciones de fábrica de OSGi, siempre que tengan nombres únicos.
+ __Resolución:__ Revise todos los paquetes de código (incluidos los paquetes de código de terceros incluidos) que se implementan como parte de la aplicación de AEM, buscando configuraciones OSGi de duplicado que se resuelven, mediante runmode, en el entorno de destinatario. La guía del mensaje de error de &quot;establecer el indicador mergeConfigurations en true&quot; no es posible en AEM como servicio de nube y debe ignorarse.

#### Causa 2

+ __Causa:__ El proyecto de AEM incluye incorrectamente el mismo paquete de código dos veces, lo que resulta en la duplicación de cualquier configuración OSGi contenida en dicho paquete.
+ __Resolución:__ Revise todos los paquetes de pom.xml incrustados en el proyecto completo y asegúrese de que tienen la  `filevault-package-maven-plugin` [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) configuración establecida en  `<cloudManagerTarget>none</cloudManagerTarget>`.

### Secuencia de comandos de reenvío mal formada

Las secuencias de comandos de informes definen el contenido de línea de base, los usuarios, las ACL, etc. En AEM como Cloud Service, las secuencias de comandos de informes se aplican durante la generación de imágenes, pero en AEM inicio rápido local del SDK se aplican cuando se activa la configuración de fábrica de informes OSGi. Debido a esto, es posible que las secuencias de comandos de informe no se ejecuten correctamente (con el registro) en AEM inicio rápido local del SDK, pero que se produzca un error en el paso Generar imagen, lo que detendrá la implementación.

+ __Causa:__ Una secuencia de comandos de informe tiene un formato incorrecto. Tenga en cuenta que esto puede dejar el repositorio en un estado incompleto, ya que cualquier secuencia de comandos de informe después de que la secuencia de comandos fallida se ejecute en el repositorio.
+ __Resolución:__ Revise el inicio rápido local del SDK de AEM cuando se implemente la configuración OSGi de la secuencia de comandos de informe para determinar si los errores son y cuáles.

### Dependencia de contenido de informes no satisfecha

Las secuencias de comandos de informes definen el contenido de línea de base, los usuarios, las ACL, etc. En AEM inicio rápido local del SDK, las secuencias de comandos de informes se aplican cuando se activa la configuración de fábrica de OSGi del informe o, en otras palabras, después de que el repositorio esté activo y pueda haber realizado cambios de contenido directamente o a través de paquetes de contenido. En AEM como Cloud Service, las secuencias de comandos de informe se aplican durante la generación de imágenes a un repositorio del que puede no haber contenido del que dependa la secuencia de comandos de informe.

+ __Causa:__ una secuencia de comandos de informe depende del contenido que no existe.
+ __Resolución:__ asegúrese de que existe el contenido del que depende la secuencia de comandos de informe. A menudo, esto indica que hay secuencias de comandos de informe insuficientemente definidas que carecen de directivas que definan estas estructuras de contenido que faltan, pero que son necesarias. Esto se puede reproducir localmente eliminando AEM, desempaquetando la Jar y agregando la configuración OSGi de informe que contiene la secuencia de comandos de informe a la carpeta de instalación, y comenzando AEM. El error se presentará en el archivo error.log del inicio rápido local del SDK de AEM.


### La versión de los componentes principales de la aplicación es buena a la versión implementada

_Este problema solo afecta a los entornos que no son de producción y que NO se actualizan automáticamente a la última versión de AEM._

AEM como Cloud Service incluye automáticamente la versión más reciente de Componentes principales en cada versión de AEM, lo que significa que, después de que se actualice automática o manualmente un AEM como entorno Cloud Service, se ha implementado la versión más reciente de Componentes principales.

Es posible que el paso Generar imagen falle cuando:

+ La aplicación de implementación actualiza la versión de dependencia de Core Components en el proyecto `core` (paquete OSGi)
+ A continuación, la aplicación de implementación se implementa en un entorno limitado (sin producción) AEM como entorno de Cloud Service que no se ha actualizado para utilizar una versión de AEM que contenga esa nueva versión de componentes principales.

Para evitar este error, cada vez que haya disponible una actualización del AEM como entorno de Cloud Service, incluya la actualización como parte de la siguiente compilación/implementación y asegúrese siempre de que las actualizaciones se incluyen después de incrementar la versión de los componentes principales en la base de código de la aplicación.

+ __Síntomas:__
el paso Generar imagen falla con un sistema de informes ERROR que 
`com.adobe.cq.wcm.core.components...` el  `core` proyecto no pudo importar los paquetes en intervalos de versiones específicos.

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __Causa:__  el paquete OSGi de la aplicación (definido en el  `core` proyecto) importa clases Java de la dependencia principal de los componentes principales, en un nivel de versión diferente al que se implementa en AEM como Cloud Service.
+ __Resolución:__
   + Con Git, vuelva a una confirmación de trabajo que exista antes del incremento de la versión del componente principal. Inserte esta confirmación en una rama Git del Administrador de nube y realice una actualización del entorno desde esta rama. Esto actualizará AEM como Cloud Service a la versión de AEM más reciente, que incluirá la versión de componentes principales más reciente. Una vez que el AEM como Cloud Service se haya actualizado a la versión de AEM más reciente, que tendrá la versión más reciente de Componentes principales, vuelva a implementar el código de error original.
   + Para reproducir este problema de forma local, asegúrese de que la versión AEM del SDK sea la misma AEM versión que utiliza el AEM como entorno Cloud Service.


### Creación de un caso de soporte de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de soporte de Adobe mediante:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Ficha Asistencia > Crear caso

   _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe que tiene una canalización defectuosa esté seleccionada en el conmutador de organizaciones de Adobe antes de crear el caso._

## Implementar en

El paso Implementar en es responsable de tomar el artefacto de código generado en Generar imagen, inicio los nuevos servicios AEM Author y Publish que lo utilizan y, una vez que se ha realizado correctamente, elimina los antiguos servicios AEM Author y Publish. Los índices y paquetes de contenido mutable también se instalan y actualizan en este paso.

Familiarícese con [AEM como registros de Cloud Service](./logs.md) antes de depurar el paso Implementar en. El registro `aemerror` contiene información sobre el inicio y el cierre de pods que puede ser pertinente para la implementación en problemas. Tenga en cuenta que el registro disponible mediante el botón Descargar registro en el paso Implementar en del Administrador de nube no es el registro `aemerror` y no contiene información detallada relacionada con el inicio de sus aplicaciones.

![Implementar en](./assets/build-and-deployment/deploy-to.png)

Los tres motivos principales por los que el paso Implementar en puede fallar:

### La canalización de Cloud Manager contiene una versión AEM antigua

+ __Causa:__ Una canalización de Cloud Manager contiene una versión de AEM anterior a la implementada en el entorno de destinatario. Esto puede suceder cuando se reutiliza una canalización y se señala a un nuevo entorno que está ejecutando una versión posterior de AEM. Esto se puede identificar comprobando si la versión AEM del entorno es buena que la versión AEM del canal.
   ![La canalización de Cloud Manager contiene una versión AEM antigua](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Resolución:__
   + Si el entorno de destinatario tiene una actualización disponible, seleccione Actualizar en las acciones del entorno y, a continuación, vuelva a ejecutar la compilación.
   + Si el entorno de destinatario no tiene una actualización disponible, significa que está ejecutando la última versión de AEM. Para resolver esto, elimine la canalización y vuelva a crearla.


### Tiempo de espera de Cloud Manager

El código que se ejecuta durante el inicio del servicio de AEM recientemente implementado lleva tanto tiempo que el tiempo de espera de Cloud Manager finaliza antes de que se pueda completar la implementación. En estos casos, la implementación puede tener éxito, incluso aunque el estado del Administrador de nube haya informado como Fallido.

+ __Causa:El código__ personalizado puede ejecutar operaciones, como grandes consultas o transiciones de contenido, que se activan al principio en ciclos de vida de componentes o paquetes OSGi y retrasan significativamente el tiempo de activación del inicio de AEM.
+ __Resolución:__ Revise la implementación del código que se ejecuta al principio del ciclo vital del OSGi Bundle, y revise los  `aemerror` registros de AEM Author y los servicios de publicación en el momento del error (hora de registro en GMT) como muestra el Administrador de nube, y busque mensajes de registro que indiquen cualquier proceso de registro personalizado que se esté ejecutando.

### Código o configuración incompatible

La mayoría de las infracciones de código y configuración se detectan anteriormente en la compilación, pero es posible que el código personalizado o la configuración sean incompatibles con la AEM como Cloud Service y no se detecten hasta que se ejecute en el contenedor.

+ __Causa:el código__ personalizado puede provocar operaciones largas, como grandes consultas o transiciones de contenido, que se activan al principio en ciclos de vida de componentes o paquetes OSGi, lo que retrasa considerablemente el tiempo de activación del inicio de AEM.
+ __Resolución:__ Revise los  `aemerror` registros de los servicios de AEM Author y Publish a la hora (hora de registro en GMT) del error, como muestra el Administrador de nube.
   1. Revise los registros de cualquier ERROR generado por las clases de Java proporcionadas por la aplicación personalizada. Si se encuentra algún problema, resuelva, inserte el código fijo y vuelva a generar la canalización.
   1. Revise los registros de cualquier ERROR informado por aspectos de AEM que esté ampliando o interactuando en la aplicación personalizada, e investigue dichos errores; es posible que estos ERRORES no se atribuyan directamente a las clases de Java. Si se encuentra algún problema, resuelva, inserte el código fijo y vuelva a generar la canalización.

### Inclusión de /var en el paquete de contenido

`/var` es mutable y contiene una variedad de contenido transitorio en tiempo de ejecución. Inclusión de `/var` en un paquete de contenido (por ejemplo: `ui.content`) implementada mediante Cloud Manager puede provocar que el paso de implementación falle.

Este problema es difícil de identificar, ya que no da lugar a un error en la implementación inicial, sólo en implementaciones posteriores. Los síntomas notables incluyen:

+ La implementación inicial se realiza correctamente, aunque el contenido mutable nuevo o modificado, que forma parte de la implementación, no parece existir en el servicio AEM Publish.
+ Se bloquea la activación/desactivación de contenido en AEM Author
+ Las implementaciones posteriores fallan en el paso Implementar en, con el paso Implementar en que fallará después de aproximadamente 60 minutos.

Validar este problema es la causa del comportamiento fallido:

1. Al determinar que al menos un paquete de contenido que forma parte de la implementación, escribe `/var`.
1. Verifique que la cola de distribución principal (en negrita) esté bloqueada en:
   + AEM Author > Herramientas > Implementación > Distribución
      ![Cola de distribución bloqueada](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. Al fallar la implementación posterior, descargue los registros de &quot;Implementar en&quot; de Cloud Manager mediante el botón Descargar registro:

   ![Descargar implementación en registros](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... y verifique que haya aproximadamente 60 minutos entre las sentencias de registro:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... y ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Tenga en cuenta que este registro no contendrá estos indicadores en las implementaciones iniciales que informan de que tienen éxito, sino únicamente en implementaciones que han fallado posteriormente.

+ __Causa:__ AEM usuario del servicio de replicación utilizado para implementar paquetes de contenido en el servicio AEM Publish no puede escribir  `/var` en AEM Publish. Esto provoca que falle la implementación del paquete de contenido en el servicio AEM Publish.
+ __Resolución:__ Las siguientes formas de resolver estos problemas se enumeran en orden de preferencia:
   1. Si los `/var` recursos no son necesarios, elimine los recursos de `/var` de los paquetes de contenido que se implementan como parte de la aplicación.
   2. Si los recursos `/var` son necesarios, defina las estructuras de nodos utilizando [repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). Los scripts de informes se pueden dirigir a AEM Author, AEM Publish o ambos, a través de los modos de ejecución OSGi.
   3. Si los `/var` recursos solo son necesarios en AEM autor y no se pueden modelar razonablemente mediante [repoinit](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit), muévalos a un paquete de contenido discreto, que solo se instala en AEM Author mediante [incrustándolo](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) en el paquete `all` de una carpeta de modo de ejecución de AEM Author (`<target>/apps/example-packages/content/install.author</target>`).
   4. Proporcione las ACL apropiadas al usuario de servicio `sling-distribution-importer` como se describe en esta [KB de Adobe](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Creación de un caso de soporte de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de soporte de Adobe mediante:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Ficha Asistencia > Crear caso

   _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe que tiene una canalización defectuosa esté seleccionada en el conmutador de organizaciones de Adobe antes de crear el caso._

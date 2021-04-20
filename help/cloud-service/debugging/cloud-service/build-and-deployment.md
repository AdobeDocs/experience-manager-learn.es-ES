---
title: Compilación e implementaciones
description: Adobe Cloud Manager facilita la creación de código y las implementaciones en AEM as a Cloud Service. Pueden producirse errores durante los pasos del proceso de compilación, lo que requiere una acción para resolverlos. Esta guía explica los errores comunes en la implementación y cómo abordarlos mejor.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2542'
ht-degree: 0%

---


# Depuración de AEM as a Cloud Service e implementaciones

Adobe Cloud Manager facilita la creación de código y las implementaciones en AEM as a Cloud Service. Pueden producirse errores durante los pasos del proceso de compilación, lo que requiere una acción para resolverlos. Esta guía explica los errores comunes en la implementación y cómo abordarlos mejor.

![Canalización de generación de Cloud](./assets/build-and-deployment/build-pipeline.png)

## Validación

El paso de validación simplemente garantiza que las configuraciones básicas de Cloud Manager sean válidas. Los errores de validación comunes incluyen:

### El entorno está en un estado no válido

+ __Mensaje de error:__ el entorno está en un estado no válido.
   ![El entorno está en un estado no válido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ el entorno de destino de la canalización está en un estado de transición, momento en el que no puede aceptar nuevas compilaciones.
+ __Solución:__ espere a que el estado se resuelva en un estado en ejecución (o actualice disponible). Si se va a eliminar el entorno, vuelva a crearlo o elija otro entorno para crearlo.

### No se encuentra el entorno asociado a la canalización

+ __Mensaje de error:__ El entorno se marca como eliminado.
   ![El entorno se marca como eliminado](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ se ha eliminado el entorno en el que está configurada la canalización para usar.
Aunque se vuelva a crear un nuevo entorno con el mismo nombre, Cloud Manager no volverá a asociar automáticamente la canalización a ese entorno con el mismo nombre.
+ __Solución:__ edite la configuración de la canalización y vuelva a seleccionar el entorno al que desee implementar.

### No se encuentra la rama Git asociada a la canalización

+ __Mensaje de error:__ Canalización no válida: XXXXXX. Reason=Branch=xxxx no se encuentra en el repositorio.
   ![Canalización no válida: XXXXXX. Reason=Branch=xxxx no se encuentra en el repositorio](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ se ha eliminado la rama Git que la canalización está configurada para usar.
+ __Solución:__ vuelva a crear la rama de Git que falta con el mismo nombre o vuelva a configurar la canalización para que se cree desde una rama diferente y existente.

## Prueba de compilación y unidad

![Prueba de compilación y unidad](./assets/build-and-deployment/build-and-unit-testing.png)

La fase Prueba de compilación y unidad realiza una compilación de Maven (`mvn clean package`) del proyecto extraído de la rama de Git configurada de la canalización.

Los errores identificados en esta fase deben ser reproducibles al crear el proyecto localmente, con las siguientes excepciones:

+ Se utiliza una dependencia maven no disponible en [Maven Central](https://search.maven.org/) y el repositorio Maven que contiene la dependencia es:
   + No se puede acceder desde Cloud Manager, como un repositorio privado interno de Maven o el repositorio de Maven requiere autenticación y se han proporcionado credenciales incorrectas.
   + No está registrado explícitamente en el `pom.xml` del proyecto. Tenga en cuenta que, incluidos los repositorios de Maven, no se recomienda porque aumenta los tiempos de compilación.
+ Las pruebas unitarias fallan debido a problemas de tiempo. Esto puede ocurrir cuando las pruebas unitarias son sensibles al tiempo. Un indicador sólido se basa en `.sleep(..)` en el código de prueba.
+ El uso de complementos Maven no compatibles.

## Escaneo de código

![Escaneo de código](./assets/build-and-deployment/code-scanning.png)

El análisis de código realiza un análisis estático del código mediante una combinación de prácticas recomendadas específicas de Java y AEM.

El análisis de código produce un error de compilación si existen vulnerabilidades de seguridad crítica en el código. Se pueden anular menos infracciones, pero se recomienda que se corrijan. Tenga en cuenta que el análisis de código es imperfecto y puede resultar en [falsos positivos](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/understand-test-results.html#dealing-with-false-positives).

Para resolver los problemas de digitalización de código, descargue el informe con formato CSV proporcionado por Cloud Manager a través del botón **Download Details** y revise las entradas.

Para obtener más información, consulte Reglas específicas de AEM, consulte la documentación de Cloud Manager [reglas de análisis de código personalizadas específicas de AEM](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Crear imágenes

![Crear imágenes](./assets/build-and-deployment/build-images.png)

La imagen de compilación es responsable de combinar los artefactos de código creados creados creados en el paso Generar y prueba de unidad con la versión de AEM, para formar un único artefacto implementable.

Aunque se encuentran problemas de compilación y compilación de código durante la Prueba de compilación y unidad, puede haber problemas estructurales o de configuración identificados al intentar combinar el artefacto de compilación personalizado con la versión de AEM.

### Duplicar configuraciones de OSGi

Cuando se resuelven varias configuraciones de OSGi mediante el modo de ejecución para el entorno AEM de destino, el paso Generar imagen falla con el error:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ el paquete completo del proyecto AEM contiene varios paquetes de código, y la misma configuración OSGi la proporcionan más de uno de los paquetes de código, lo que provoca un conflicto, lo que provoca que el paso Generar imagen no pueda decidir cuál debe utilizarse, por lo que falla la compilación. Tenga en cuenta que esto no se aplica a las configuraciones de fábrica de OSGi, siempre que tengan nombres únicos.
+ __Solución:__ revise todos los paquetes de código (incluidos los paquetes de código de terceros incluidos) que se implementen como parte de la aplicación AEM, buscando configuraciones OSGi duplicadas que se resuelvan, a través del modo de ejecución, en el entorno de destino. La guía del mensaje de error &quot;establecer el indicador mergeConfigurations en true&quot; no es posible en AEM as a Cloud Service y debe ignorarse.

#### Causa 2

+ __Causa:__ el proyecto de AEM incluye incorrectamente el mismo paquete de código dos veces, lo que resulta en la duplicación de cualquier configuración de OSGi contenida en dicho paquete.
+ __Solución:__ revise todos los paquetes pom.xml incrustados en todo el proyecto y asegúrese de que tienen la  `filevault-package-maven-plugin` [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) configuración configurada en  `<cloudManagerTarget>none</cloudManagerTarget>`.

### Secuencia de comandos de repoción malformada

Las secuencias de comandos de informe definen el contenido de línea de base, los usuarios, las ACL, etc. En AEM as a Cloud Service, los scripts de informe se aplican durante la generación de imagen, pero en el inicio rápido local del SDK de AEM se aplican cuando se activa la configuración de fábrica de informes OSGi. Debido a esto, los scripts de Report pueden fallar silenciosamente (con el inicio de sesión) en el inicio rápido local del SDK de AEM, pero pueden provocar que el paso Generar imagen falle y detener la implementación.

+ __Causa:__ el formato de una secuencia de comandos de informe no es correcto. Tenga en cuenta que esto puede dejar el repositorio en un estado incompleto, ya que cualquier secuencia de comandos de informe después de que la secuencia de comandos fallida se ejecutará en el repositorio.
+ __Solución:__ revise el inicio rápido local del SDK de AEM cuando se implemente la configuración OSGi del script de informe para determinar si los errores son y cuáles son.

### Dependencia de contenido de informe insatisfactoria

Las secuencias de comandos de informe definen el contenido de línea de base, los usuarios, las ACL, etc. En el inicio rápido local del SDK de AEM, los scripts de informe se aplican cuando se activa la configuración de fábrica de OSGi del repositorio, o en otras palabras, después de que el repositorio esté activo y puede haber sufrido cambios de contenido directamente o a través de paquetes de contenido. En AEM as a Cloud Service, los scripts de puntos se aplican durante la compilación de la imagen a un repositorio que puede no contener contenido del que depende el script de informe.

+ __Causa:__ una secuencia de comandos de informe depende del contenido que no existe.
+ __Solución:__ asegúrese de que existe el contenido del que depende la secuencia de comandos de informe. A menudo, esto indica secuencias de comandos de informe insuficientemente definidas que carecen de directivas que definan estas estructuras de contenido que faltan, pero que son necesarias. Esto se puede reproducir localmente eliminando AEM, desempaquetando el Jar y añadiendo la configuración OSGi del informe que contiene el script de informe a la carpeta de instalación, e iniciando AEM. El error se presentará en el archivo error.log local de inicio rápido del SDK de AEM.


### La versión de los componentes principales de la aplicación es mayor que la versión implementada

_Este problema solo afecta a los entornos que no son de producción y que NO se actualizan automáticamente a la última versión de AEM._

AEM as a Cloud Service incluye automáticamente la última versión de los componentes principales en cada versión de AEM, lo que significa que después de que se haya implementado la última versión de los componentes principales en el entorno de AEM as a Cloud Service de forma automática o manual.

Es posible que el paso Generar imagen falle cuando:

+ La aplicación de implementación actualiza la versión de dependencia de los componentes principales de maven en el proyecto `core` (paquete OSGi)
+ A continuación, la aplicación de implementación se implementa en un entorno de entorno limitado (que no sea de producción) de AEM as a Cloud Service que no se ha actualizado para utilizar una versión de AEM que contenga esa nueva versión de componentes principales.

Para evitar este error, siempre que haya disponible una actualización del entorno de AEM as a Cloud Service , incluya la actualización como parte de la siguiente compilación/implementación y asegúrese siempre de que las actualizaciones se incluyen después de incrementar la versión de los componentes principales en la base de código de la aplicación.

+ __Síntomas:__
el paso Generar imagen falla y ERROR informa de que 
`com.adobe.cq.wcm.core.components...` el  `core` proyecto no pudo importar los paquetes en intervalos de versiones específicos.

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __Causa:__  el paquete OSGi de la aplicación (definido en el  `core` proyecto) importa clases Java de la dependencia principal de los componentes principales, a un nivel de versión diferente al que se implementa en AEM as a Cloud Service.
+ __Resolución:__
   + Con Git, revierta a una confirmación de trabajo existente antes del incremento de la versión del componente principal. Empuje esta confirmación a una rama de Git de Cloud Manager y realice una actualización del entorno desde esta rama. Esto actualizará AEM as a Cloud Service a la última versión de AEM, que incluirá la versión posterior de los componentes principales. Una vez que AEM as a Cloud Service se actualice a la última versión de AEM, que tendrá la última versión de los componentes principales, vuelva a implementar el código que originalmente falló.
   + Para reproducir este problema localmente, asegúrese de que la versión del SDK de AEM sea la misma versión de la versión de AEM que utiliza el entorno de AEM as a Cloud Service .


### Crear un caso de asistencia de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de asistencia de Adobe a través de:

+ [Adobe Admin Console](https://adminconsole.adobe.com)  > Pestaña Asistencia > Crear caso

   _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe que tiene una canalización fallida esté seleccionada en el conmutador de organizaciones de Adobe antes de crear el caso._

## Implementar en

El paso Implementar en es responsable de tomar el artefacto de código generado en la imagen de compilación, inicia los nuevos servicios de AEM Author y Publish que lo utilizan y, una vez realizado correctamente, elimina cualquier servicio antiguo de AEM Author y Publish. Los paquetes de contenido mutables y los índices también se instalan y actualizan en este paso.

Familiarícese con [AEM as a Cloud Service logs](./logs.md) antes de depurar el paso Implementar en . El registro `aemerror` contiene información sobre el inicio y cierre de pods que puede ser pertinente para Implementar en problemas. Tenga en cuenta que el registro disponible mediante el botón Descargar registro en el paso Implementar en de Cloud Manager no es el registro `aemerror` y no contiene información detallada relacionada con el inicio de sus aplicaciones.

![Implementar en](./assets/build-and-deployment/deploy-to.png)

Las tres razones principales por las que el paso Implementar en puede fallar:

### La canalización de Cloud Manager contiene una versión antigua de AEM

+ __Causa:__ una canalización de Cloud Manager contiene una versión de AEM anterior a la implementada en el entorno de destino. Esto puede ocurrir cuando se reutiliza una canalización y se señala a un nuevo entorno que ejecuta una versión posterior de AEM. Esto se puede identificar comprobando si la versión de AEM del entorno es mayor que la versión de AEM de la canalización.
   ![La canalización de Cloud Manager contiene una versión antigua de AEM](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Resolución:__
   + Si el entorno de destino tiene una actualización disponible, seleccione Actualizar en las acciones del entorno y, a continuación, vuelva a ejecutar la compilación.
   + Si el entorno de destino no tiene una actualización disponible, significa que está ejecutando la última versión de AEM. Para resolver esto, elimine la canalización y vuelva a crearla.


### Se agota el tiempo de espera de Cloud Manager

El código que se ejecuta durante el inicio del servicio AEM recién implementado tarda tanto que Cloud Manager supera el tiempo de espera antes de que se pueda completar la implementación. En estos casos, la implementación puede tener éxito, incluso aunque el estado de Cloud Manager informe sea Failed.

+ __Causa:__ El código personalizado puede ejecutar operaciones, como consultas grandes o transmisiones de contenido, activadas al principio en el paquete OSGi o ciclos de vida del componente retrasando significativamente el tiempo de inicio de AEM.
+ __Solución:__ revise la implementación para el código que se ejecuta al principio del ciclo de vida del paquete OSGi, y revise los  `aemerror` registros de AEM Author y Publish Services alrededor de la hora del error (hora de registro en GMT) como se muestra en Cloud Manager, y busque mensajes de registro que indiquen cualquier proceso de registro personalizado que ejecute.

### Código o configuración incompatible

La mayoría de las infracciones de código y configuración se detectan anteriormente en la compilación, pero es posible que el código personalizado o la configuración sean incompatibles con AEM as a Cloud Service y no se detecten hasta que se ejecuten en el contenedor.

+ __Causa:__ El código personalizado puede invocar operaciones largas, como consultas grandes o transmisiones de contenido, que se desencadenan temprano en el paquete OSGi o en los ciclos de vida de los componentes, lo que retrasa significativamente el tiempo de inicio de AEM.
+ __Solución:__ revise los  `aemerror` registros de los servicios de AEM Author y Publish alrededor de la hora (hora de registro en GMT) del error, como muestra Cloud Manager.
   1. Revise los registros para ver si hay ERRORES lanzados por las clases Java proporcionadas por la aplicación personalizada. Si se encuentran problemas, resuelva, inserte el código fijo y vuelva a compilar la canalización.
   1. Revise los registros para detectar cualquier ERROR informado por aspectos de AEM con los que esté ampliando o interactuando en su aplicación personalizada, e investigue dichos ERRORES; es posible que estos ERRORES no se atribuyan directamente a las clases Java. Si se encuentran problemas, resuelva, inserte el código fijo y vuelva a compilar la canalización.

### Inclusión de /var en el paquete de contenido

`/var` es mutable con una variedad de contenido transitorio y de tiempo de ejecución. Inclusión de `/var` en paquetes de contenido (por ejemplo, `ui.content`) implementada mediante Cloud Manager puede provocar que el paso Implementar falle.

Es difícil identificar este problema, ya que no provoca un error en la implementación inicial, solo en implementaciones posteriores. Los síntomas notables incluyen:

+ La implementación inicial se realiza correctamente, aunque el contenido mutable nuevo o modificado, que forma parte de la implementación, no parece existir en el servicio AEM Publish.
+ La activación/desactivación del contenido en AEM Author está bloqueada
+ Las implementaciones posteriores fallan en el paso Implementar en , con el paso Implementar en fallando después de aproximadamente 60 minutos.

Para validar este problema es la causa del comportamiento fallido:

1. Al determinar que al menos un paquete de contenido que forma parte de la implementación, escribe en `/var`.
1. Compruebe que la cola de distribución principal (en negrita) esté bloqueada en:
   + Autor de AEM > Herramientas > Implementación > Distribución
      ![Cola de distribución bloqueada](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. Al fallar la implementación posterior, descargue los registros &quot;Implementar en&quot; de Cloud Manager con el botón Descargar registro:

   ![Descargar implementación en registros](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... y verifique que haya aproximadamente 60 minutos entre las instrucciones de registro:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... y ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Tenga en cuenta que este registro no contendrá estos indicadores en las implementaciones iniciales que informan que son exitosas, sino solo en las implementaciones fallidas posteriores.

+ __Causa:__ el usuario del servicio de replicación de AEM que se usa para implementar paquetes de contenido en el servicio de publicación de AEM no puede escribir  `/var` en AEM Publish. Esto provoca que falle la implementación del paquete de contenido en el servicio AEM Publish.
+ __Solución:__ Las siguientes formas de resolver estos problemas se enumeran en orden de preferencia:
   1. Si los `/var` recursos no son necesarios, elimine los recursos en `/var` de los paquetes de contenido que se implementan como parte de la aplicación.
   2. Si los `/var` recursos son necesarios, defina las estructuras de nodos utilizando [report](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). Los scripts de informe se pueden dirigir a AEM Author, AEM Publish o ambos, a través de los modos de ejecución OSGi.
   3. Si los `/var` recursos solo son necesarios en AEM Author y no se pueden modelar razonablemente utilizando [report](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit), muévalos a un paquete de contenido discreto, que solo se instala en AEM Author [incrustándolo](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) en el paquete `all` en una carpeta de modo de ejecución de AEM Author (`<target>/apps/example-packages/content/install.author</target>`).
   4. Proporcione ACL adecuadas al usuario del servicio `sling-distribution-importer` tal como se describe en esta [base de conocimiento de Adobe](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Crear un caso de asistencia de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de asistencia de Adobe a través de:

+ [Adobe Admin Console](https://adminconsole.adobe.com)  > Pestaña Asistencia > Crear caso

   _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe que tiene una canalización fallida esté seleccionada en el conmutador de organizaciones de Adobe antes de crear el caso._

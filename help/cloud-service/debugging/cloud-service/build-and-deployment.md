---
title: Compilación e implementaciones
description: Adobe Cloud Manager facilita la compilación del código y las implementaciones en AEM as a Cloud Service. Pueden producirse errores durante los pasos del proceso de generación, que requieren acciones para resolverlos. Esta guía explica cómo comprender los errores comunes de en la implementación y cómo abordarlos mejor.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
duration: 534
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 0%

---

# Depuración de la compilación y las implementaciones de AEM as a Cloud Service

Adobe Cloud Manager facilita la compilación del código y las implementaciones en AEM as a Cloud Service. Pueden producirse errores durante los pasos del proceso de generación, que requieren acciones para resolverlos. Esta guía explica cómo comprender los errores comunes de en la implementación y cómo abordarlos mejor.

![Canalización de compilación de Cloud Manager](./assets/build-and-deployment/build-pipeline.png)

## Validación

El paso de validación simplemente garantiza que las configuraciones básicas de Cloud Manager sean válidas. Los errores de validación comunes incluyen:

### El entorno está en un estado no válido

+ __Mensaje de error:__ El entorno está en un estado no válido.
  ![El entorno está en un estado no válido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ El entorno de destino de la canalización se encuentra en un estado de transición en el que no puede aceptar nuevas compilaciones.
+ __Resolución:__ Espere a que el estado se resuelva en un estado en ejecución (o actualización disponible). Si se elimina el entorno, vuelva a crearlo o elija un entorno diferente al que compilar.

### No se encuentra el entorno asociado a la canalización

+ __Mensaje de error:__ El entorno está marcado como eliminado.
  ![El entorno se ha marcado como eliminado](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ Se ha eliminado el entorno que la canalización está configurada para usar.
Incluso si se vuelve a crear un nuevo entorno con el mismo nombre, Cloud Manager no vuelve a asociar automáticamente la canalización a ese entorno con el mismo nombre.
+ __Resolución:__ Edite la configuración de la canalización y vuelva a seleccionar el entorno en el que desea realizar la implementación.

### No se encuentra la rama Git asociada a la canalización

+ __Mensaje de error:__ Canalización no válida: XXXXXX. Motivo=No se encuentra Branch=xxxx en el repositorio.
  ![Canalización no válida: XXXXXX. Motivo=No se encuentra Branch=xxxx en el repositorio](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ Se ha eliminado la rama Git que la canalización está configurada para usar.
+ __Resolución:__ Vuelva a crear la rama Git que falta con el mismo nombre o vuelva a configurar la canalización para que se genere a partir de una rama diferente existente.

## Prueba de compilación y unidad

![Prueba de compilación y unidad](./assets/build-and-deployment/build-and-unit-testing.png)

La fase Prueba de generación y unidad realiza una generación Maven (`mvn clean package`) del proyecto desprotegido de la rama Git configurada de la canalización.

Los errores identificados en esta fase deben ser reproducibles generando el proyecto localmente, con las siguientes excepciones:

+ Se usa una dependencia Maven no disponible en [Maven Central](https://search.maven.org/), y el repositorio Maven que contiene la dependencia es:
   + No accesible desde Cloud Manager, como un repositorio Maven interno privado, o el repositorio Maven requiere autenticación y se han proporcionado credenciales incorrectas.
   + No se registró explícitamente en `pom.xml` del proyecto. Tenga en cuenta que, no se recomienda incluir repositorios Maven, ya que aumenta los tiempos de compilación.
+ Las pruebas unitarias fallan debido a problemas de sincronización. Esto puede ocurrir cuando las pruebas unitarias son sensibles al tiempo. Un indicador seguro se basa en `.sleep(..)` en el código de prueba.
+ El uso de complementos de Maven no compatibles.

## Escaneado de códigos

![Escaneo de código](./assets/build-and-deployment/code-scanning.png)

El análisis de código realiza un análisis de código estático mediante una combinación de prácticas recomendadas específicas de Java y AEM.

El análisis del código provoca un error de compilación si existen vulnerabilidades de seguridad críticas en el código. Las infracciones menores se pueden anular, pero se recomienda corregirlas. Tenga en cuenta que el análisis de código es imperfecto y puede dar como resultado [falsos positivos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html?lang=es#dealing-with-false-positives).

Para resolver los problemas de digitalización de código, descargue el informe en formato CSV proporcionado por Cloud Manager mediante el botón **Descargar detalles** y revise las entradas.

Para obtener más información, consulte Reglas específicas de AEM, consulte las [reglas de análisis de código personalizadas específicas de AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html?lang=es) de la documentación de Cloud Manager.

## Generar imágenes

![Generar imágenes](./assets/build-and-deployment/build-images.png)

Generar imagen es responsable de combinar los artefactos de código creados en el paso Generar y prueba de unidad con la versión de AEM para formar un único artefacto implementable.

Aunque se encuentran problemas de compilación y compilación de código durante la compilación y prueba de unidad, puede haber problemas de configuración o estructurales identificados al intentar combinar el artefacto de compilación personalizada con la versión de AEM.

### Duplicar configuraciones de OSGi

Cuando varias configuraciones de OSGi se resuelven mediante el modo de ejecución para el entorno de AEM de destino, el paso Generar imagen falla con el siguiente error:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ El paquete todo del proyecto de AEM, contiene varios paquetes de código y la misma configuración de OSGi la proporciona más de uno de los paquetes de código, lo que da como resultado un conflicto, y el paso Generar imagen no puede decidir cuál debe usarse, lo que provoca que se produzca un error en la compilación. Tenga en cuenta que esto no se aplica a las configuraciones de fábrica de OSGi, siempre y cuando tengan nombres únicos.
+ __Resolución:__ Revise todos los paquetes de código (incluidos los paquetes de código de terceros incluidos) que se implementan como parte de la aplicación de AEM en busca de configuraciones de OSGi duplicadas que se resuelvan, a través del modo de ejecución, en el entorno de destino. La guía del mensaje de error &quot;set the mergeConfigurations flag to true&quot; no es posible en AEM as a Cloud Service y debe ignorarse.

#### Causa 2

+ __Causa:__ El del proyecto de AEM incluye incorrectamente el mismo paquete de código dos veces, lo que da como resultado la duplicación de cualquier configuración OSGi contenida en dicho paquete.
+ __Resolución:__ Revise todos los pom.xml de paquetes incrustados en el proyecto all y asegúrese de que tienen `filevault-package-maven-plugin` [configuración](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=es#cloud-manager-target) establecida en `<cloudManagerTarget>none</cloudManagerTarget>`.

### Script de repoinit mal formado

Los scripts de repoinit definen el contenido de línea de base, los usuarios, las ACL, etc. En AEM as a Cloud Service, los scripts de repoinit se aplican durante la compilación de imágenes, pero en el inicio rápido local de AEM SDK se aplican cuando se activa la configuración de fábrica de repoinit OSGi. Debido a esto, los scripts Repoinit pueden fallar silenciosamente (con el registro) en el inicio rápido local de AEM SDK y, sin embargo, hacer que el paso Generar imagen falle, lo que detiene la implementación.

+ __Causa:__ Un script de repoinit tiene un formato incorrecto. Esto puede dejar el repositorio en un estado incompleto, ya que los scripts de repoinit después del script que falla no se ejecutan en el repositorio.
+ __Resolución:__ Revise el inicio rápido local de AEM SDK cuando se implemente la configuración OSGi del script de repoinit para determinar si se producen errores y cuáles se producen.

### Dependencia de contenido de repoinit no satisfecha

Los scripts de repoinit definen el contenido de línea de base, los usuarios, las ACL, etc. En el inicio rápido local de AEM SDK, los scripts repoinit se aplican cuando se activa la configuración de fábrica de repoinit OSGi, o en otras palabras, después de que el repositorio esté activo y pueda haber sufrido cambios de contenido directamente o a través de paquetes de contenido. En AEM as a Cloud Service, los scripts de repoinit se aplican durante la generación de imágenes en un repositorio del que puede no depender el script repoinit.

+ __Causa:__ Un script de repoinit depende de contenido que no existe.
+ __Resolución:__ Asegúrese de que el contenido del script repoinit depende de que exista. A menudo, esto indica un script de repoinit inadecuadamente definido al que le faltan directivas que definen estas estructuras de contenido que faltan, pero que son obligatorias. Esto se puede reproducir localmente eliminando AEM, desempaquetando Jar y agregando la configuración de OSGi de repoinit que contiene el script de repoinit a la carpeta de instalación e iniciando AEM. El error se mostrará en el archivo error.log del inicio rápido local de AEM SDK.


### La versión de los componentes principales de la aplicación es mayor que la versión implementada

_Este problema solo afecta a los entornos que no son de producción y que NO se actualizan automáticamente a la última versión de AEM._

AEM as a Cloud Service incluye automáticamente la última versión de los componentes principales en cada versión de AEM, lo que significa que, después de actualizar un entorno de AEM as a Cloud Service de forma automática o manual, se le implementa la última versión de los componentes principales.

Es posible que el paso Generar imagen falle cuando:

+ La aplicación de implementación actualiza la versión de dependencia Maven de los componentes principales en el proyecto `core` (paquete OSGi)
+ La aplicación de implementación se implementa a continuación en un entorno de AEM as a Cloud Service de zona protegida (que no sea de producción) que no se ha actualizado para utilizar una versión de AEM que contenga esa nueva versión de los componentes principales.

Para evitar este error, siempre que haya una actualización disponible del entorno de AEM as a Cloud Service, incluya la actualización como parte de la siguiente generación o implementación y asegúrese siempre de que las actualizaciones se incluyan después de incrementar la versión de los componentes principales en la base de código de la aplicación.

+ __Síntomas:__
El paso Generar imagen falla con un informe de ERROR que indica que el proyecto `core` no pudo importar los paquetes de `com.adobe.cq.wcm.core.components...` en intervalos de versiones específicos.

  ```
  [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
  [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  ```

+ __Causa:__ El paquete OSGi de la aplicación (definido en el proyecto `core`) importa las clases Java de la dependencia principal de los componentes principales en un nivel de versión diferente al que se implementa en AEM as a Cloud Service.
+ __Resolución:__
   + Con Git, revierta a una confirmación de trabajo que exista antes del incremento de la versión del componente principal. Envíe esta confirmación a una rama de Git de Cloud Manager y realice una actualización del entorno desde esta rama. Esto actualizará AEM as a Cloud Service a la última versión de AEM, que incluirá la versión posterior de los componentes principales. Una vez que AEM as a Cloud Service se actualice a la última versión de AEM, que tendrá la última versión de componentes principales, vuelva a implementar el código que falló originalmente.
   + Para reproducir este problema localmente, asegúrese de que la versión de AEM SDK sea la misma versión de AEM que el entorno de AEM as a Cloud Service.


### Creación de un caso de soporte de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de asistencia de Adobe a través de:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Pestaña Asistencia > Crear caso

  _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe a la que se le ha producido un error en la canalización esté seleccionada en el conmutador Organizaciones de Adobe antes de crear el caso._

## Implementar en

El paso Implementar en es responsable de tomar el artefacto de código generado en Generar imagen, inicia nuevos servicios de AEM Author y Publish mediante él y, una vez finalizado correctamente, elimina cualquier servicio antiguo de AEM Author y Publish. Los paquetes de contenido mutable y los índices también se instalan y actualizan en este paso.

Familiarícese con [los registros de AEM as a Cloud Service](./logs.md) antes de depurar el paso Implementar en. El registro `aemerror` contiene información sobre el inicio y el cierre de los pods que puede ser relevante para la implementación en problemas. Tenga en cuenta que el registro disponible a través del botón Descargar registro en el paso Implementar en de Cloud Manager no es el registro `aemerror` y no contiene información detallada relativa al inicio de las aplicaciones.

![Implementar en](./assets/build-and-deployment/deploy-to.png)

Las tres razones principales por las que el paso Implementar en puede fallar:

### La canalización de Cloud Manager contiene una versión antigua de AEM

+ __Causa:__ Una canalización de Cloud Manager contiene una versión de AEM anterior a la implementada en el entorno de destino. Esto puede ocurrir cuando se reutiliza una canalización y se dirige a un nuevo entorno que ejecuta una versión posterior de AEM. Esto se puede identificar comprobando si la versión de AEM del entorno es mayor que la versión de AEM de la canalización.
  ![La canalización de Cloud Manager contiene una versión antigua de AEM](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Resolución:__
   + Si el entorno de destino tiene una actualización disponible, seleccione Actualizar en las acciones del entorno y, a continuación, vuelva a ejecutar la compilación.
   + Si el entorno de destino no tiene una actualización disponible, significa que está ejecutando la última versión de AEM. Para resolver esto, elimine la canalización y vuelva a crearla.


### Tiempo de espera de Cloud Manager

El código que se ejecuta durante el inicio del servicio de AEM recién implementado tarda tanto que Cloud Manager agota el tiempo de espera antes de que pueda completarse la implementación. En estos casos, la implementación puede tener éxito, incluso aunque se haya notificado el estado Error en Cloud Manager.

+ __Causa:__ El código personalizado puede ejecutar operaciones, como consultas grandes o travesías de contenido, desencadenadas al principio del paquete OSGi o de los ciclos de vida del componente, lo que retrasa significativamente el tiempo de inicio de AEM.
+ __Resolución:__ Revise la implementación para el código que se ejecuta al principio del ciclo de vida del paquete OSGi, y revise los registros de `aemerror` para los servicios de AEM Author y Publish alrededor del momento del error (hora de registro en GMT) como lo muestra Cloud Manager, y busque los mensajes de registro que indiquen cualquier proceso de registro personalizado que se esté ejecutando.

### Código o configuración no compatibles

La mayoría de las infracciones de código y configuración se capturan anteriormente en la compilación, pero es posible que el código personalizado o la configuración sean incompatibles con AEM as a Cloud Service y no se detecten hasta que se ejecuten en el contenedor.

+ __Causa:__ El código personalizado puede invocar operaciones largas, como consultas grandes o travesías de contenido, activadas al principio del paquete OSGi o los ciclos de vida del componente, lo que retrasa significativamente el tiempo de inicio de AEM.
+ __Resolución:__ revise los registros de `aemerror` para los servicios de publicación y creación de AEM alrededor del momento (hora de registro en GMT) del error, tal como lo muestra Cloud Manager.
   1. Revise los registros para ver si hay ERRORES producidos por las clases Java proporcionadas por la aplicación personalizada. Si se encuentra algún problema, resuelva, inserte el código corregido y vuelva a compilar la canalización.
   1. Revise los registros para detectar cualquier ERROR notificado por los aspectos de AEM que está ampliando o con los que está interactuando en la aplicación personalizada, e investigue dichos ERRORES; es posible que estos ERRORES no se atribuyan directamente a las clases de Java. Si se encuentra algún problema, resuelva, inserte el código corregido y vuelva a compilar la canalización.

### Inclusión de /var en el paquete de contenido

`/var` es mutable y contiene una variedad de contenido transitorio en tiempo de ejecución. Incluyendo `/var` en paquetes de contenido (p. ej. `ui.content`) implementado mediante Cloud Manager puede provocar errores en el paso Implementación.

Este problema es difícil de identificar, ya que no provoca un error en la implementación inicial, solo en implementaciones posteriores. Los síntomas notables incluyen:

+ La implementación inicial se realiza correctamente, aunque el contenido mutable nuevo o modificado, que forma parte de la implementación, no parece existir en el servicio de publicación de AEM.
+ La activación o desactivación de contenido en AEM Author está bloqueada
+ Las implementaciones posteriores fallan en el paso Implementar en y el paso Implementar en falla después de aproximadamente 60 minutos.

Para validar este problema, la causa del comportamiento fallido es:

1. Determinando que al menos un paquete de contenido que forma parte de la implementación escribe en `/var`.
1. Compruebe que la cola de distribución principal (en negrita) está bloqueada en:
   + AEM Author > Herramientas > Implementación > Distribución

     ![Cola de distribución bloqueada](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. En las implementaciones posteriores que produzcan errores, descargue los registros &quot;Implementar en&quot; de Cloud Manager con el botón Descargar registro:

   ![Descargar implementación en registros](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... y compruebe que haya aproximadamente 60 minutos entre las instrucciones de registro:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... y ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Tenga en cuenta que este registro no contendrá estos indicadores en las implementaciones iniciales que informan como exitosas, sino solo en implementaciones fallidas posteriores.

+ __Causa:__ El usuario del servicio de replicación de AEM que se utilizó para implementar paquetes de contenido en el servicio de publicación de AEM no puede escribir en `/var` en AEM Publish. Esto provoca que la implementación del paquete de contenido en el servicio de publicación de AEM falle.
+ __Resolución:__ Las siguientes maneras de resolver estos problemas se enumeran en el orden de preferencia:
   1. Si los recursos de `/var` no son necesarios, quite los recursos de `/var` de los paquetes de contenido que se implementan como parte de la aplicación.
   2. Si los recursos de `/var` son necesarios, defina las estructuras de nodos mediante [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=es#repoinit). Los scripts Repoinit se pueden dirigir a AEM Author, AEM Publish o ambos mediante los modos de ejecución OSGi.
   3. Si los recursos de `/var` solo son necesarios para el autor de AEM y no se pueden modelar razonablemente con [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=es#repoinit), muévalos a un paquete de contenido discreto que solo esté instalado en el autor de AEM al [incrustarlo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=es#embeddeds) en el paquete `all` en una carpeta de modo de ejecución de autor de AEM (`<target>/apps/example-packages/content/install.author</target>`).
   4. Proporcione ACL apropiados al usuario de servicio `sling-distribution-importer` tal como se describe en este [Adobe KB](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Creación de un caso de soporte de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de asistencia de Adobe a través de:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Pestaña Asistencia > Crear caso

  _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe a la que se le ha producido un error en la canalización esté seleccionada en el conmutador Organizaciones de Adobe antes de crear el caso._

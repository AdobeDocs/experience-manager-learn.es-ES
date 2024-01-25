---
title: Compilación e implementaciones
description: Adobe AEM Cloud Manager facilita la compilación del código y las implementaciones para que se vean as a Cloud Service. Pueden producirse errores durante los pasos del proceso de generación, que requieren acciones para resolverlos. Esta guía explica cómo comprender los errores comunes de en la implementación y cómo abordarlos mejor.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
duration: 671
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 0%

---

# AEM Depuración de la compilación y las implementaciones as a Cloud Service de

Adobe AEM Cloud Manager facilita la compilación del código y las implementaciones para que se vean as a Cloud Service. Pueden producirse errores durante los pasos del proceso de generación, que requieren acciones para resolverlos. Esta guía explica cómo comprender los errores comunes de en la implementación y cómo abordarlos mejor.

![Canalización de compilación de Cloud Manager](./assets/build-and-deployment/build-pipeline.png)

## Validación

El paso de validación simplemente garantiza que las configuraciones básicas de Cloud Manager sean válidas. Los errores de validación comunes incluyen:

### El entorno está en un estado no válido

+ __Mensaje de error:__ El entorno está en un estado no válido.
  ![El entorno está en un estado no válido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ El entorno de destino de la canalización está en un estado transitorio en el que no puede aceptar nuevas compilaciones.
+ __Resolución:__ Espere a que el estado se resuelva en un estado en ejecución (o actualice el estado disponible). Si se elimina el entorno, vuelva a crearlo o elija un entorno diferente al que compilar.

### No se encuentra el entorno asociado a la canalización

+ __Mensaje de error:__ El entorno se marca como eliminado.
  ![El entorno se marca como eliminado](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ Se ha eliminado el entorno que la canalización está configurada para utilizar.
Incluso si se vuelve a crear un nuevo entorno con el mismo nombre, Cloud Manager no vuelve a asociar automáticamente la canalización a ese entorno con el mismo nombre.
+ __Resolución:__ Edite la configuración de la canalización y vuelva a seleccionar el entorno en el que desea implementar.

### No se encuentra la rama Git asociada a la canalización

+ __Mensaje de error:__ Canalización no válida: XXXXXX. Motivo=No se encuentra Branch=xxxx en el repositorio.
  ![Canalización no válida: XXXXXX. Motivo=No se encuentra Branch=xxxx en el repositorio](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ Se ha eliminado la rama Git que la canalización está configurada para utilizar.
+ __Resolución:__ Vuelva a crear la rama Git que falta con el mismo nombre o vuelva a configurar la canalización para que se genere desde una rama diferente existente.

## Prueba de compilación y unidad

![Prueba de compilación y unidad](./assets/build-and-deployment/build-and-unit-testing.png)

La fase Generar y prueba de unidades realiza una compilación de Maven (`mvn clean package`) del proyecto extraído de la rama Git configurada de la canalización.

Los errores identificados en esta fase deben ser reproducibles generando el proyecto localmente, con las siguientes excepciones:

+ Una dependencia de Maven no está disponible en [Maven Central](https://search.maven.org/) y el repositorio de Maven que contiene la dependencia es:
   + No accesible desde Cloud Manager, como un repositorio Maven interno privado, o el repositorio Maven requiere autenticación y se han proporcionado credenciales incorrectas.
   + No registrado explícitamente en el del proyecto `pom.xml`. Tenga en cuenta que, no se recomienda incluir repositorios Maven, ya que aumenta los tiempos de compilación.
+ Las pruebas unitarias fallan debido a problemas de sincronización. Esto puede ocurrir cuando las pruebas unitarias son sensibles al tiempo. Un indicador potente se basa en `.sleep(..)` en el código de prueba.
+ El uso de complementos de Maven no compatibles.

## Escaneado de códigos

![Escaneado de códigos](./assets/build-and-deployment/code-scanning.png)

AEM La digitalización de código realiza un análisis de código estático mediante una combinación de prácticas recomendadas específicas de Java y de la aplicación de código de la aplicación.

El análisis del código provoca un error de compilación si existen vulnerabilidades de seguridad críticas en el código. Las infracciones menores se pueden anular, pero se recomienda corregirlas. Tenga en cuenta que el escaneo de código es imperfecto y puede dar como resultado [falsos positivos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives).

Para resolver los problemas de análisis de código, descargue el informe en formato CSV proporcionado por Cloud Manager a través de la **Descargar detalles** y revise las entradas.

AEM Para obtener más información, consulte Reglas específicas de la, consulte Documentación de Cloud Manager [AEM reglas de escaneo de código específicas para el cliente](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Generar imágenes

![Generar imágenes](./assets/build-and-deployment/build-images.png)

AEM La creación de imagen es responsable de combinar los artefactos de código creados en el paso Generar y prueba de unidad con la versión de la aplicación para formar un único artefacto que se puede implementar.

AEM Aunque se encuentran problemas de compilación y compilación de código durante las pruebas de compilación y unidad, puede haber problemas de configuración o estructurales identificados al intentar combinar el artefacto de compilación personalizada con la versión de la versión de la.

### Duplicar configuraciones de OSGi

AEM Cuando varias configuraciones de OSGi se resuelven mediante el modo de ejecución para el entorno de segmentación de datos, el paso Generar imagen falla con el siguiente error:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ AEM El paquete todo del proyecto de la aplicación, contiene varios paquetes de código y la misma configuración OSGi la proporciona más de uno de los paquetes de código, lo que da como resultado un conflicto, y el paso Generar imagen no puede decidir cuál debe utilizarse, por lo que la generación no funciona. Tenga en cuenta que esto no se aplica a las configuraciones de fábrica de OSGi, siempre y cuando tengan nombres únicos.
+ __Resolución:__ AEM Revise todos los paquetes de código (incluidos los paquetes de código de terceros incluidos) que se implementan como parte de la aplicación, buscando configuraciones OSGi duplicadas que se resuelvan, a través del modo de ejecución, en el entorno de destino. AEM La guía del mensaje de error &quot;set the mergeConfigurations flag to true&quot; no es posible en as a Cloud Service y debe ignorarse.

#### Causa 2

+ __Causa:__ AEM La inclusión incorrecta del proyecto del mismo paquete de código dos veces, lo que da como resultado la duplicación de cualquier configuración OSGi contenida en dicho paquete.
+ __Resolución:__ Revise todos los archivos pom.xml de los paquetes incrustados en todos los proyectos y asegúrese de que tengan `filevault-package-maven-plugin` [configuración](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) establezca en `<cloudManagerTarget>none</cloudManagerTarget>`.

### Script de repoinit mal formado

Los scripts de repoinit definen el contenido de línea de base, los usuarios, las ACL, etc. AEM En as a Cloud Service AEM, los scripts repoinit se aplican durante la compilación de la imagen, pero en el inicio rápido local del SDK de la aplicación se aplican cuando se activa la configuración de fábrica de repoinit de OSGi. AEM Debido a esto, los scripts de Repoinit pueden fallar silenciosamente (con registro) en el inicio rápido local del SDK de la aplicación, pero hacen que el paso Generar imagen falle, lo que detiene la implementación.

+ __Causa:__ El script de repoinit tiene un formato incorrecto. Esto puede dejar el repositorio en un estado incompleto, ya que los scripts de repoinit después del script que falla no se ejecutan en el repositorio.
+ __Resolución:__ AEM Revise el inicio rápido local del SDK de cuando se implemente la configuración OSGi del script de repoinit para determinar si se producen errores y cuáles.

### Dependencia de contenido de repoinit no satisfecha

Los scripts de repoinit definen el contenido de línea de base, los usuarios, las ACL, etc. AEM En el inicio rápido local del SDK de la, los scripts repoinit se aplican cuando se activa la configuración de fábrica de repoinit OSGi, o en otras palabras, después de que el repositorio esté activo y pueda haber sufrido cambios de contenido directamente o a través de paquetes de contenido. AEM En as a Cloud Service, los scripts de repoinit se aplican durante la compilación de una imagen con un repositorio del que puede no depender el script repoinit.

+ __Causa:__ Un script de repoinit depende del contenido que no existe.
+ __Resolución:__ Asegúrese de que el contenido del script de repoinit exista. A menudo, esto indica un script de repoinit inadecuadamente definido al que le faltan directivas que definen estas estructuras de contenido que faltan, pero que son obligatorias. AEM AEM Esto se puede reproducir localmente eliminando el Jar, desempaquetándolo, agregando la configuración OSGi de repoinit que contiene el script de repoinit a la carpeta de instalación e iniciando el proceso de instalación de la. AEM El error se presentará en el error.log del inicio rápido local del SDK de la.


### La versión de los componentes principales de la aplicación es mayor que la versión implementada

_AEM Este problema solo afecta a los entornos que no son de producción y que NO se actualizan automáticamente a la última versión de la versión de la._

AEM La versión as a Cloud Service AEM AEM incluye automáticamente la última versión de los componentes principales en cada versión de la, lo que significa que, después de actualizar un entorno as a Cloud Service de forma automática o manual, se le implementará la versión más reciente de los componentes principales.

Es posible que el paso Generar imagen falle cuando:

+ La aplicación de implementación actualiza la versión de dependencia Maven de los componentes principales en el `core` Proyecto (paquete OSGi)
+ AEM A continuación, la aplicación de implementación se implementa en un entorno as a Cloud Service AEM de zona protegida (que no sea de producción) que no se ha actualizado para utilizar una versión de que contenga esa nueva versión de los componentes principales.

AEM Para evitar este error, cada vez que haya una actualización disponible del entorno as a Cloud Service de la aplicación, incluya la actualización como parte de la siguiente compilación/implementación y asegúrese siempre de que las actualizaciones se incluyan después de incrementar la versión de los componentes principales en la base de código de la aplicación.

+ __Síntomas:__
El paso Generar imagen falla con un informe de ERROR que `com.adobe.cq.wcm.core.components...` paquetes con intervalos de versiones específicos no se han podido importar mediante la variable `core` proyecto.

  ```
  [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
  [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  ```

+ __Causa:__  El paquete OSGi de la aplicación (definido en la variable `core` AEM proyecto) importa clases Java de la dependencia principal de los componentes principales, en un nivel de versión diferente al que se implementa para los componentes as a Cloud Service de la aplicación de la versión de la aplicación de la aplicación de la versión de la aplicación de la versión de la aplicación de la aplicación de.
+ __Resolución:__
   + Con Git, revierta a una confirmación de trabajo que exista antes del incremento de la versión del componente principal. Envíe esta confirmación a una rama de Git de Cloud Manager y realice una actualización del entorno desde esta rama. AEM Esto actualizará la versión as a Cloud Service AEM a la última versión de la, que incluirá la versión posterior de los componentes principales. AEM Una vez que la versión as a Cloud Service AEM de la se actualice a la última versión de la versión, que tendrá la última versión de los componentes principales, vuelva a implementar el código que falló originalmente.
   + AEM AEM AEM Para reproducir este problema localmente, asegúrese de que la versión del SDK de la sea la misma versión de la versión que utiliza el entorno as a Cloud Service de la.


### Creación de un caso de soporte de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de soporte de Adobe a través de:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Pestaña Asistencia > Crear caso

  _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe que tiene una canalización con errores esté seleccionada en el conmutador Organizaciones de Adobe antes de crear el caso._

## Implementar en

AEM AEM El paso Implementar en es responsable de tomar el artefacto de código generado en Generar imagen, inicia nuevos servicios de autor y publicación que lo utilizan y, una vez finalizado correctamente, elimina cualquier servicio antiguo de autor y publicación de la. Los paquetes de contenido mutable y los índices también se instalan y actualizan en este paso.

Familiarícese con [AEM registros as a Cloud Service](./logs.md) antes de depurar el paso Implementar en. El `aemerror` El registro contiene información sobre el inicio y el cierre de los pods que puede ser pertinente para la implementación de problemas. Tenga en cuenta que el registro disponible a través del botón Descargar registro en el paso Implementar en de Cloud Manager no es el `aemerror` registra y no contiene información detallada sobre el inicio de las aplicaciones.

![Implementar en](./assets/build-and-deployment/deploy-to.png)

Las tres razones principales por las que el paso Implementar en puede fallar:

### AEM La canalización de Cloud Manager contiene una versión antigua de la aplicación

+ __Causa:__ AEM Una canalización de Cloud Manager contiene una versión más antigua de la que se implementa en el entorno de destino. AEM Esto puede ocurrir cuando se reutiliza una canalización y se dirige a un nuevo entorno que ejecuta una versión posterior de la misma AEM AEM Esto se puede identificar comprobando si la versión del entorno en la que se realiza el envío es mayor que la versión de la canalización que se ha creado para el envío de la lista de versiones de la canalización.
  ![AEM La canalización de Cloud Manager contiene una versión antigua de la aplicación](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Resolución:__
   + Si el entorno de destino tiene una actualización disponible, seleccione Actualizar en las acciones del entorno y, a continuación, vuelva a ejecutar la compilación.
   + AEM Si el entorno de destino no tiene una actualización disponible, significa que está ejecutando la última versión de la aplicación de. Para resolver esto, elimine la canalización y vuelva a crearla.


### Cloud Manager agota el tiempo de espera

AEM El código que se ejecuta durante el inicio del servicio de implementación recién implementado tarda tanto tiempo que Cloud Manager agota el tiempo de espera antes de que se complete la implementación. En estos casos, la implementación puede tener éxito, incluso aunque se haya notificado el estado Error en Cloud Manager.

+ __Causa:__ AEM El código personalizado puede ejecutar operaciones, como consultas grandes o travesías de contenido, activadas al principio en los ciclos de vida del paquete OSGi o del componente, lo que retrasa significativamente el tiempo de inicio de la actividad de la.
+ __Resolución:__ Revise la implementación para el código que se ejecuta al principio del ciclo de vida del paquete OSGi y revise la `aemerror` AEM registra los servicios de autor y publicación de la en el momento del error (registrar la hora en GMT), tal como muestra Cloud Manager, y busca los mensajes de registro que indican cualquier registro personalizado que se esté ejecutando durante los procesos.

### Código o configuración no compatibles

AEM La mayoría de las infracciones de código y configuración se capturan anteriormente en la compilación, pero es posible que el código o la configuración personalizados sean incompatibles con el as a Cloud Service de la y pasen desapercibidos hasta que se ejecuten en el contenedor.

+ __Causa:__ AEM El código personalizado puede invocar operaciones largas, como consultas grandes o travesías de contenido, activadas al principio del paquete OSGi o ciclos de vida de componentes que retrasan significativamente el tiempo de inicio de la.
+ __Resolución:__ Revise la `aemerror` AEM registra para los servicios de autor y publicación de alrededor de la hora (hora de registro en GMT) del error, como lo muestra Cloud Manager.
   1. Revise los registros para ver si hay ERRORES producidos por las clases Java proporcionadas por la aplicación personalizada. Si se encuentra algún problema, resuelva, inserte el código corregido y vuelva a compilar la canalización.
   1. AEM Revise los registros para ver si hay ALGÚN ERROR notificado por aspectos de la aplicación personalizada con los que esté ampliando o interactuando e investigue dichos errores; es posible que estos errores no se atribuyan directamente a clases Java. Si se encuentra algún problema, resuelva, inserte el código corregido y vuelva a compilar la canalización.

### Inclusión de /var en el paquete de contenido

`/var` es mutable y contiene una variedad de contenido transitorio en tiempo de ejecución. Incluyendo `/var` en paquetes de contenido (por ejemplo, `ui.content`) implementado mediante Cloud Manager puede provocar errores en el paso de implementación.

Este problema es difícil de identificar, ya que no provoca un error en la implementación inicial, solo en implementaciones posteriores. Los síntomas notables incluyen:

+ AEM La implementación inicial se realiza correctamente; sin embargo, el contenido mutable nuevo o modificado, que forma parte de la implementación, no parece existir en el servicio de publicación de.
+ AEM Se ha bloqueado la activación o desactivación de contenido en el autor de la
+ Las implementaciones posteriores fallan en el paso Implementar en y el paso Implementar en falla después de aproximadamente 60 minutos.

Para validar este problema, la causa del comportamiento fallido es:

1. Al determinar que al menos un paquete de contenido que forma parte de la implementación, escribe en `/var`.
1. Compruebe que la cola de distribución principal (en negrita) está bloqueada en:
   + AEM Autor de la > Herramientas > Implementación > Distribución
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

+ __Causa:__ AEM AEM El usuario del servicio de replicación de datos utilizado para implementar paquetes de contenido en el servicio de publicación de la no puede escribir en `/var` AEM en la publicación de. AEM Esto provoca que la implementación del paquete de contenido en el servicio Publicación de la falle.
+ __Resolución:__ Las siguientes formas de resolver estos problemas se enumeran en orden de preferencia:
   1. Si la variable `/var` Los recursos de no son necesarios para eliminar ningún recurso en `/var` a partir de paquetes de contenido que se implementan como parte de la aplicación.
   2. Si la variable `/var` Si los recursos son necesarios, defina las estructuras de nodos mediante [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). AEM AEM Los scripts Repoinit se pueden dirigir a los modos de ejecución de OSGi para que se dirijan a los modos de ejecución de Autor, Publicación de la o ambos.
   3. Si la variable `/var` AEM los recursos solo son necesarios en el autor de la y no se pueden modelar razonablemente con [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)AEM , muévalos a un paquete de contenido discreto que solo esté instalado en el Autor de la aplicación de manera que se pueda crear un paquete de contenido independiente. Para ello, haga lo siguiente: [incrustación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=es#embeddeds) que en el `all` AEM paquete en una carpeta de modo de ejecución de Autor de (`<target>/apps/example-packages/content/install.author</target>`).
   4. Proporcione ACL adecuados a la `sling-distribution-importer` usuario de servicio como se describe en esta [KB de Adobe](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Creación de un caso de soporte de Adobe

Si los enfoques de solución de problemas anteriores no resuelven el problema, cree un caso de soporte de Adobe a través de:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Pestaña Asistencia > Crear caso

  _Si es miembro de varias organizaciones de Adobe, asegúrese de que la organización de Adobe que tiene una canalización con errores esté seleccionada en el conmutador Organizaciones de Adobe antes de crear el caso._

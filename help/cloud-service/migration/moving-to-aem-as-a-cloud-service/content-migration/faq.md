---
title: Preguntas frecuentes sobre la migración de contenido as a Cloud Service AEM
description: Obtenga respuestas a las preguntas más frecuentes sobre la migración de contenido a AEM as a Cloud Service.
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
source-git-commit: b2656329270ac90458dbc25bb05f39bf76921f26
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 0%

---


# Preguntas frecuentes sobre la migración de contenido as a Cloud Service AEM

Obtenga respuestas a las preguntas más frecuentes sobre la migración de contenido a AEM as a Cloud Service.

## Terminología

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [Analizador de prácticas recomendadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [Herramienta de transferencia de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Sistema Identity Management](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

Utilice la siguiente plantilla para proporcionar más detalles al crear tickets de asistencia de Adobe relacionados con CTT.

![Plantilla de ticket de asistencia del Adobe de migración de contenido](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## Preguntas generales sobre la migración de contenido

### P: ¿Cuáles son los diferentes métodos para migrar contenido a AEM como Cloud Services?

Hay tres métodos diferentes disponibles

+ Uso de la herramienta de transferencia de contenido (AEM 6.3+ → AEMaaCS)
+ A través del administrador de paquetes (AEM → AEMaaCS)
+ Servicio de importación masiva para recursos (S3/Azure → AEMaaCS)

### P: ¿Existe un límite en la cantidad de contenido que se puede transferir mediante CTT?

No. CTT como herramienta podría extraerse de AEM fuente e introducirse en AEMaaCS. Sin embargo, hay límites específicos en la plataforma AEMaaCS que deben tenerse en cuenta antes de la migración.

Para obtener más información, consulte [requisitos previos de migración a la nube](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### P: Tengo el último informe de BPA de mi sistema de origen, ¿qué debo hacer con él?

Exporte el informe como CSV y, a continuación, cárguelo en Cloud Acceleration Manager. [asociado a su organización IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). A continuación, siga el proceso de revisión como [descritos en la fase de preparación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

Revise la evaluación de la complejidad del código y el contenido que proporciona la herramienta y tome nota de los elementos de acción asociados que llevan a la refactorización de código o a la evaluación de migración a la nube.

### P: ¿Se recomienda extraer un autor de origen e introducirlo en AEMaaCS author y publicar?

Siempre se recomienda realizar la extracción y la ingesta 1:1 entre los niveles de autor y publicación. Dicho esto, es aceptable extraer el autor de la producción de origen e ingerirlo en Dev, Stage y Production CS.

### P: ¿Existe alguna forma de estimar el tiempo que se tarda en migrar el contenido de la AEM de origen a AEMaaCS mediante CTT?

Dado que el proceso de migración depende de la anchura de la banda de Internet, la pila asignada para el proceso CTT, la memoria libre disponible y la E/S de disco que son subjetivas a cada sistema de origen, se recomienda ejecutar la Prueba de las migraciones antes y extrapolar esos puntos de datos para obtener estimaciones.

### P: ¿Cómo se ve afectado mi rendimiento de AEM de origen si inicio el proceso de extracción de CTT?

La herramienta CTT se ejecuta en su propio proceso Java™ que toma hasta 4 GB de memoria, que se puede configurar a través de la configuración OSGi. Este número puede cambiar, pero puede mejorar el proceso de Java™ y averiguarlo.

Si AZCopy está instalado y/o la opción de precopia / función de validación habilitada, entonces el proceso AZCopy consume ciclos de CPU.

Además de jvm , la herramienta también utiliza la E/S de disco para almacenar los datos en un espacio temporal transitorio y que se limpiará después del ciclo de extracción. Además de la RAM, la CPU y la E/S de disco, la herramienta CTT también utiliza el ancho de banda de red del sistema de origen para cargar datos en el almacén de Azure Blob.

La cantidad de recursos que toma el proceso de extracción de CTT depende del número de nodos, el número de blobs y su tamaño agregado. Es difícil proporcionar una fórmula y, por lo tanto, se recomienda ejecutar una pequeña prueba de migración para determinar los requisitos de tamaño del servidor de origen.

Si los entornos de clonación se utilizan para la migración, no afectará a la utilización de recursos del servidor de producción en directo, pero tiene sus propios inconvenientes con la sincronización de contenido entre la producción en directo y el clon

### P: En mi sistema de creación de origen, tenemos SSO configurado para que los usuarios se autentiquen en la instancia de autor. ¿Tengo que usar la función de asignación de usuarios de CTT en este caso?

La respuesta corta es &quot;**Sí**&quot;.

Extracción e ingesta de CTT **without** la asignación de usuarios solo migra el contenido, los principios asociados (usuarios, grupos) de la AEM de origen a AEMaaCS. Sin embargo, estos usuarios (identidades) presentes en Adobe IMS necesitan tener acceso (aprovisionado con) a la instancia AEMaaCS para autenticarse correctamente. El trabajo de [herramienta de asignación de usuarios](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) es asignar el usuario AEM local al usuario IMS para que la autenticación y las autorizaciones funcionen juntas.

En este caso, el proveedor de identidad SAML está configurado con Adobe IMS para utilizar Federated / Enterprise ID, en lugar de utilizar directamente AEM mediante el gestor de autenticación.

### P: En mi sistema de creación de origen, tenemos configurada la autenticación básica para que los usuarios se autentiquen en la instancia de Autor con los usuarios AEM locales. ¿Tengo que usar la función de asignación de usuarios de CTT en este caso?

La respuesta corta es &quot;**Sí**&quot;.

La extracción e ingesta de CTT sin asignación de usuarios sí migra el contenido, los principios asociados (usuarios, grupos) de la AEM de origen a AEMaaCS. Sin embargo, estos usuarios (identidades) presentes en Adobe IMS necesitan tener acceso (aprovisionado con) a la instancia AEMaaCS para autenticarse correctamente. El trabajo de [herramienta de asignación de usuarios](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) es asignar el usuario AEM local al usuario IMS para que la autenticación y las autorizaciones funcionen juntas.

En este caso, los usuarios utilizan Adobe ID personal y el administrador de IMS utiliza Adobe ID para proporcionar acceso a AEMaaCS.

### P: ¿Qué significan los términos &quot;borrar&quot; y &quot;sobrescribir&quot; en el contexto de CTT?

En el contexto de [fase de extracción](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html), las opciones son sobrescribir los datos del contenedor de ensayo de ciclos de extracción anteriores o añadir el diferencial (añadido, actualizado o eliminado) a él. El contenedor de ensayo no es nada, pero el contenedor de almacenamiento de blob asociado al conjunto de migración. Cada conjunto de migración obtiene su propio contenedor de ensayo.

En el contexto de [fase de ingesta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html), las opciones son + para reemplazar todo el repositorio de contenido de AEMaaCS o sincronizar el contenido diferencial (añadido, actualizado o eliminado) del contenedor de migración de ensayo.

### P: Hay varios sitios web, recursos asociados, usuarios y grupos en el sistema de origen. ¿Es posible migrarlos por fases a AEMaaCS?

Sí, es posible, pero requiere una planificación cuidadosa con respecto a:

+ Creación de los conjuntos de migración suponiendo que los sitios, los recursos se encuentran en sus respectivas jerarquías
   + Compruebe si es aceptable migrar todos los recursos como parte de un conjunto de migración y, a continuación, traer los sitios que los utilizan por fases
+ En el estado actual, el proceso de ingesta de autor impide que la instancia de autor esté disponible para la creación de contenido, aunque el nivel de publicación pueda servir al contenido
   + Esto significa que, hasta que la ingesta termina en autor, las actividades de creación de contenido se bloquean

Revise el proceso de extracción e ingesta superior como está documentado antes de planificar las migraciones.

### P: ¿Mis sitios web van a estar disponibles para los usuarios finales aunque la ingesta se produzca en instancias de autor o publicación de AEMaaCS?

Sí. El tráfico del usuario final no se ve interrumpido por la actividad de migración de contenido. Sin embargo, la ingesta de autor bloquea la creación de contenido hasta que se completa.

### P: El informe BPA muestra elementos relacionados con la falta de representaciones originales. ¿Debería limpiarlos en el origen antes de la extracción?

Sí. La representación original que falta significa que el binario del recurso no se carga correctamente en primer lugar. Considerándolo datos incorrectos, revise, realice una copia de seguridad utilizando el Administrador de paquetes (según sea necesario) y elimínelos del AEM de origen antes de ejecutar la extracción. Los datos incorrectos tendrán resultados negativos en los pasos de procesamiento de recursos.

### P: El informe BPA tiene elementos relacionados con la falta `jcr:content` para carpetas. ¿Qué debo hacer con ellos?

When `jcr:content` falta en el nivel de carpeta, cualquier acción para propagar la configuración, como el procesamiento de perfiles, etc. de los padres se romperán a este nivel. Revise el motivo de la falta `jcr:content`. Aunque estas carpetas se pueden migrar, tenga en cuenta que degradan la experiencia del usuario y causan ciclos de solución de problemas innecesarios más tarde.

### P: He creado un conjunto de migración. ¿es posible comprobar su tamaño?

Sí, hay un [Comprobar tamaño](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) que forma parte de CTT.

### P: Estoy realizando la migración (extracción, ingesta). ¿Es posible validar que todo mi contenido extraído se incorpora en target?

Sí, hay un [validación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) que forma parte de CTT.

### P: Mi cliente debe mover contenido entre entornos AEMaaCS, como AEMaaCS Dev a AEMaaCS Stage o a AEMaaCS Prod. ¿Puedo utilizar la herramienta de transferencia de contenido para estos casos de uso?

Desafortunadamente, no. El caso de uso de CTT es migrar contenido de fuentes locales/alojadas en AMS AEM 6.3+ a entornos en la nube AEMaaCS. [Lea la documentación de CTT](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### P: ¿Qué tipo de problemas se esperan durante la extracción?

La fase de extracción es un proceso involucrado que requiere que varios aspectos funcionen según lo esperado. Ser conscientes de los diferentes tipos de problemas que pueden producirse y cómo mitigarlos aumenta el éxito general de la migración de contenido.

La documentación pública se mejora continuamente en función de los conocimientos, pero hay algunas categorías de problemas de alto nivel y posibles razones subyacentes.

![AEM problemas de extracción de migración de contenido as a Cloud Service](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### P: ¿Qué tipo de problemas se esperan durante la ingestión?

La fase de ingesta se produce completamente en la plataforma en la nube y requiere ayuda de los recursos que tienen acceso a la infraestructura de AEMaaCS. Cree un ticket de asistencia para obtener más ayuda.

Estas son las posibles categorías de problemas (no lo considere como una lista exclusiva)

![AEM problemas as a Cloud Service de ingesta de migración de contenido](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### P: ¿Es necesario que mi servidor de origen tenga una conexión a Internet de salida para que funcione CTT?

La respuesta corta es &quot;**Sí**&quot;.

El proceso CTT requiere conectividad con los siguientes recursos:

+ El entorno as a Cloud Service AEM destino: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ El servicio de almacenamiento del blob de Azure: `casstorageprod.blob.core.windows.net`
+ El extremo de E/S de asignación de usuario: `usermanagement.adobe.io`

Consulte la documentación para obtener más información sobre [conectividad de origen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## Preguntas relacionadas con el procesamiento de recursos en Dynamic Media

### P: ¿Los recursos se van a volver a procesar automáticamente después de su incorporación en AEMaaCS?

No. Para procesar los recursos, se debe iniciar la solicitud de reprocesamiento.

### P: ¿Los recursos se van a reindexar automáticamente después de su incorporación en AEMaaCS?

Sí. Los recursos se reindexan según las definiciones de índice disponibles en AEMaaCS.

### P: El AEM de origen tiene una integración con Dynamic Media. ¿Hay cosas específicas que se deben tener en cuenta antes de la migración de contenido?

Sí, tenga en cuenta lo siguiente cuando el AEM de origen tenga Dynamic Media Integration.

+ AEMaaCS solo admite el modo Scene7 de Dynamic Media. Si el sistema de origen está en modo híbrido, se requiere la migración DM a los modos Scene7.
+ Si el método es migrar desde instancias de clon de origen, entonces es seguro deshabilitar la integración de DM en clon que se usaría para CTT. Este paso es solo evitar escrituras en DM o evitar la carga en el tráfico de DM.
+ Tenga en cuenta que CTT migra nodos, metadatos de un conjunto de migración de AEM de origen a AEMaaCS. No realizará ninguna operación en DM directamente.

### P: ¿Cuáles son los diferentes enfoques de migración cuando la integración de DM está presente en el AEM de fuentes?

Lea antes la pregunta y la respuesta anteriores

(Estas son dos opciones posibles, pero no se limitan a estas dos). Depende de cómo el cliente desee abordar el UAT, las pruebas de rendimiento, el entorno disponible y si un clon se está utilizando para la migración o no. Consideren estos dos puntos como punto de partida para la discusión

**Opción 1**

Si el número de activos/nodos del entorno de origen está en el extremo inferior (~100K), suponiendo que se puedan migrar durante un período de 24 + 72 horas, incluidas la extracción y la ingesta, el mejor enfoque es

+ Realizar la migración desde la producción directamente
+ Ejecute una extracción e ingesta iniciales en AEMaaCS con `wipe=true`
   + Este paso migra todos los nodos y binarios
+ Continúe trabajando en el autor de productos On-Premise/AMS
+ A partir de ahora, ejecute todas las demás pruebas de ciclos de migración con `wipe=true`
   + Tenga en cuenta que esta operación migra el almacén de nodos completo, pero solo los blobs modificados en lugar de los blobs completos. El conjunto anterior de blobs está presente en el almacén de Azure blob de la instancia de AEMaaCS de destino.
   + Utilice esta prueba de migraciones para medir la duración de la migración, la asignación de usuarios, las pruebas y la validación del resto de funcionalidades.
+ Finalmente, antes de la semana de lanzamiento, realice una migración wipe=true
   + Conecte Dynamic Media en AEMaaCS
   + Desconectar la configuración de DM de AEM fuente local

Con esta opción puede ejecutar la migración de uno a uno, es decir, un desarrollador local → AEMaaCS Dev, etc. y mover las configuraciones de DM desde los entornos respectivos

(En caso de que se planifique realizar la migración desde Clone)

**Opción 2**

+ Crear un clon del creador de producción, eliminar la configuración de DM de Clonar
+ Migración del clon local → AEMaaCS Dev / Stage
   + Conecte brevemente la empresa DM de producción a AEMaaCS Dev/Stage con fines de validación
   + Durante la conexión de DM activa, evite la ingesta de recursos en AEMaaCS
   + Esto les permite validar validaciones específicas de CTT y DM
+ Una vez finalizada la prueba en AEMaaCS
   + Ejecute una migración de borrado desde el escenario local a AEMaaCS Stage

Ejecute una migración de borrado desde el desarrollador local al desarrollador de AEMaaCS.

El enfoque anterior se puede utilizar solo para medir la duración de la migración, pero requiere una limpieza posterior.

## Recursos adicionales

+ [Sugerencias y trucos para migrar a Experience Manager en la nube (Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [Vídeo de la serie de expertos CTT](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [Vídeos de la serie de expertos sobre otros temas de AEMaaCS](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)

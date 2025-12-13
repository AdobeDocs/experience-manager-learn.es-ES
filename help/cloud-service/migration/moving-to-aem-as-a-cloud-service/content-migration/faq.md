---
title: Preguntas frecuentes sobre la migración de contenido de AEM as a Cloud Service
description: Obtenga respuestas a las preguntas frecuentes acerca de la migración de contenido a AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
doc-type: article
topic: Migration
feature: Migration
role: Developer
level: Beginner
jira: KT-11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
duration: 399
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1877'
ht-degree: 0%

---

# Preguntas frecuentes sobre la migración de contenido de AEM as a Cloud Service

Obtenga respuestas a las preguntas frecuentes acerca de la migración de contenido a AEM as a Cloud Service.

## Terminología

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html?lang=es)
+ **BPA**: [Analizador de prácticas recomendadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html?lang=es)
+ **CTT**: [Herramienta de transferencia de contenido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=es)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=es)
+ **IMS**: [Sistema Identity Management](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=es)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html?lang=es)

Utilice la siguiente plantilla para proporcionar más detalles al crear vales de soporte de Adobe relacionados con CTT.

![Plantilla de vale de soporte técnico de Adobe para migración de contenido](../../assets/faq/adobe-support-ticket-template.png) {align="center"}

## Preguntas generales sobre la migración de contenido

### P: ¿Cuáles son los diferentes métodos para migrar contenido a AEM as a Cloud Services?

Hay tres métodos diferentes disponibles

+ Uso de la herramienta de transferencia de contenido (AEM 6.3+ → AEMaaCS)
+ A través del Administrador de paquetes (AEM → AEMaaCS)
+ Servicio de importación por lotes para Assets (S3/Azure → AEMaaCS)

### P: ¿Hay algún límite en la cantidad de contenido que se puede transferir mediante CTT?

No. CTT como herramienta podría extraer de la fuente de AEM e ingerir en AEMaaCS. Sin embargo, existen límites específicos en la plataforma AEMaaCS que deben tenerse en cuenta antes de la migración.

Para obtener más información, consulte [requisitos previos de migración en la nube](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html?lang=es).

### P: Tengo el informe de BPA más reciente de mi sistema de origen, ¿qué debo hacer con él?

Exporte el informe como CSV y luego cárguelo en Cloud Acceleration Manager, [asociado con su organización de IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=es). A continuación, siga con el proceso de revisión como [se describe en la fase de preparación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html?lang=es).

Revise la evaluación de la complejidad del código y el contenido proporcionada por la herramienta y tome nota de los elementos de acción asociados que conducen al registro de refactorización de código pendiente o a la evaluación de la migración a la nube.

### P: ¿Se recomienda extraer en el autor de origen e introducir en el autor y publicación de AEMaaCS?

Siempre se recomienda realizar una extracción y una ingesta de 1:1 entre los niveles de creación y publicación. Dicho esto, es aceptable extraer el autor de producción de origen e ingerirlo en Dev, Stage y Production CS.

### P: ¿Hay alguna manera de estimar el tiempo que se tarda en migrar el contenido de AEM de origen a AEMaaCS mediante CTT?

Dado que el proceso de migración depende del ancho de banda de Internet, la pila asignada para el proceso de CTT, la memoria libre disponible y la E/S de disco que son subjetivas a cada sistema de origen, se recomienda ejecutar Proof Of migraciones desde el principio y extrapolar esos puntos de datos para obtener estimaciones.

### P: ¿Cómo se ve afectado el rendimiento de AEM de origen si inicio un proceso de extracción CTT?

La herramienta CTT se ejecuta en su propio proceso Java™ que ocupa hasta 4 gb de pila, que se puede configurar mediante la configuración OSGi. Este número puede cambiar, pero puede utilizar grep para el proceso de Java™ y averiguarlo.

Si AZCopy está instalado y/o la opción de copia previa / función de validación está activada, el proceso de AZCopy consume ciclos de CPU.

Aparte de jvm , la herramienta también utiliza E/S de disco para almacenar los datos en un espacio temporal de transición y que se limpiará después del ciclo de extracción. Además de la RAM, CPU y la E/S de disco, la herramienta CTT también utiliza el ancho de banda de red del sistema de origen para cargar datos en el almacén de blobs de Azure.

La cantidad de recursos que toma el proceso de extracción de CTT depende del número de nodos, el número de blobs y su tamaño agregado. Es difícil proporcionar una fórmula y, por lo tanto, se recomienda ejecutar una pequeña Prueba de migración para determinar los requisitos de tamaño del servidor de origen.

Si se utilizan entornos clonados para la migración, esto no afectará a la utilización de recursos del servidor de producción en directo, pero tiene sus propios inconvenientes con respecto a la sincronización de contenido entre la producción en directo y el clon

### P: ¿Qué significan los términos &quot;borrar&quot; y &quot;sobrescribir&quot; en el contexto de CTT?

En el contexto de [fase de extracción](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=es#extraction-setup-phase), las opciones son sobrescribir los datos en el contenedor de ensayo de ciclos de extracción anteriores o agregar el diferencial (agregado/actualizado/eliminado) en él. El contenedor de ensayo no es nada, pero el contenedor de almacenamiento del blob asociado al conjunto de migración. Cada conjunto de migración obtiene su propio contenedor de ensayo.

En el contexto de [fase de ingesta](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html?lang=es), las opciones son + para reemplazar todo el repositorio de contenido de AEMaaCS o sincronizar el contenido diferencial (agregado/actualizado/eliminado) del contenedor de migración de ensayo.

### P: Hay varios sitios web, recursos asociados, usuarios y grupos en el sistema de origen. ¿Es posible migrarlos por fases a AEMaaCS?

Sí, es posible, pero requiere una planificación cuidadosa con respecto a:

+ Creación de los conjuntos de migración suponiendo que los sitios, los recursos se almacenan en sus respectivas jerarquías
   + Compruebe si es aceptable migrar todos los recursos como parte de un conjunto de migración y, a continuación, traer los sitios que los utilizan por fases
+ En el estado actual, el proceso de inserción de autores hace que la instancia de creación no esté disponible para la creación de contenido aunque el nivel de publicación pueda seguir sirviendo el contenido
   + Esto significa que hasta que la ingesta se complete en Author, las actividades de creación de contenido se congelan
+ Los usuarios ya no se migran, aunque los grupos sí.

Revise el proceso de extracción e ingesta superior tal y como está documentado antes de planificar las migraciones.

### P: ¿Mis sitios web van a estar disponibles para los usuarios finales aunque la ingesta se esté produciendo en instancias de autor o publicación de AEMaaCS?

Sí. La actividad de migración de contenido no interrumpe el tráfico del usuario final. Sin embargo, la ingesta de autores detiene la creación de contenido hasta que se completa.

### P: El informe de BPA muestra los elementos relacionados con las representaciones originales que faltan. ¿Debo limpiarlos en la fuente antes de la extracción?

Sí. La representación original que falta significa que el binario de recursos no se cargó correctamente en primer lugar. Si los considera datos incorrectos, revise, realice una copia de seguridad con el Administrador de paquetes (según sea necesario) y elimínelos del AEM de origen antes de ejecutar la extracción. Los datos incorrectos tendrán resultados negativos en los pasos de procesamiento de recursos.

### P: El informe de BPA tiene elementos relacionados con el nodo `jcr:content` que falta para las carpetas. ¿Qué debo hacer con ellos?

Cuando falta `jcr:content` en el nivel de carpeta, cualquier acción para propagar la configuración, como perfiles de procesamiento, etc. de los padres se romperán en este nivel. Revise el motivo por el que falta `jcr:content`. Aunque estas carpetas podrían migrarse, tenga en cuenta que degradan la experiencia del usuario y causan ciclos de solución de problemas innecesarios más adelante.

### P: He creado un conjunto de migración. ¿es posible comprobar el tamaño del mismo?

Sí, hay una característica [Comprobar tamaño](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=es#migration-set-size) que forma parte del CTT.

### P: Estoy realizando la migración (extracción, ingesta). ¿Es posible validar que todo el contenido extraído se incorpora en Target?

Sí, hay una característica [validation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html?lang=es) que forma parte de CTT.

### P: Mi cliente tiene un requisito para mover contenido entre entornos de AEMaaCS, como desde Desarrollo de AEMaaCS a Fase de AEMaaCS o a Producto de AEMaaCS. ¿Puedo utilizar la herramienta de transferencia de contenido para estos casos de uso?

Desafortunadamente, no. El caso de uso de CTT es migrar contenido de la fuente de AEM 6.3+ local/alojada en AMS a entornos de nube AEMaaCS. [Lea la documentación de CTT](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=es).

### P: ¿Qué tipo de problemas se esperan durante la extracción?

La fase de extracción es un proceso involucrado que requiere que varios aspectos funcionen según lo esperado. Conocer los diferentes tipos de problemas que pueden producirse y cómo mitigarlos aumenta el éxito general de la migración de contenido.

La documentación pública se mejora continuamente en función de las lecciones aprendidas, pero hay algunas categorías de problemas de alto nivel y posibles razones subyacentes.

![Problemas de extracción de migración de contenido de AEM as a Cloud Service](../../assets/faq/extraction-issues.jpg) {align="center"}

### P: ¿Qué tipo de problemas se esperan durante la ingestión?

La fase de ingesta se produce completamente en la plataforma de la nube y requiere la ayuda de los recursos que tienen acceso a la infraestructura de AEMaaCS. Cree un ticket de asistencia para obtener más ayuda.

Estas son las posibles categorías de problemas (no las considere una lista exclusiva)

![Problemas de ingesta de migración de contenido de AEM as a Cloud Service](../../assets/faq/ingestion-issues.jpg) {align="center"}



### P: ¿Mi servidor de origen necesita tener conexión saliente a Internet para que funcione CTT?

La respuesta corta es &quot;**Sí**&quot;.

El proceso de CTT requiere conectividad con los siguientes recursos:

+ El entorno de AEM as a Cloud Service de destino: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ El servicio de almacenamiento del blob de Azure: `casstorageprod.blob.core.windows.net`

Consulte la documentación para obtener más información sobre [conectividad de origen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=es#source-environment-connectivity).

## Preguntas relacionadas con el procesamiento de recursos en Dynamic Media

### P: ¿Los recursos se van a volver a procesar automáticamente después de su ingesta en AEMaaCS?

No. Para procesar los recursos, se debe iniciar la solicitud para volver a procesar.

### P: ¿Los recursos se van a reindexar automáticamente después de la ingesta en AEMaaCS?

Sí. Los recursos se reindexan según las definiciones de índice disponibles en AEMaaCS.

### P: AEM de origen está integrado con Dynamic Media. ¿Hay alguna cosa específica que deba tenerse en cuenta antes de la migración de contenido?

Sí, tenga en cuenta lo siguiente cuando el AEM de origen tenga integración con Dynamic Media.

+ AEMaaCS solo admite el modo Scene7 de Dynamic Media. Si el sistema de origen está en modo híbrido, se requiere la migración de DM a modos de Scene7.
+ Si el método consiste en migrar desde instancias de clonación de origen, es seguro deshabilitar la integración de DM en el clon que se utilizaría para CTT. Este paso es puramente para evitar cualquier escritura en DM o evitar la carga en el tráfico DM.
+ Tenga en cuenta que CTT migra nodos y metadatos de un conjunto de migración de AEM de origen a AEMaaCS. No realizará ninguna operación directamente en DM.

### P: ¿Cuáles son los diferentes enfoques de migración cuando la integración de DM está presente en Source AEM?

Lea la pregunta y respuesta anteriores

(Estas son dos opciones posibles, pero no se limitan solo a estas dos). Depende de cómo desee el cliente abordar el UAT, las pruebas de rendimiento, el entorno disponible y si un clon se está utilizando para la migración o no. Por favor, considere estos dos puntos de partida para el debate

**Opción 1**

Si el número de activos/nodos en el entorno de origen está en el extremo inferior (~100 000), suponiendo que se puedan migrar en un periodo de 24 + 72 horas, incluidas la extracción y la ingesta, el mejor enfoque es

+ Realizar migración desde producción directamente
+ Ejecutar una extracción e ingesta inicial en AEMaaCS con `wipe=true`
   + Este paso migra todos los nodos y binarios
+ Continúe trabajando en el creador de producción local/de AMS
+ A partir de ahora, ejecute todas las demás pruebas de ciclos de migración con `wipe=true`
   + Tenga en cuenta que esta operación migra el almacén de nodos completos, pero solo los blobs modificados en lugar de los blobs completos. El conjunto anterior de blobs está en el almacén de blobs de Azure de la instancia de AEMaaCS de destino.
   + Utilice esta prueba de migraciones para medir la duración, la prueba y la validación de la migración del resto de funcionalidades
+ Finalmente, antes de la semana de go-live, realice una migración wipe=true
   + Conexión de Dynamic Media en AEMaaCS
   + Desconectar la configuración de DM de la fuente local de AEM

Con esta opción puede ejecutar la migración de uno a uno, lo que significa Desarrollo local → Desarrollo de AEMaaCS, etc. y mueva las configuraciones de DM de los entornos respectivos

(En caso de que la migración se planee realizar desde Clonar)

**Opción 2**

+ Crear un clon del autor de la producción y eliminar la configuración de DM del clon
+ Migración del clon local → AEMaaCS Dev/Stage
   + Conecte brevemente la empresa de DM de producción a AEMaaCS Dev/Stage con fines de validación
   + Durante la conexión de DM activa, evite la ingesta de recursos en AEMaaCS
   + Esto les permite validar validaciones específicas de CTT y DM
+ Una vez finalizada la prueba en AEMaaCS
   + Ejecute una migración de borrado del escenario local al escenario de AEMaaCS

Ejecute una migración de borrado del desarrollo local al desarrollo de AEMaaCS.

El método anterior puede utilizarse únicamente para medir la duración de la migración, pero requiere limpiarla más adelante.

## Recursos adicionales

+ [Sugerencias y trucos para migrar a Experience Manager en la nube ( Cumbre 2022)](https://business.adobe.com/es/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [Vídeo de la serie Expert de CTT](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html?lang=es)

+ [Vídeos de series de expertos sobre otros temas de AEMaaCS](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html?lang=es)

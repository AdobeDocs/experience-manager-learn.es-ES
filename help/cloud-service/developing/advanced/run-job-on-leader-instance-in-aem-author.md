---
title: Cómo ejecutar un trabajo en una instancia de líder en AEM as a Cloud Service
description: Obtenga información sobre cómo ejecutar un trabajo en la instancia de líder en AEM as a Cloud Service.
version: Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
source-git-commit: 7dca86137d476418c39af62c3c7fa612635c0583
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---


# Cómo ejecutar un trabajo en una instancia de líder en AEM as a Cloud Service

AEM Obtenga información sobre cómo ejecutar un trabajo en la instancia principal del servicio de creación de como parte de AEM as a Cloud Service y comprenda cómo configurarlo para que se ejecute una sola vez.

Los trabajos de Sling son tareas asincrónicas que funcionan en segundo plano, diseñadas para controlar eventos activados por el sistema o el usuario. De forma predeterminada, estos trabajos se distribuyen de forma equitativa en todas las instancias (pods) del clúster.

Para obtener más información, consulte [Eventos de Apache Sling y administración de trabajos](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html).

## Creación y procesamiento de trabajos

Para fines de demostración, vamos a crear un _trabajo sencillo que indique al procesador del trabajo que registre un mensaje_.

### Creación de un trabajo

Utilice el siguiente código para _crear_ un trabajo de Apache Sling:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

Los puntos clave que se deben tener en cuenta en el código anterior son:

- La carga del trabajo tiene dos propiedades: `action` y `message`.
- Con el método `addJob(...)` de [JobManager](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html), el trabajo se agrega al tema `wknd/simple/job/topic`.

### Procesar un trabajo

Utilice el siguiente código para _procesar_ el trabajo de Apache Sling anterior:

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

Los puntos clave que se deben tener en cuenta en el código anterior son:

- La clase `SimpleJobConsumerImpl` implementa la interfaz `JobConsumer`.
- Es un servicio registrado para consumir trabajos del tema `wknd/simple/job/topic`.
- El método `process(...)` procesa el trabajo registrando la propiedad `message` de la carga del trabajo.

### Procesamiento de trabajo predeterminado

Al implementar el código anterior en un entorno de AEM as a Cloud Service AEM AEM AEM y ejecutarlo en el servicio de autor de la, que funciona como un clúster con varias JVM de autor de la, el trabajo se ejecutará una vez en cada instancia de autor de la instancia (pod), lo que significa que el número de trabajos creados coincidirá con el número de pods. El número de pods siempre será mayor que uno (para entornos no RDE), pero fluctuará según la administración de recursos interna de AEM as a Cloud Service.

AEM AEM El trabajo se ejecuta en cada instancia de autor (pod) de la porque `wknd/simple/job/topic` está asociado con la cola principal de la misma, que distribuye los trabajos entre todas las instancias disponibles.

Esto suele resultar problemático si el trabajo es responsable de cambiar el estado, como crear o actualizar recursos o servicios externos.

AEM Si desea que el trabajo se ejecute una sola vez en el servicio de creación de la, agregue la [configuración de cola de trabajos](#how-to-run-a-job-on-the-leader-instance) que se describe a continuación.

AEM Puede verificarlo si revisa los registros del servicio de autor de la en [Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager).

![Trabajo procesado por todas las instancias](./assets/run-job-once/job-processed-by-all-instances.png)


Debería ver lo siguiente:

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

AEM Hay dos entradas de registro, una para cada instancia de autor de la (`68775db964-nxxcx` y `68775db964-r4zk7`), que indican que cada instancia (pod) ha procesado el trabajo.

## Cómo ejecutar un trabajo en la instancia de líder

AEM Para ejecutar un trabajo _solo una vez_ en el servicio de autor de la, cree una nueva cola de trabajos de Sling del tipo **Ordenado** y asocie el tema del trabajo (`wknd/simple/job/topic`) a esta cola. AEM Con esta configuración, solo se permitirá que la instancia de autor (pod) de la versión líder de la aplicación procese el trabajo.

AEM En el módulo `ui.config` de su proyecto de, cree un archivo de configuración OSGi (`org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json`) y guárdelo en la carpeta `ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author`.

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

Los puntos clave que se deben tener en cuenta en la configuración anterior son:

- El tema de la cola está establecido en `wknd/simple/job/topic`.
- El tipo de cola está establecido en `ORDERED`.
- El número máximo de trabajos paralelos se ha establecido en `1`.

AEM Una vez implementada la configuración anterior, el trabajo lo procesa exclusivamente la instancia principal, lo que garantiza que se ejecute una sola vez en todo el servicio de autor de la.

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

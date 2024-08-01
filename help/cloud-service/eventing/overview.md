---
title: AEM Eventing
description: AEM Obtenga información acerca de la visualización de eventos, qué es, por qué y cuándo utilizarla, y ejemplos de ella.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 540
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
exl-id: 142ed6ae-1659-4849-80a3-50132b2f1a86
source-git-commit: efa0a16649c41fab8309786a766483cfeab98867
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 0%

---

# AEM Eventing

AEM Obtenga información acerca de la visualización de eventos, qué es, por qué y cuándo utilizarla, y ejemplos de ella.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

## Qué es

AEM AEM El evento de eventos de es un sistema nativo de la nube que permite a las suscripciones a Eventos de la de eventos procesarse en sistemas externos. AEM AEM Un Evento es una notificación de cambio de estado que envía el usuario cada vez que se produce una acción específica. Por ejemplo, esto puede incluir eventos cuando se crea, actualiza o elimina un fragmento de contenido.

AEM ![Ventilación de eventos de la](./assets/aem-eventing.png)

El diagrama anterior visualizaba cómo AEM as a Cloud Service produce eventos y los envía a los Eventos de Adobe I/O, lo que a su vez los expone a los suscriptores de eventos.

En resumen, hay tres componentes principales:

1. **Proveedor de eventos:** AEM as a Cloud Service.
1. **Eventos de Adobe I/O:** Plataforma de desarrollador para integrar, ampliar y crear aplicaciones y experiencias basadas en los productos y tecnologías de Adobe.
1. AEM **Consumidor de eventos:** sistemas propiedad del cliente que se suscribe a los eventos de la. Por ejemplo, un CRM (Administración de la relación con los clientes), PIM (Administración de la información del producto), OMS (Sistema de Order Management) o una aplicación personalizada.

### ¿En qué se diferencia?

Los eventos de [Apache Sling](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), los eventos OSGi y la [observación JCR](https://jackrabbit.apache.org/oak/docs/features/observation.html) ofrecen mecanismos para suscribirse y procesar eventos. AEM Sin embargo, son diferentes de la ventilación de la, tal como se describe en esta documentación.

AEM Entre las principales distinciones de la ventilación de la se incluyen:

- AEM AEM El código de consumidor de evento se ejecuta fuera de la, no en la misma JVM que se ejecuta en la misma interfaz de usuario de JVM que la de la instancia de usuario.
- AEM El código de producto de es responsable de definir los eventos y enviarlos a los Eventos de Adobe I/O.
- La información del evento está estandarizada y se envía en formato JSON. Para obtener más información, consulte [cloudevents](https://cloudevents.io/).
- AEM Para volver a comunicarse con el cliente de eventos, el consumidor de eventos utiliza la API de AEM as a Cloud Service.


## Por qué y cuándo usarlo

AEM La ventilación de los sistemas ofrece numerosas ventajas para la arquitectura del sistema y la eficacia operativa. AEM Las razones principales para utilizar la ventilación de la son:

- **Para crear arquitecturas impulsadas por eventos**: Facilita la creación de sistemas de correspondencia imprecisa que pueden escalarse de forma independiente y son resistentes a los errores.
- AEM **Código bajo y menores costos de operación**: evita personalizaciones en los equipos, lo que conduce a sistemas que son más fáciles de mantener y ampliar, reduciendo así los gastos de operación.
- AEM **Simplifica la comunicación entre sistemas de punto a punto y externos**: elimina las conexiones punto a punto al permitir que los Eventos de Adobe I/O AEM administren las comunicaciones, como determinar qué eventos de deben entregarse a sistemas o servicios específicos.
- **Mayor durabilidad de los eventos**: Eventos de Adobe I/O es un sistema de alta disponibilidad y escalable, diseñado para manejar grandes volúmenes de eventos y entregarlos de manera confiable a los suscriptores.
- **Procesamiento paralelo de eventos**: habilita la entrega de eventos a varios suscriptores simultáneamente, lo que permite el procesamiento de eventos distribuido en varios sistemas.
- **Desarrollo de aplicaciones sin servidor**: admite la implementación del código de consumidor de eventos como una aplicación sin servidor, lo que mejora aún más la flexibilidad y escalabilidad del sistema.

### Limitaciones

AEM La evasión de eventos, aunque es potente, tiene ciertas limitaciones que se deben tener en cuenta:

- **Disponibilidad restringida a AEM as a Cloud Service AEM**: Actualmente, el servicio de ventilación de la está disponible exclusivamente para AEM as a Cloud Service.
- AEM **Compatibilidad con eventos limitados**: Por ahora, solo se admiten eventos de fragmento de contenido de la lista de distribución de contenido de la lista de distribución de contenido en la red. Sin embargo, se espera que el ámbito se amplíe con la adición de más eventos en el futuro.

## Cómo habilitar

AEM La ventilación de la está habilitada para cada entorno de AEM as a Cloud Service y solo está disponible para los entornos en modo previo al lanzamiento. AEM AEM AEM Póngase en contacto con el <a href="mailto:grp-aem-events@adobe.com">equipo de eventos-eventos</a> para habilitar su entorno de con la ventilación de eventos-eventos-eventos-eventos-eventos-eventos.

AEM Si ya está habilitado, consulte [Habilitar eventos de en su entorno de AEM Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) para ver los pasos siguientes.

## Cómo suscribirse

AEM AEM Para suscribirte a eventos de, no tienes que escribir ningún código en el código, sino que se configura un proyecto [Adobe Developer Console](https://developer.adobe.com/). Adobe Developer Console es una puerta de enlace a las API de Adobe, los SDK, los eventos, el tiempo de ejecución y App Builder.

En este caso, un _proyecto_ en Adobe Developer Console le permite suscribirse a los eventos emitidos desde el entorno de AEM as a Cloud Service y configurar la entrega de eventos a sistemas externos.

AEM Para obtener más información, consulte [Cómo suscribirse a eventos de la en Adobe Developer Console](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Cómo consumir

AEM Existen dos métodos principales para consumir eventos de: los métodos _push_ y _pull_.

- **Método push**: en este enfoque, los eventos de Adobe I/O notifican al consumidor de eventos de forma proactiva cuando hay un evento disponible. Las opciones de integración incluyen Webhooks, Adobe I/O Runtime y Amazon EventBridge.
- **Método de extracción**: en este caso, el consumidor de eventos sondea activamente los eventos de Adobe I/O para buscar nuevos eventos. La opción de integración principal para este método es la API de diario de Adobe Developer.

AEM Para obtener más información, consulte [Procesamiento de eventos mediante eventos de Adobe I/O](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Ejemplos

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="AEM Recibir eventos en un webhook" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div>AEM <strong><a href="./examples/webhook.md">Recibir eventos en un enlace web</a></strong></div>
        <p>
          Utilice el webhook proporcionado por el Adobe AEM para recibir los eventos de la y revisar los detalles del evento.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="AEM Cargar diario de eventos" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div>AEM <strong><a href="./examples/journaling.md">Cargar diario de eventos de</a></strong></div>
        <p>
          Utilice la aplicación web proporcionada por el Adobe AEM para cargar los eventos de la revista y revisar los detalles del evento.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="AEM Recibir eventos en acción de Adobe I/O Runtime" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div>AEM <strong><a href="./examples/runtime-action.md">Recibir eventos en la acción de Adobe I/O Runtime</a></strong></div>
        <p>
          AEM Reciba eventos de la y revise los detalles del evento.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="AEM Procesamiento de eventos de mediante la acción de Adobe I/O Runtime" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div>AEM <strong><a href="./examples/event-processing-using-runtime-action.md">Procesamiento de eventos de mediante la acción de Adobe I/O Runtime</a></strong></div>
        <p>
          AEM Obtenga información sobre cómo procesar eventos de recibidos mediante la acción de Adobe I/O Runtime. AEM SPA El procesamiento de eventos incluye llamadas de retorno de eventos, persistencia de datos de eventos y mostrarlos en la interfaz de usuario de.
        </p>
      </td>
  </tr>    
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="Eventos de AEM Assets para la integración con PIM" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div><strong><a href="./examples/assets-pim-integration.md">Eventos de AEM Assets para la integración de PIM</a></strong></div>
        <p>
          Aprenda a integrar los sistemas de AEM Assets y de gestión de la información del producto (PIM) para las actualizaciones de metadatos.
        </p>
      </td>
  </tr>  
</table>

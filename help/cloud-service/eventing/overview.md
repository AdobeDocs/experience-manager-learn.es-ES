---
title: Eventos de AEM
description: Obtenga información sobre los eventos de AEM, qué es, por qué y cuándo utilizarlos, y ejemplos.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# Eventos de AEM

Obtenga información sobre los eventos de AEM, qué es, por qué y cuándo utilizarlos, y ejemplos.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

## Qué es

AEM Event es un sistema de eventos nativo en la nube que permite las suscripciones a AEM Events para su procesamiento en sistemas externos. Un evento de AEM es una notificación de cambio de estado que AEM envía cada vez que se produce una acción específica. Por ejemplo, esto puede incluir eventos cuando se crea, actualiza o elimina un fragmento de contenido.

![Eventos de AEM](./assets/aem-eventing.png)

El diagrama anterior visualizaba cómo AEM as a Cloud Service produce eventos y los envía a Adobe I/O Events, que a su vez los expone a los suscriptores de eventos.

En resumen, hay tres componentes principales:

1. **Proveedor de eventos:** AEM as a Cloud Service.
1. **Adobe I/O Events:** plataforma para desarrolladores para integrar, ampliar y crear aplicaciones y experiencias basadas en los productos y tecnologías de Adobe.
1. **Consumidor de eventos:** sistemas propiedad del cliente que se suscribe a los eventos de AEM. Por ejemplo, un CRM (Administración de la relación con los clientes), PIM (Administración de la información del producto), OMS (Sistema de Order Management) o una aplicación personalizada.

### ¿En qué se diferencia?

Los eventos de [Apache Sling](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), los eventos OSGi y la [observación JCR](https://jackrabbit.apache.org/oak/docs/features/observation.html) ofrecen mecanismos para suscribirse y procesar eventos. Sin embargo, son diferentes de los Eventos de AEM, tal como se describe en esta documentación.

Las principales distinciones de los eventos de AEM incluyen:

- El código de consumidor de evento se ejecuta fuera de AEM y no en la misma JVM que AEM.
- El código de producto de AEM es responsable de definir los eventos y enviarlos a Adobe I/O Events.
- La información del evento está estandarizada y se envía en formato JSON. Para obtener más información, consulte [cloudevents](https://cloudevents.io/).
- Para volver a comunicarse con AEM, el consumidor de eventos utiliza la API de AEM as a Cloud Service.


## Por qué y cuándo usarlo

AEM Eventing ofrece numerosas ventajas para la arquitectura del sistema y la eficacia operativa. Las razones principales para utilizar AEM Eventing son:

- **Para crear arquitecturas impulsadas por eventos**: Facilita la creación de sistemas de correspondencia imprecisa que pueden escalarse de forma independiente y son resistentes a los errores.
- **Código bajo y menores costos operacionales**: evita las personalizaciones en AEM, lo que hace que los sistemas sean más fáciles de mantener y ampliar, reduciendo así los gastos operacionales.
- **Simplifica la comunicación entre AEM y sistemas externos**: elimina las conexiones punto a punto al permitir que Adobe I/O Events administre las comunicaciones, como la determinación de qué eventos de AEM deben entregarse a sistemas o servicios específicos.
- **Mayor durabilidad de los eventos**: Adobe I/O Events es un sistema escalable y de alta disponibilidad diseñado para gestionar grandes volúmenes de eventos y enviarlos de forma fiable a los suscriptores.
- **Procesamiento paralelo de eventos**: habilita la entrega de eventos a varios suscriptores simultáneamente, lo que permite el procesamiento de eventos distribuido en varios sistemas.
- **Desarrollo de aplicaciones sin servidor**: admite la implementación del código de consumidor de eventos como una aplicación sin servidor, lo que mejora aún más la flexibilidad y escalabilidad del sistema.

### Limitaciones

Los eventos de AEM, aunque son potentes, tienen ciertas limitaciones que se deben tener en cuenta:

- **Disponibilidad restringida a AEM as a Cloud Service**: Actualmente, AEM Eventing está disponible exclusivamente para AEM as a Cloud Service.

- **Tipos de eventos disponibles**: revise la lista actual de tipos de eventos disponibles [aquí](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#available-event-types).

## Cómo habilitar

Consulte [Habilitar eventos de AEM en su entorno de AEM Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) para ver los pasos siguientes.

## Cómo suscribirse

Para suscribirte a los eventos de AEM, no tienes que escribir ningún código en AEM, sino que se ha configurado un proyecto [Adobe Developer Console](https://developer.adobe.com/). Adobe Developer Console es una puerta de enlace a las API de Adobe, los SDK, los eventos, el tiempo de ejecución y App Builder.

En este caso, un _proyecto_ en Adobe Developer Console le permite suscribirse a los eventos emitidos desde el entorno de AEM as a Cloud Service y configurar la entrega de eventos a sistemas externos.

Para obtener más información, consulte [Suscripción a eventos de AEM en Adobe Developer Console](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Cómo consumir

Existen dos métodos principales para consumir eventos de AEM: el método _push_ y el método _pull_.

- **Método push**: con este método, Adobe I/O Events notifica al consumidor de eventos de forma proactiva cuando hay un evento disponible. Las opciones de integración incluyen Webhooks, Adobe I/O Runtime y Amazon EventBridge.
- **Método de extracción**: en este caso, el consumidor de eventos sondea Adobe I/O Events de forma activa para buscar nuevos eventos. La opción de integración principal para este método es la API de diario de Adobe Developer.

Para obtener más información, consulte [Procesamiento de eventos de AEM mediante Adobe I/O Events](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Ejemplos

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="Recibir eventos de AEM en un gancho web" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">Recibir eventos de AEM en un gancho web</a></strong></div>
        <p>
          Utilice el webhook proporcionado por Adobe para recibir eventos de AEM y revisar los detalles del evento.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="Cargar diario de eventos de AEM" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">Cargar diario de eventos de AEM</a></strong></div>
        <p>
          Utilice la aplicación web proporcionada por Adobe para cargar eventos de AEM desde el historial y revisar los detalles del evento.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="Recibir eventos de AEM sobre la acción de Adobe I/O Runtime" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">Recibir eventos de AEM en la acción de Adobe I/O Runtime</a></strong></div>
        <p>
          Reciba eventos de AEM y revise los detalles del evento.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="Procesamiento de eventos de AEM mediante la acción de Adobe I/O Runtime" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div><strong><a href="./examples/event-processing-using-runtime-action.md">Procesamiento de eventos de AEM mediante la acción de Adobe I/O Runtime</a></strong></div>
        <p>
          Obtenga información sobre cómo procesar los eventos de AEM recibidos mediante la acción de Adobe I/O Runtime. El procesamiento de eventos incluye la llamada de retorno de AEM, la persistencia de datos de evento y su visualización en la SPA.
        </p>
      </td>
  </tr>
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="Eventos de AEM Assets para la integración con PIM" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div><strong><a href="./examples/assets-pim-integration.md">Eventos de AEM Assets para la integración de PIM</a></strong></div>
        <p>
          Aprenda a integrar AEM Assets y sistemas de gestión de la información de productos (PIM) para actualizar los metadatos.
        </p>
      </td>
  </tr> 
</table>

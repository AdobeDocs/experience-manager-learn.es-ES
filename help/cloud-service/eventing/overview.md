---
title: AEM Eventing
description: AEM Obtenga información acerca de la visualización de eventos, qué es, por qué y cuándo utilizarla, y ejemplos de ella.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
source-git-commit: 55f5cef46f7451ebb5b42b8cf17e71efeb0329c2
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 0%

---


# AEM Eventing

AEM Obtenga información acerca de la visualización de eventos, qué es, por qué y cuándo utilizarla, y ejemplos de ella.

>[!IMPORTANT]
>
>AEM El evento as a Cloud Service solo está disponible para usuarios registrados en el modo previo al lanzamiento. AEM AEM Para habilitar la celebración de eventos en su entorno as a Cloud Service de la, póngase en contacto con [AEM Equipo de eventos de la](mailto:grp-aem-events@adobe.com).

## Qué es

AEM AEM El evento de eventos de es un sistema nativo de la nube que permite a las suscripciones a Eventos de la de eventos procesarse en sistemas externos. AEM AEM Un Evento es una notificación de cambio de estado que envía el usuario cada vez que se produce una acción específica. Por ejemplo, esto puede incluir eventos cuando se crea, actualiza o elimina un fragmento de contenido.

![AEM Eventing](./assets/aem-eventing.png)

AEM El diagrama anterior visualizaba cómo los eventos as a Cloud Service producen eventos y los envían a los Eventos de Adobe I/O, lo que a su vez los expone a los suscriptores de eventos.

En resumen, hay tres componentes principales:

1. **Proveedor de eventos:** AEM as a Cloud Service.
1. **Eventos de Adobe I/O:** Plataforma de desarrollador para integrar, ampliar y crear aplicaciones y experiencias basadas en los productos y tecnologías de Adobe.
1. **Consumidor de eventos:** AEM Sistemas propiedad del cliente que se suscriben a los eventos de la. Por ejemplo, un CRM (Customer Relationship Management), PIM (Product Information Management), OMS (Order Management System) o una aplicación personalizada.

### ¿En qué se diferencia?

El [Eventos de Apache Sling](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), eventos OSGi y [Observación JCR](https://jackrabbit.apache.org/oak/docs/features/observation.html) todos los mecanismos de oferta para suscribirse y procesar eventos. AEM Sin embargo, son diferentes de la ventilación de la, tal como se describe en esta documentación.

AEM Entre las principales distinciones de la ventilación de la se incluyen:

- AEM AEM El código de consumidor de evento se ejecuta fuera de la, no en la misma JVM que se ejecuta en la misma interfaz de usuario de JVM que la de la instancia de usuario.
- AEM El código de producto de es responsable de definir los eventos y enviarlos a los Eventos de Adobe I/O.
- La información del evento está estandarizada y se envía en formato JSON. Para obtener más información, consulte [cloudevents](https://cloudevents.io/).
- AEM AEM Para volver a comunicarse con el cliente de eventos, el consumidor de eventos utiliza la API as a Cloud Service de.


## Por qué y cuándo usarlo

AEM La ventilación de los sistemas ofrece numerosas ventajas para la arquitectura del sistema y la eficacia operativa. AEM Las razones principales para utilizar la ventilación de la son:

- **Para crear arquitecturas impulsadas por eventos**: Facilita la creación de sistemas de correspondencia imprecisa que pueden escalarse de forma independiente y son resistentes a los fallos.
- **Código bajo y menores costes operativos** AEM : Evita las personalizaciones en los sistemas, lo que conduce a sistemas que son más fáciles de mantener y ampliar, lo que reduce los gastos operativos.
- **AEM Simplificación de la comunicación entre sistemas externos y de**: elimina las conexiones punto a punto al permitir que los eventos de Adobe I/O AEM administren las comunicaciones, como la determinación de qué eventos de deben entregarse a sistemas o servicios específicos.
- **Mayor durabilidad de los eventos**: Eventos de Adobe I/O es un sistema de alta disponibilidad y escalable, diseñado para gestionar grandes volúmenes de eventos y enviarlos de forma fiable a los suscriptores.
- **Procesamiento paralelo de eventos**: Permite la entrega de eventos a varios suscriptores simultáneamente, lo que permite el procesamiento de eventos distribuidos en varios sistemas.
- **Desarrollo de aplicaciones sin servidor**: admite la implementación del código de consumidor de evento como aplicación sin servidor, lo que mejora la flexibilidad y escalabilidad del sistema.

### Restricciones

AEM La evasión de eventos, aunque es potente, tiene ciertas limitaciones que se deben tener en cuenta:

- **AEM Disponibilidad restringida a la as a Cloud Service** AEM AEM : Actualmente, la ventilación de Eventing está disponible exclusivamente para el as a Cloud Service de la.
- **Asistencia de eventos limitada** AEM : A partir de ahora, solo se admiten eventos de fragmento de contenido de la. Sin embargo, se espera que el ámbito se amplíe con la adición de más eventos en el futuro.

## Cómo habilitar

AEM AEM La ventilación de la está habilitada por entorno as a Cloud Service de la aplicación y solo está disponible para los entornos en modo previo al lanzamiento. Contacto [AEM Equipo de eventos de la](mailto:grp-aem-events@adobe.com) AEM AEM para habilitar su entorno de con la ventilación de la.

Si ya está habilitado, consulte [AEM Activación de eventos de en el entorno de AEM Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) para ver los pasos siguientes.

## Cómo suscribirse

AEM AEM Para suscribirse a Eventos de la, no tiene que escribir ningún código en la lista de eventos, sino que tiene que escribir un código en la lista de [Consola de Adobe Developer](https://developer.adobe.com/) el proyecto está configurado. La consola de Adobe Developer es una puerta de enlace a las API de Adobe, los SDK, los eventos, el tiempo de ejecución y el generador de aplicaciones.

En este caso, una _proyecto_ en la consola de Adobe Developer AEM, permite suscribirse a los eventos emitidos desde entornos as a Cloud Service de y configurar la entrega de eventos a sistemas externos.

Para obtener más información, consulte [AEM Suscripción a los eventos de la de Adobe Developer](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Cómo consumir

AEM Existen dos métodos principales para consumir eventos de: _push_ y el método _extraer_ método.

- **Método push**: Con este enfoque, los eventos de Adobe I/O notifican al consumidor de eventos de forma proactiva cuando un evento está disponible. Las opciones de integración incluyen Webhooks, Adobe I/O Runtime y Amazon EventBridge.
- **Método Pull**: En este caso, el consumidor de eventos sondea activamente los eventos de Adobe I/O para buscar nuevos eventos. La opción de integración principal para este método es la API de diario de Adobe I/O.

Para obtener más información, consulte [AEM Procesamiento de eventos mediante eventos de Adobe I/O](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Ejemplos

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="AEM Recibir eventos en un webhook" src="./assets/examples/webhook/Eventing-webhook.png"/></a>
        <div><strong><a href="./examples/webhook.md">AEM Recibir eventos en un webhook</a></strong></div>
        <p>
          Utilice el webhook proporcionado por el Adobe AEM para recibir los eventos de la y revisar los detalles del evento.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="AEM Cargar diario de eventos" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">AEM Cargar diario de eventos</a></strong></div>
        <p>
          Utilice la aplicación web proporcionada por el Adobe AEM para cargar los eventos de la revista y revisar los detalles del evento.
        </p>
      </td>
    </tr>
</table>

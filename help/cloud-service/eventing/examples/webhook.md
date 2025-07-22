---
title: Webhooks y eventos de AEM
description: Obtenga información sobre cómo recibir eventos de AEM en un webhook y revisar los detalles del evento, como carga útil, encabezados y metadatos.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: e01eb7ff050321a70d84f8a613705799017dbf5d
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 1%

---

# Webhooks y eventos de AEM

Obtenga información sobre cómo recibir eventos de AEM en un webhook y revisar los detalles del evento, como carga útil, encabezados y metadatos.


>[!VIDEO](https://video.tv.adobe.com/v/3427051?quality=12&learn=on)


>[!IMPORTANT]
>
>Los extremos de demostración en directo de este tutorial se alojaron anteriormente en [Glitch](https://glitch.com/). Desde julio de 2025, Glitch ha interrumpido su servicio de alojamiento y los puntos de conexión ya no son accesibles.
>&#x200B;>Estamos trabajando activamente en migrar las demostraciones a una plataforma alternativa. El contenido del tutorial sigue siendo preciso y pronto se proporcionarán vínculos actualizados.
>&#x200B;>Gracias por su comprensión y paciencia.

Utilice su propio webhook hasta que los puntos finales de demostración en directo vuelvan a estar disponibles.

## Requisitos previos

Para completar este tutorial, necesita lo siguiente:

- Entorno AEM as a Cloud Service con [evento de AEM habilitado](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Proyecto Adobe Developer Console configurado para eventos de AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).


## Acceso al webhook

Para acceder al webhook proporcionado por Adobe, siga estos pasos:

- Verifique que puede acceder al [Glitch - webhook alojado](https://lovely-ancient-coaster.glitch.me/) en una nueva pestaña del navegador.

  ![Glitch: webhook alojado](../assets/examples/webhook/glitch-hosted-webhook.png)

- Escribe un nombre único para tu webhook, por ejemplo `<YOUR_PETS_NAME>-aem-eventing` y haz clic en **Conectar**. Debería ver `Connected to: ${YOUR-WEBHOOK-URL}` mensaje en la pantalla.

  ![Glitch - crear gancho web](../assets/examples/webhook/glitch-create-webhook.png)

- Tome nota de la **URL de webhook**. Lo necesita más adelante en este tutorial.

## Configuración del webhook en un proyecto de Adobe Developer Console

Para recibir eventos de AEM en la URL del gancho web anterior, siga estos pasos:

- En [Adobe Developer Console](https://developer.adobe.com), navegue hasta el proyecto y haga clic para abrirlo.

- En la sección **Productos y servicios**, haga clic en los puntos suspensivos `...` junto a la tarjeta de eventos que desee que envíe eventos de AEM al gancho web y seleccione **Editar**.

  ![Edición de proyecto de Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- En el diálogo **Configurar el registro de eventos** que acaba de abrir, haga clic en **Siguiente** para continuar con el paso **Cómo recibir eventos**.

  ![Configuración del proyecto de Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- En el paso **Cómo recibir eventos**, selecciona la opción **Webhook**, pega la **URL de Webhook** que copiaste anteriormente del webhook alojado en Glitch y haz clic en **Guardar eventos configurados**.

  ![Gancho web del proyecto Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- En la página de Glitch, debería ver una petición GET, es una petición de desafío enviada por Adobe I/O Events para comprobar la URL del webhook.

  ![Problema - solicitud de desafío](../assets/examples/webhook/glitch-challenge-request.png)


## Déclencheur de eventos de AEM

Para almacenar en déclencheur eventos de AEM desde el entorno de AEM as a Cloud Service que se haya registrado en el proyecto de Adobe Developer Console anterior, siga estos pasos:

- Acceda a su entorno de creación de AEM as a Cloud Service e inicie sesión a través de [Cloud Manager](https://my.cloudmanager.adobe.com/).

- Según sus **Eventos suscritos**, cree, actualice, elimine, publique o cancele la publicación de un fragmento de contenido.

## Revisar detalles del evento

Después de completar los pasos anteriores, debería ver los eventos de AEM que se envían al gancho web. Busque la petición POST en la página del webhook Glitch.

![Problema - petición POST](../assets/examples/webhook/glitch-post-request.png)

Estos son los detalles clave de la solicitud de POST:

- ruta: `/webhook/${YOUR-WEBHOOK-URL}`, por ejemplo `/webhook/AdobeTM-aem-eventing`

- encabezados: encabezados de solicitud enviados por Adobe I/O Events, por ejemplo:

```json
{
"connection": "close",
"x-forwarded-for": "34.205.178.127,::ffff:10.10.10.136,::ffff:10.10.84.114",
"x-forwarded-proto": "https,http,http",
"x-forwarded-port": "443,80,80",
"host": "lovely-ancient-coaster.glitch.me",
"content-length": "826",
"x-adobe-public-key2-path": "/prod/keys/pub-key-IkpzhSpTw0.pem",
"x-adobe-delivery-id": "18abfb47-d24a-4684-ade8-f442a3444033",
"x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p46652-e1074060@adobe.com",
"x-adobe-public-key1-path": "/prod/keys/pub-key-Ptc2pD9vT9.pem",
"x-adobe-event-id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
"x-adobe-event-code": "aem.sites.contentFragment.modified",
"user-agent": "Adobe/1.0",
"x-adobe-digital-signature-2": "zGLso15+6PV6X6763/x6WqgxDlEXpkv5ty8q4njaq3aUngAI9VCcYonbScEjljRluzjZ05uMJmRfNxwjj60syxEJPuc0dpmMU635gfna7I4T7IaHs496wx4m2E5mvCM+aKbNQ+NPOutyTqI8Ovq29P2P87GIgMlGhAtOaxRVGNc6ksBxc2tCWbrKUhW8hPJ0sHphU499dN4TT32xrZaiRw4akT3M/hYydsA8dcWpJ7S4dpuDS21YyDHAB8s9Dawtr3fyPEyLgZzpwZDfCqQ8gdSCGqKscE4pScwqPkKOYCHDnBvDZVe583jhcZbHGjk7Ncp/FrgQk7avWsk5XlzcuA==",
"x-adobe-digital-signature-1": "QD7THFJ1vmJqD/BatIpzO6+ACQ9cSKPR7XVaW0LI7cN/xs7ucyri6dmkerOPe9EJpjGoqCg8rxWedrIRQB3lgVskChbHH3Ujx5YG0aTQLSd1Lsn5CFbW1U0l0GqId9Cnd6MccrqSznZXcdW1rMFuRk8+gqwabBifSaLbu3r30G5hmqQd72VtiYTE4m23O3jYIMiv62pRP+a+p4NjNj1XG320uRSry+BPniTjDJ6oN/Ng7aUEKML8idZ/ZTqeh/rJSrVO95UryUolFDRwDkRn5zKonbvhSLAeXzaPhvimWUHtldq9M1WTyRMpsBk8BRzaklxlq+woJ2UjYPUIEzjotw==",
"accept-encoding": "deflate,compress,identity",
"content-type": "application/cloudevents+json; charset=UTF-8",
"x-forwarded-host": "lovely-ancient-coaster.glitch.me",
"traceparent": "00-c27558588d994f169186ca6a3c6607d4-a7e7ee36625488d4-01"
}
```

- cuerpo/carga útil: cuerpo de la solicitud enviado por Adobe I/O Events, por ejemplo:

```json
{
  "specversion": "1.0",
  "type": "aem.sites.contentFragment.modified",
  "source": "acct:aem-p46652-e1074060@adobe.com",
  "id": "bf922a49-9db4-4377-baf4-70e96e15c45f",
  "time": "2023-12-12T20:36:43.583228Z",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "datacontenttype": "application/json",
  "data": {
    "user": {
      "imsUserId": "933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xxx@adobe.com",
      "displayName": "First LastName",
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "model": {
      "id": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9a2d3e6a-efda-4079-a86e-0ef2ede692da",
    "properties": [
      {
        "name": "groupSize",
        "changeType": "modified"
      }
    ]
  },
  "event_id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
  "recipient_client_id": "f51ea733ba404db299fefbf285dc1c42"
}
```

Puede ver que los detalles del evento de AEM tienen toda la información necesaria para procesar el evento en el webhook. Por ejemplo, el tipo de evento (`type`), el origen de evento (`source`), el identificador de evento (`event_id`), la hora del evento (`time`) y los datos de evento (`data`).

## Recursos adicionales

- El código fuente de [AEM-Eventing Webhook](../assets/examples/webhook/aemeventing-webhook.tgz) está disponible para su referencia.

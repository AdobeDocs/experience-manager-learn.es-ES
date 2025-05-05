---
title: Servicio de correo electrónico
description: Obtenga información sobre cómo configurar AEM as a Cloud Service para que se conecte con un servicio de correo electrónico mediante puertos de salida.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 76
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Servicio de correo electrónico

Envíe correos electrónicos desde AEM as a Cloud Service configurando `DefaultMailService` de AEM para usar puertos de salida de red avanzados.

Como la mayoría de los servicios de correo no se ejecutan a través de HTTP/HTTPS, las conexiones a los servicios de correo de AEM as a Cloud Service deben procesarse como proxy de salida.

+ `smtp.host` se ha establecido en la variable de entorno OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]`, de modo que se enrute a través de la salida.
   + `$[env:AEM_PROXY_HOST]` es una variable reservada que AEM as a Cloud Service asigna al host `proxy.tunnel` interno.
   + NO intente establecer `AEM_PROXY_HOST` a través de Cloud Manager.
+ `smtp.port` se ha establecido en el puerto `portForward.portOrig` que se asigna al host y al puerto del servicio de correo electrónico de destino. Este ejemplo utiliza la asignación: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + `smpt.port` está establecido en el puerto `portForward.portOrig` y NO en el puerto real del servidor SMTP. La regla `portForwards` de Cloud Manager ha establecido la asignación entre el puerto `smtp.port` y el puerto `portForward.portOrig` (como se muestra a continuación).

Dado que los secretos no deben almacenarse en el código, es mejor proporcionar el nombre de usuario y la contraseña del servicio de correo electrónico utilizando [variables de configuración OSGi secretas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=es#secret-configuration-values), configuradas mediante AIO CLI o la API de Cloud Manager.

Normalmente, [salida de puerto flexible](../flexible-port-egress.md) se usa para satisfacer la integración con un servicio de correo electrónico a menos que sea necesario `allowlist` la IP de Adobe, en cuyo caso se puede usar [dirección IP de salida dedicada](../dedicated-egress-ip-address.md).

Además, revise la documentación de AEM sobre [enviar correo electrónico](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=es#sending-email).

## Compatibilidad avanzada con redes

Las siguientes opciones avanzadas de red admiten el siguiente ejemplo de código.

Asegúrese de que la configuración avanzada de red [proper](../advanced-networking.md#advanced-networking) se haya configurado antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuración de OSGi

Este ejemplo de configuración de OSGi configura el servicio OSGi de correo de AEM para que utilice un servicio de correo externo mediante la siguiente regla de Cloud Manager `portForwards` de la operación [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration).

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Configure [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=es#sending-email) de AEM según lo requiera su proveedor de correo electrónico (p. ej. `smtp.ssl`, etc.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

La variable OSGi `EMAIL_USERNAME` y `EMAIL_PASSWORD`, así como el secreto, se pueden establecer por entorno mediante:

+ [Configuración del entorno de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=es)
+ o utilizando el comando `aio CLI`

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```

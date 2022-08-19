---
title: Servicio de correo electrónico
description: Obtenga información sobre cómo configurar AEM as a Cloud Service para conectarse con un servicio de correo electrónico mediante puertos de salida.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: d6eddceb3f414e67b5b6e3fba071cd95597dc41c
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# Servicio de correo electrónico

Enviar correos electrónicos desde AEM as a Cloud Service configurando AEM `DefaultMailService` para utilizar puertos de salida de red avanzados.

Debido a que (la mayoría) de los servicios de correo no se ejecutan en HTTP/HTTPS, las conexiones a los servicios de correo de AEM as a Cloud Service deben eliminarse mediante proxy.

+ `smtp.host` está configurada en la variable de entorno OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` así que se enruta a través de la salida.
   + `$[env:AEM_PROXY_HOST]` es una variable reservada que AEM as a Cloud Service se asigna a la variable interna `proxy.tunnel` host.
   + NO intente establecer la variable `AEM_PROXY_HOST` mediante Cloud Manager.
+ `smtp.port` se configura como `portForward.portOrig` puerto que se asigna al host y puerto del servicio de correo electrónico de destino. Este ejemplo utiliza la asignación: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + La variable `smpt.port` se configura como `portForward.portOrig` y NO el puerto real del servidor SMTP. La asignación entre la variable `smtp.port` y `portForward.portOrig` el puerto es establecido por Cloud Manager `portForwards` (como se muestra a continuación).

Dado que los secretos no deben almacenarse en el código, el nombre de usuario y la contraseña del servicio de correo electrónico son los que mejor se proporcionan utilizando [variables de configuración OSGi secretas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), se configura mediante la CLI de AIO o la API de Cloud Manager.

Normalmente, [salida de puerto flexible](../flexible-port-egress.md) se utiliza para satisfacer la integración con un servicio de correo electrónico a menos que sea necesario `allowlist` la IP de Adobe, en cuyo caso [dirección ip de salida dedicada](../dedicated-egress-ip-address.md) se puede usar.

Además, revise AEM documentación sobre [enviar correo electrónico](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## Compatibilidad avanzada con redes

El siguiente ejemplo de código es compatible con las siguientes opciones avanzadas de red.

Asegúrese de que la variable [apropiado](../advanced-networking.md#advanced-networking) se ha configurado la configuración avanzada de redes antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ü | š | š | š |

## Configuración de OSGi

Este ejemplo de configuración de OSGi configura AEM servicio de correo OSGi para utilizar un servicio de correo externo a través de Cloud Manager siguiente `portForwards` regla de [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación.

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

Configurar AEM [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) según lo requiera su proveedor de correo electrónico (p. ej. `smtp.ssl`, etc.).

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

La variable `EMAIL_USERNAME` y `EMAIL_PASSWORD` La variable OSGi y el secreto se pueden configurar por entorno mediante:

+ [Configuración del entorno de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ o utilizando `aio CLI` command

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```

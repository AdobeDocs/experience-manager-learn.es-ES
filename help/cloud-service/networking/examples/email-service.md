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
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---

# Servicio de correo electrónico

Enviar correos electrónicos desde AEM as a Cloud Service configurando AEM `DefaultMailService` para utilizar puertos de salida de red avanzados.

Debido a que (la mayoría) de los servicios de correo no se ejecutan en HTTP/HTTPS, las conexiones a los servicios de correo de AEM as a Cloud Service deben eliminarse mediante proxy.

+ `smtp.host` está configurada en la variable de entorno OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` así que se enruta a través de la salida.
+ `smtp.port` se configura como `portForward.portOrig` puerto que se asigna al host y puerto del servicio de correo electrónico de destino. Este ejemplo utiliza la asignación: `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.

Dado que los secretos no deben almacenarse en el código, el nombre de usuario y la contraseña del servicio de correo electrónico son los que mejor se proporcionan utilizando [variables de configuración OSGi secretas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), se configura mediante la CLI de AIO o la API de Cloud Manager.

Normalmente, [salida de puerto flexible](../flexible-port-egress.md) se utiliza para satisfacer la integración con un servicio de correo electrónico a menos que sea necesario `allowlist` la IP de Adobe, en cuyo caso [dirección ip de salida dedicada](../dedicated-egress-ip-address.md) se puede usar.

## Compatibilidad avanzada con redes

El siguiente ejemplo de código es compatible con las siguientes opciones avanzadas de red.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ü | š | š | š |

## Configuración de OSGi

Este ejemplo de configuración de OSGi configura AEM servicio de correo OSGi para utilizar un servicio de correo externo a través de Cloud Manager siguiente `portForwards` regla de [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación.

```json
...
"portForwards": [{
    "name": "smtp.sendgrid.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=apikey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

Lo siguiente `aio CLI` puede utilizarse para configurar los secretos OSGi por entorno:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "apikey" --secret EMAIL_PASSWORD "password123"
```
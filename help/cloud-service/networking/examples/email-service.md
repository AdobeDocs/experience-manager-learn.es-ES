---
title: Servicio de correo electrónico
description: AEM Obtenga información sobre cómo configurar el acceso as a Cloud Service a la para conectarse con un servicio de correo electrónico mediante puertos de salida.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 98
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Servicio de correo electrónico

AEM Envío de correos electrónicos desde el as a Cloud Service AEM configurando la opción de envío `DefaultMailService` para utilizar puertos de salida de red avanzados.

AEM Dado que la mayoría de los servicios de correo no se ejecutan a través de HTTP/HTTPS, las conexiones a servicios de correo desde el as a Cloud Service deben ser procesadas como proxy hacia fuera.

+ `smtp.host` se establece en la variable de entorno OSGi. `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` así que se dirige a través de la salida.
   + `$[env:AEM_PROXY_HOST]` AEM es una variable reservada que se asigna de forma as a Cloud Service a la variable interna de `proxy.tunnel` host.
   + NO intente establecer la variable `AEM_PROXY_HOST` mediante Cloud Manager.
+ `smtp.port` se establece en `portForward.portOrig` puerto que se asigna al host y al puerto del servicio de correo electrónico de destino. Este ejemplo utiliza la asignación: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + El `smpt.port` se establece en `portForward.portOrig` y NO el puerto real del servidor SMTP. La asignación entre las variables `smtp.port` y el `portForward.portOrig` El puerto lo establece Cloud Manager `portForwards` (como se muestra a continuación).

Dado que los secretos no deben almacenarse en el código, el nombre de usuario y la contraseña del servicio de correo electrónico se proporcionan mejor utilizando [variables de configuración OSGi secretas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), establecida mediante AIO CLI o la API de Cloud Manager.

Normalmente, [salida de puerto flexible](../flexible-port-egress.md) se utiliza para satisfacer la integración con un servicio de correo electrónico a menos que sea necesario `allowlist` la IP de Adobe, en cuyo caso [dirección ip de salida dedicada](../dedicated-egress-ip-address.md) se puede utilizar.

AEM Además, revise la documentación de la sobre [enviar correo electrónico](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email).

## Compatibilidad avanzada con redes

Las siguientes opciones avanzadas de red admiten el siguiente ejemplo de código.

Asegúrese de que la [apropiado](../advanced-networking.md#advanced-networking) la configuración avanzada de red se ha establecido antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuración de OSGi

AEM En este ejemplo de configuración de OSGi se configura el servicio OSGi de correo electrónico para que utilice un servicio de correo externo mediante el siguiente Cloud Manager `portForwards` regla de la [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación.

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

AEM Configurar la [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) según lo requiera su proveedor de correo electrónico (p. ej. `smtp.ssl`, etc.).

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

El `EMAIL_USERNAME` y `EMAIL_PASSWORD` La variable OSGi y el secreto se pueden establecer por entorno, utilizando lo siguiente:

+ [Configuración del entorno de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ o utilizando el `aio CLI` mando

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```

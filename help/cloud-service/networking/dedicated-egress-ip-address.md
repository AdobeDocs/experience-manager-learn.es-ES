---
title: Dirección IP de salida dedicada
description: Aprenda a configurar y utilizar la dirección IP de salida dedicada, que permite que las conexiones salientes de AEM se originen desde una IP dedicada.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '1238'
ht-degree: 1%

---

# Dirección IP de salida dedicada

Aprenda a configurar y utilizar la dirección IP de salida dedicada, que permite que las conexiones salientes de AEM se originen desde una IP dedicada.

## ¿Qué es la dirección IP de salida dedicada?

La dirección IP de salida dedicada permite que las solicitudes de AEM as a Cloud Service utilicen una dirección IP dedicada, lo que permite que los servicios externos filtren las solicitudes entrantes por esta dirección IP. Like [puertos de salida flexibles](./flexible-port-egress.md), la IP de salida dedicada le permite entrar en puertos no estándar.

Un programa de Cloud Manager solo puede tener un __single__ tipo de infraestructura de red. Asegúrese de que la dirección IP de salida dedicada sea la más [tipo apropiado de infraestructura de red](./advanced-networking.md)  para su AEM as a Cloud Service antes de ejecutar los siguientes comandos.

>[!MORELIKETHIS]
>
> Lea el AEM as a Cloud Service [documentación avanzada de configuración de red](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) para obtener más información sobre la dirección IP de salida dedicada.

## Requisitos previos

A la hora de configurar la dirección IP de salida dedicada, es necesario lo siguiente:

+ API de Cloud Manager con [Permisos de propietario empresarial de Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acceso a [Credenciales de autenticación de la API de Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID de organización (también conocido como ID de organización de IMS)
   + ID del cliente (también conocido como clave de API)
   + Token de acceso (también conocido como token de portador)
+ ID del programa de Cloud Manager
+ ID del entorno de Cloud Manager

Para obtener más información, consulte el siguiente tutorial sobre cómo configurar, configurar y obtener credenciales de API de Cloud Manager y cómo utilizarlas para realizar una llamada de API de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

Este tutorial utiliza `curl` para realizar las configuraciones de la API de Cloud Manager. El `curl` asume una sintaxis Linux/macOS. Si utiliza el símbolo del sistema de Windows, reemplace la variable `\` carácter de salto de línea con `^`.

## Habilitar la dirección IP de salida dedicada en el programa

Comience habilitando y configurando la dirección IP de salida dedicada en AEM as a Cloud Service.

1. En primer lugar, determine la región en la que se configurarán las redes avanzadas mediante la API de Cloud Manager [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. La variable `region name` es necesario para realizar llamadas posteriores a la API de Cloud Manager. Normalmente, se utiliza la región en la que reside el entorno de producción.

   __petición HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Habilitar la dirección IP de salida dedicada para un programa de Cloud Manager mediante la API de Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. Utilice el `region` código obtenido de la API de Cloud Manager `listRegions` operación.

   __solicitud HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Espere 15 minutos para que el programa Cloud Manager proporcione la infraestructura de red.

1. Comprobar que el entorno ha finalizado __dirección IP de salida dedicada__ configuración mediante la API de Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) mediante la función `id` devuelto desde la solicitud HTTP createNetworkInfrastructure en el paso anterior.

   __petición HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Compruebe que la respuesta HTTP contiene un __status__ de __ready__. Si aún no está listo, vuelva a comprobar el estado cada pocos minutos.

## Configuración de proxies de direcciones IP de salida dedicados por entorno

1. Habilitar y configurar el __dirección IP de salida dedicada__ configuración en cada entorno as a Cloud Service AEM mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación.

   __solicitud HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Defina los parámetros JSON en una `dedicated-egress-ip-address.json` y se proporcionan para curl a través de `... -d @./dedicated-egress-ip-address.json`.

   [Descargue el ejemplo dedicated-egress-ip-address.json](./assets/dedicated-egress-ip-address.json). Este archivo solo es un ejemplo. Configure el archivo según sea necesario en función de los campos opcionales/obligatorios documentados en [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   La firma HTTP de la configuración de la dirección IP de salida dedicada solo difiere de [puerto de salida flexible](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) en el sentido de que también es compatible con el `nonProxyHosts` configuración.

   `nonProxyHosts` declara un conjunto de hosts para los que el puerto 80 o 443 debe enrutarse a través de los intervalos predeterminados de direcciones IP compartidas en lugar de la dirección IP de salida dedicada. `nonProxyHosts` puede resultar útil, ya que la salida de tráfico a través de direcciones IP compartidas puede optimizarse aún más automáticamente mediante Adobe.

   Para cada `portForwards` , la red avanzada define la siguiente regla de reenvío:

   | Host proxy | Puerto proxy |  | Host externo | Puerto externo |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Para cada entorno, valide las reglas de salida que están en vigor utilizando la API de Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación.

   __petición HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Las configuraciones de direcciones IP de salida dedicadas se pueden actualizar mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. Recordar `enableEnvironmentAdvancedNetworkingConfiguration` es `PUT` , por lo que todas las reglas deben proporcionarse con cada invocación de esta operación.

1. Obtenga la variable __dirección IP de salida dedicada__ mediante una resolución DNS (por ejemplo, [DNSChecker.org](https://dnschecker.org/)) en el host: `p{programId}.external.adobeaemcloud.com`o ejecutando `dig` desde la línea de comandos.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   El nombre de host no puede ser `pinged`, ya que es una salida y _not_ y el ingreso.

1. Ahora puede usar la dirección IP de salida dedicada en su código de AEM y configuración personalizados. A menudo, al utilizar direcciones IP de salida dedicadas, los servicios externos AEM conexiones as a Cloud Service a se configuran para permitir solo el tráfico desde esta dirección IP dedicada.

## Conexión a servicios externos a través de la dirección IP de salida dedicada

Con la dirección IP de salida dedicada habilitada, AEM código y configuración pueden utilizar la IP de salida dedicada para realizar llamadas a servicios externos. Hay dos tipos de llamadas externas que AEM tratan de forma diferente:

1. Llamadas HTTP/HTTPS a servicios externos
   + Incluye llamadas HTTP/HTTPS realizadas a servicios que se ejecutan en puertos distintos de los puertos estándar 80 o 443.
1. llamadas no HTTP/HTTPS a servicios externos
   + Incluye llamadas que no sean HTTP, como conexiones con servidores de correo, bases de datos SQL o servicios que se ejecutan en otros protocolos que no sean HTTP/HTTPS.

Las solicitudes HTTP/HTTPS de AEM en puertos estándar (80/443) están permitidas de forma predeterminada, pero no utilizan la dirección IP de salida dedicada si no se configuran correctamente como se describe a continuación.

>[!TIP]
>
> Consulte la documentación de la dirección IP de salida dedicada de AEM as a Cloud Service para [el conjunto completo de reglas de enrutamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

Al crear conexiones HTTP/HTTPS desde AEM, para obtener una dirección IP de salida dedicada, la conexión debe realizarse mediante hosts y puertos especiales, proporcionados mediante marcadores de posición.

AEM proporciona dos conjuntos de variables especiales del sistema Java™ que se asignan a AEM proxies HTTP/HTTPS.

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi | Configuración del servidor web mod_proxy de Apache | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Host proxy para conexiones HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | Puerto proxy para conexiones HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | Host proxy para conexiones HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | Puerto proxy para conexiones HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

Las solicitudes a los servicios externos HTTP/HTTPS deben realizarse configurando la configuración proxy del cliente HTTP Java™ mediante AEM valores de puertos/hosts proxy.

Al realizar llamadas HTTP/HTTPS a servicios externos en cualquier puerto, no se corresponde con `portForwards` debe definirse mediante la API de Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` , ya que las &quot;reglas&quot; de reenvío de puertos se definen &quot;en código&quot;.

#### Ejemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Ejemplo de código Java™ que establece la conexión HTTP/HTTPS de AEM as a Cloud Service a un servicio externo mediante el protocolo HTTP/HTTPS.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Conexiones no HTTP/HTTPS a servicios externos

Al crear conexiones no HTTP/HTTPS (por ejemplo SQL, SMTP, etc.) de AEM, la conexión debe realizarse mediante un nombre de host especial proporcionado por AEM.

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Host proxy para conexiones no HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


A continuación, se llama a las conexiones a servicios externos a través de la función `AEM_PROXY_HOST` y el puerto asignado (`portForwards.portOrig`), que luego AEM enruta al nombre de host externo asignado (`portForwards.name`) y puerto (`portForwards.portDest`).

| Host proxy | Puerto proxy |  | Host externo | Puerto externo |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Ejemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexión SQL utilizando JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexión SQL utilizando JDBC DataSourcePool</a></strong></div>
      <p>
            Ejemplo de código Java™ conectándose a bases de datos SQL externas configurando AEM grupo de fuentes de datos JDBC.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexión SQL mediante API de Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexión SQL mediante API de Java™</a></strong></div>
      <p>
            Ejemplo de código de Java™ que se conecta a bases de datos SQL externas mediante las API SQL de Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Red privada virtual (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servicio de correo electrónico</a></strong></div>
      <p>
        Ejemplo de configuración de OSGi utilizando AEM para conectarse a servicios de correo electrónico externos.
      </p>
    </td>   
</tr></table>

---
title: Red privada virtual (VPN)
description: Aprenda a conectar AEM as a Cloud Service con su VPN para crear canales de comunicación seguros entre AEM y servicios internos.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '1261'
ht-degree: 0%

---


# Red privada virtual (VPN)

Aprenda a conectar AEM as a Cloud Service con su VPN para crear canales de comunicación seguros entre AEM y servicios internos.

## ¿Qué es la red privada virtual?

La red privada virtual (VPN) permite a un cliente as a Cloud Service AEM conectar un programa de Cloud Manager a un cliente existente, [admitido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) VPN. Esto permite conexiones seguras y controladas entre AEM as a Cloud Service y servicios dentro de la red del cliente.

Un programa de Cloud Manager solo puede tener un __single__ tipo de infraestructura de red. Asegúrese de que la red privada virtual sea la más [tipo apropiado de infraestructura de red](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html%3Flang%3Dja#general-vpn-considerations) para su AEM as a Cloud Service antes de ejecutar los siguientes comandos.

>[!MORELIKETHIS]
>
> Lea el AEM as a Cloud Service [documentación avanzada de configuración de red](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) para obtener más información sobre la red privada virtual.

## Requisitos previos

A la hora de configurar la red privada virtual, es necesario lo siguiente:

+ Cuenta de Adobe con [Permisos de propietario empresarial de Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Acceso a [Credenciales de autenticación de la API de Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID de organización (también conocido como ID de organización de IMS)
   + ID del cliente (también conocido como clave de API)
   + Token de acceso (también conocido como token de portador)
+ ID del programa de Cloud Manager
+ ID del entorno de Cloud Manager
+ Una red privada virtual, con acceso a todos los parámetros de conexión necesarios.

Este tutorial utiliza `curl` para realizar las configuraciones de la API de Cloud Manager. El `curl` asume una sintaxis Linux/macOS. Si utiliza el símbolo del sistema de Windows, reemplace la variable `\` carácter de salto de línea con `^`.

## Habilitar la red privada virtual por programa

Comience habilitando la Red privada virtual en AEM as a Cloud Service.

1. En primer lugar, determine la región en la que se configurarán las redes avanzadas mediante la API de Cloud Manager [listRegion](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) operación. La variable `region name` será necesario para realizar llamadas de API de Cloud Manager posteriores. Normalmente, se utiliza la región en la que reside el entorno de producción.

   __petición HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Habilitar la red privada virtual para un programa de Cloud Manager mediante las API de Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) operación. Utilice el `region` código obtenido de la API de Cloud Manager `listRegions` operación.

   __solicitud HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Defina los parámetros JSON en una `vpn-create.json` y se proporcionan para curl a través de `... -d @./vpn-create.json`.

[Descargue el ejemplo vpn-create.json](./assets/vpn-create.json)

   ```json
   { 
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [ 
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22", 
               "10.151.202.22",
               "10.154.155.22"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Espere entre 45 y 60 minutos para que el programa Cloud Manager proporcione la infraestructura de red.

1. Comprobar que el entorno ha finalizado __Red privada virtual__ configuración mediante la API de Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) mediante la función `id` devuelto desde la solicitud HTTP createNetworkInfrastructure en el paso anterior.

   __petición HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Compruebe que la respuesta HTTP contiene un __status__ de __ready__. Si aún no está listo, vuelva a comprobar el estado cada pocos minutos.

## Configuración de proxies de red privada virtual por entorno

1. Habilitar y configurar el __Red privada virtual__ configuración en cada entorno as a Cloud Service AEM mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación.

   __solicitud HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Defina los parámetros JSON en una `vpn-configure.json` y se proporcionan para curl a través de `... -d @./vpn-configure.json`.

[Descargue el ejemplo vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   `nonProxyHosts` declara un conjunto de hosts para los que el puerto 80 o 443 debe enrutarse a través de los intervalos predeterminados de direcciones IP compartidas en lugar de la dirección IP de salida dedicada. Esto puede resultar útil, ya que la salida de tráfico a través de direcciones IP compartidas puede optimizarse automáticamente mediante Adobe.

   Para cada `portForwards` , la red avanzada define la siguiente regla de reenvío:

   | Host proxy | Puerto proxy |  | Host externo | Puerto externo |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Si su implementación AEM __only__ requiere conexiones HTTP/HTTPS al servicio externo, deje el `portForwards` matriz vacía, ya que estas reglas solo son necesarias para solicitudes que no sean HTTP/HTTPS.


1. Para cada entorno, valide las reglas de enrutamiento vpn que están en vigor utilizando la API de Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) operación.

   __petición HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Las configuraciones de proxy de red privada virtual se pueden actualizar mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación. Recordar `enableEnvironmentAdvancedNetworkingConfiguration` es `PUT` , por lo que todas las reglas deben proporcionarse con cada invocación de esta operación.

1. Ahora puede utilizar la configuración de salida de red privada virtual en su código de AEM y configuración personalizados.

## Conexión a servicios externos a través de la red privada virtual

Con la red privada virtual habilitada, AEM código y configuración pueden utilizarlos para realizar llamadas a servicios externos a través de la VPN. Hay dos tipos de llamadas externas que AEM tratan de forma diferente:

1. Llamadas HTTP/HTTPS a servicios externos en puertos no estándar
   + Incluye llamadas HTTP/HTTPS realizadas a servicios que se ejecutan en puertos distintos de los puertos estándar 80 o 443.
1. llamadas no HTTP/HTTPS a servicios externos
   + Incluye llamadas que no sean HTTP, como conexiones con servidores de correo, bases de datos SQL o servicios que se ejecutan en otros protocolos que no sean HTTP/HTTPS.

Las solicitudes HTTP/HTTPS de AEM en puertos estándar (80/443) están permitidas de forma predeterminada y no necesitan ninguna configuración o consideración extra.

### HTTP/HTTPS en puertos no estándar

Al crear conexiones HTTP/HTTPS a puertos no estándar (no-80/443) desde AEM, la conexión debe realizarse a través de puertos y host especiales, proporcionados mediante marcadores de posición.

AEM proporciona dos conjuntos de variables especiales del sistema Java™ que se asignan a AEM proxies HTTP/HTTPS.

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi | Configuración del servidor web mod_proxy de Apache | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Host proxy para conexiones HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | Puerto proxy para conexiones HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | Host proxy para conexiones HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | Puerto proxy para conexiones HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

Las solicitudes a los servicios externos HTTP/HTTPS deben realizarse configurando la configuración proxy del cliente HTTP Java™ a través de los valores de puertos/hosts proxy AEM.

Cuando se realizan llamadas HTTP/HTTPS a servicios externos en puertos no estándar, no se corresponde con `portForwards` debe definirse mediante la API de Cloud Manager `__enableEnvironmentAdvancedNetworkingConfiguration` , ya que las &quot;reglas&quot; de reenvío de puertos se definen &quot;en código&quot;.

>[!TIP]
>
> Consulte la documentación de red privada virtual de AEM as a Cloud Service para [el conjunto completo de reglas de enrutamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### Ejemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS en puertos no estándar" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS en puertos no estándar</a></strong></div>
    <p>
        Ejemplo de código Java™ que establece la conexión HTTP/HTTPS desde AEM as a Cloud Service a un servicio externo en puertos HTTP/HTTPS no estándar.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Ejemplos de código de conexiones no HTTP/HTTPS

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

### Limitar el acceso a AEM as a Cloud Service a través de VPN

La configuración de la Red privada virtual permite que el acceso a AEM entornos as a Cloud Service se limite al acceso VPN.

#### Ejemplos de configuración

<table><tr>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en"><img alt="Aplicación de una lista de permitidos IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en">Aplicación de una lista de permitidos IP</a></strong></div>
      <p>
            Configure una lista de permitidos IP de modo que solo el tráfico VPN pueda acceder a AEM.
      </p>
    </td>   
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Restricciones de acceso VPN basadas en rutas a AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Restricciones de acceso VPN basadas en rutas a AEM Publish</a></strong></div>
      <p>
            Requiere acceso VPN para rutas específicas en AEM Publish.
      </p>
    </td>
   <td></td>   
</tr></table>

---
title: Salida de puerto flexible
description: Aprenda a configurar y utilizar salidas de puerto flexibles para admitir conexiones externas de AEM as a Cloud Service a servicios externos.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '1032'
ht-degree: 0%

---


# Salida de puerto flexible

Aprenda a configurar y utilizar salidas de puerto flexibles para admitir conexiones externas de AEM as a Cloud Service a servicios externos.

## ¿Qué es la salida de puerto flexible?

La salida de puerto flexible permite adjuntar reglas de reenvío de puertos personalizadas y específicas a AEM as a Cloud Service, lo que permite realizar conexiones de AEM a servicios externos.

Un programa de Cloud Manager solo puede tener un __single__ tipo de infraestructura de red. Asegúrese de que la dirección IP de salida dedicada sea la más [tipo apropiado de infraestructura de red](./advanced-networking.md)  para su AEM as a Cloud Service antes de ejecutar los siguientes comandos.

>[!MORELIKETHIS]
>
> Lea el AEM as a Cloud Service [documentación avanzada de configuración de red](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#flexible-port-egress) para obtener más información sobre la salida de puerto flexible.

## Requisitos previos

A la hora de configurar salidas de puerto flexibles, es necesario lo siguiente:

+ proyecto de Adobe I/O con la API de Cloud Manager habilitada y [Permisos de propietario empresarial de Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Acceso a [Credenciales de autenticación de la API de Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID de organización (también conocido como ID de organización de IMS)
   + ID del cliente (también conocido como clave de API)
   + Token de acceso (también conocido como token de portador)
+ ID del programa de Cloud Manager
+ ID del entorno de Cloud Manager

Este tutorial utiliza `curl` para realizar las configuraciones de la API de Cloud Manager. El `curl` asume una sintaxis Linux/macOS. Si utiliza el símbolo del sistema de Windows, reemplace la variable `\` carácter de salto de línea con `^`.

## Habilitar salidas de puerto flexibles por programa

Comience habilitando la salida de puerto flexible en AEM as a Cloud Service.

1. En primer lugar, determine la región en la que se configurarán las redes avanzadas mediante la API de Cloud Manager [listRegion](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) operación. La variable `region name` será necesario para realizar llamadas de API de Cloud Manager posteriores. Normalmente, se utiliza la región en la que reside el entorno de producción.

   __petición HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Habilitar la salida de puerto flexible para un programa de Cloud Manager mediante la API de Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) operación. Utilice el `region` código obtenido de la API de Cloud Manager `listRegions` operación.

   __solicitud HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Espere 15 minutos para que el programa Cloud Manager proporcione la infraestructura de red.

1. Comprobar que el entorno ha finalizado __salida de puerto flexible__ configuración mediante la API de Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) mediante la función `id` devuelto desde la solicitud HTTP createNetworkInfrastructure en el paso anterior.

   __petición HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Compruebe que la respuesta HTTP contiene un __status__ de __ready__. Si aún no está listo, vuelva a comprobar el estado cada pocos minutos.

## Configurar proxies de salida de puerto flexibles por entorno

1. Habilitar y configurar el __salida de puerto flexible__ configuración en cada entorno as a Cloud Service AEM mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación.

   __solicitud HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Defina los parámetros JSON en una `flexible-port-egress.json` y se proporcionan para curl a través de `... -d @./flexible-port-egress.json`.

[Descargue el ejemplo flexible-port-egress.json](./assets/flexible-port-egress.json)

   ```json
   {
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

   Para cada `portForwards` , la red avanzada define la siguiente regla de reenvío:

   | Host proxy | Puerto proxy |  | Host externo | Puerto externo |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Si su implementación AEM __only__ requiere conexiones HTTP/HTTPS (puerto 80/443) al servicio externo, deje el `portForwards` matriz vacía, ya que estas reglas solo son necesarias para solicitudes que no sean HTTP/HTTPS.

1. Para cada entorno, valide las reglas de salida que están en vigor utilizando la API de Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) operación.

   __petición HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Las configuraciones de salida de puerto flexibles se pueden actualizar mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación. Recordar `enableEnvironmentAdvancedNetworkingConfiguration` es `PUT` , por lo que todas las reglas deben proporcionarse con cada invocación de esta operación.

1. Ahora puede utilizar la configuración de salida de puerto flexible en su código de AEM y configuración personalizados.


## Conexión a servicios externos a través de una salida de puerto flexible

Con el proxy de salida de puerto flexible habilitado, AEM código y configuración pueden utilizarlos para realizar llamadas a servicios externos. Hay dos tipos de llamadas externas que AEM tratan de forma diferente:

1. Llamadas HTTP/HTTPS a servicios externos en puertos no estándar
   + Incluye llamadas HTTP/HTTPS realizadas a servicios que se ejecutan en puertos distintos de los puertos estándar 80 o 443.
1. llamadas no HTTP/HTTPS a servicios externos
   + Incluye llamadas que no sean HTTP, como conexiones con servidores de correo, bases de datos SQL o servicios que se ejecutan en otros protocolos que no sean HTTP/HTTPS.

Las solicitudes HTTP/HTTPS de AEM en puertos estándar (80/443) están permitidas de forma predeterminada y no necesitan ninguna configuración o consideración extra.


### HTTP/HTTPS en puertos no estándar

Al crear conexiones HTTP/HTTPS a puertos no estándar (no-80/443) desde AEM, las conexiones deben realizarse a través de puertos y host especiales, proporcionados mediante marcadores de posición.

AEM proporciona dos conjuntos de variables especiales del sistema Java™ que se asignan a AEM proxies HTTP/HTTPS.

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi | | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Host proxy para conexiones HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | | `AEM_HTTP_PROXY_PORT` | Puerto proxy para conexiones HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` | | `AEM_HTTPS_PROXY_HOST` | Host proxy para conexiones HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | | `AEM_HTTPS_PROXY_PORT` | Puerto proxy para conexiones HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` |

Cuando se realizan llamadas HTTP/HTTPS a servicios externos en puertos no estándar, no se corresponde con `portForwards` debe definirse mediante la API de Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` , ya que las &quot;reglas&quot; de reenvío de puertos se definen &quot;en código&quot;.

>[!TIP]
>
> Consulte la documentación de salida de puerto flexible de AEM as a Cloud Service para [el conjunto completo de reglas de enrutamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

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

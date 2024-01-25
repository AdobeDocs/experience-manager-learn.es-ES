---
title: Salida de puerto flexible
description: AEM Obtenga información sobre cómo configurar y utilizar la salida de puerto flexible para admitir conexiones externas desde servicios as a Cloud Service a servicios externos, desde el servidor de correo electrónico a la red, o desde el servidor de correo electrónico.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
duration: 906
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 1%

---

# Salida de puerto flexible

AEM Obtenga información sobre cómo configurar y utilizar la salida de puerto flexible para admitir conexiones externas desde servicios as a Cloud Service a servicios externos, desde el servidor de correo electrónico a la red, o desde el servidor de correo electrónico.

## ¿Qué es la salida de puerto flexible?

AEM La salida de puerto flexible permite que se adjunten reglas de reenvío de puerto personalizadas y específicas a los puertos as a Cloud Service AEM, lo que permite que se realicen conexiones desde los servicios externos a los que se va a realizar la conexión.

Un programa de Cloud Manager solo puede tener un __soltero__ tipo de infraestructura de red. Asegúrese de que la dirección IP de salida dedicada sea la más adecuada [tipo adecuado de infraestructura de red](./advanced-networking.md)  AEM para el recurso as a Cloud Service antes de ejecutar los siguientes comandos.

>[!MORELIKETHIS]
>
> AEM Leer el recurso as a Cloud Service [documentación de configuración de red avanzada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) para obtener más información sobre la salida de puerto flexible.

## Requisitos previos

Se requiere lo siguiente al configurar una salida de puerto flexible:

+ Proyecto de consola de Adobe Developer con la API de Cloud Manager habilitada y [Permisos de propietario de empresa de Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acceso a [Credenciales de autenticación de la API de Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID de organización (también conocido como ID de organización de IMS)
   + ID del cliente (también conocido como clave API)
   + Token de acceso (también conocido como Token de portador)
+ El ID de programa de Cloud Manager
+ Los ID de entorno de Cloud Manager

Para obtener más información, consulte el siguiente tutorial sobre cómo configurar y obtener credenciales de API de Cloud Manager y cómo utilizarlas para realizar una llamada de API de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Este tutorial utiliza `curl` para realizar las configuraciones de la API de Cloud Manager. El proporcionado `curl` Los comandos de asumen una sintaxis de Linux/macOS. Si utiliza el símbolo del sistema de Windows, reemplace la variable `\` salto de línea con `^`.

## Habilitar salida de puerto flexible por programa

AEM Comience habilitando la salida de puerto flexible en el modo as a Cloud Service.

1. En primer lugar, determine la región en la que está configurada la red avanzada mediante la API de Cloud Manager [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. El `region name` es necesario para realizar llamadas posteriores a la API de Cloud Manager. Normalmente, se utiliza la región en la que reside el entorno de producción.

   AEM Encuentre la región de su entorno as a Cloud Service de la en [Cloud Manager](https://my.cloudmanager.adobe.com) en el [detalles del entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). El nombre de región que se muestra en Cloud Manager puede ser [asignado al código de región](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) se utiliza en la API de Cloud Manager.

   __solicitud HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Habilitar la salida de puerto flexible para un programa de Cloud Manager mediante la API de Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. Utilice el adecuado `region` código obtenido de la API de Cloud Manager `listRegions` operación.

   __solicitud HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Espere 15 minutos para que el programa Cloud Manager aprovisione la infraestructura de red.

1. Compruebe que el entorno haya finalizado __salida de puerto flexible__ Configuración de mediante la API de Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) operación, usando el `id` devuelto por la solicitud HTTP createNetworkInfrastructure del paso anterior.

   __petición HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Compruebe que la respuesta HTTP contiene un __status__ de __ready__. Si aún no está listo, vuelva a comprobar el estado cada pocos minutos.

## Configuración de proxies de salida de puerto flexibles por entorno

1. Habilite y configure el __salida de puerto flexible__ AEM Configuración de en cada entorno as a Cloud Service de mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación.

   __enableEnvironmentAdvancedNetworkingConfiguration petición HTTP__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Defina los parámetros JSON en una `flexible-port-egress.json` y se proporciona para rizar mediante `... -d @./flexible-port-egress.json`.

   [Descargue el ejemplo flexible-port-egress.json](./assets/flexible-port-egress.json). Este archivo es solo un ejemplo. Configure el archivo según sea necesario en función de los campos opcionales/obligatorios documentados en [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   | Host de proxy | Puerto Proxy |  | Host externo | Puerto externo |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEM Si su implementación de la __solamente__ requiere conexiones HTTP/HTTPS (puerto 80/443) al servicio externo, deje el `portForwards` matriz vacía, ya que estas reglas solo son necesarias para solicitudes que no son HTTP/HTTPS.

1. Para cada entorno, valide que las reglas de salida estén en vigor mediante la API de Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación.

   __getEnvironmentAdvancedNetworkingConfiguration petición HTTP__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Las configuraciones de salida de puerto flexibles se pueden actualizar mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. Recordar `enableEnvironmentAdvancedNetworkingConfiguration` es un `PUT` , por lo que todas las reglas deben proporcionarse con cada invocación de esta operación.

1. AEM Ahora puede utilizar la configuración flexible de salida de puerto en el código de la personalizada y en la configuración de.


## Conexión a servicios externos mediante salida de puerto flexible

AEM Con el proxy de salida de puerto flexible habilitado, el código y la configuración de la pueden utilizarlos para realizar llamadas a servicios externos. AEM Existen dos tipos de llamadas externas que se tratan de manera diferente en el modo que se hace:

1. Llamadas HTTP/HTTPS a servicios externos en puertos no estándar
   + Incluye llamadas HTTP/HTTPS realizadas a servicios que se ejecutan en puertos que no son los puertos estándar 80 o 443.
1. llamadas no HTTP/HTTPS a servicios externos
   + Incluye cualquier llamada que no sea HTTP, como conexiones con servidores de correo, bases de datos SQL o servicios que se ejecutan en otros protocolos que no son HTTP/HTTPS.

AEM Las solicitudes HTTP/HTTPS de los puertos estándar (80/443) se permiten de forma predeterminada y no necesitan configuraciones ni consideraciones adicionales.


### HTTP/HTTPS en puertos no estándar

AEM Cuando se crean conexiones HTTP/HTTPS a puertos no estándar (no-80/443) desde la red de puertos, las conexiones deben realizarse a través de puertos y hosts especiales, proporcionados mediante marcadores de posición.

AEM AEM proporciona dos conjuntos de variables de sistema Java™ especiales que se asignan a proxies HTTP/HTTPS de.

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host de proxy para conexiones HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | Puerto proxy para conexiones HTTPS (establecer reserva en `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | Puerto proxy para conexiones HTTPS (establecer reserva en `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Cuando se realizan llamadas HTTP/HTTPS a servicios externos en puertos no estándar, no corresponde a `portForwards` debe definirse con la API de Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` operación, ya que las &quot;reglas&quot; de reenvío de puertos se definen &quot;en el código&quot;.

>[!TIP]
>
> AEM Consulte la documentación de salida de puerto flexible de la as a Cloud Service para [el conjunto completo de reglas de enrutamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### Ejemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="HTTP/HTTPS en puertos no estándar" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS en puertos no estándar</a></strong></div>
    <p>
        AEM Ejemplo de código Java™ que hace que la conexión HTTP/HTTPS se establezca de forma as a Cloud Service a un servicio externo en puertos HTTP/HTTPS no estándar.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Conexiones no HTTP/HTTPS a servicios externos

Al crear conexiones no HTTP/HTTPS (por ejemplo, AEM AEM SQL, SMTP, etc.) desde el punto de vista de la seguridad, la conexión debe realizarse a través de un nombre de host especial proporcionado por el usuario de la red de seguridad de la red de datos (SQL, SMTP, etc.) de la red de seguridad de la red de datos

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy para conexiones no HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


A continuación, se llama a las conexiones a servicios externos a través de `AEM_PROXY_HOST` y el puerto asignado (`portForwards.portOrig`AEM ), que luego enruta al nombre de host externo asignado (`portForwards.name`) y el puerto (`portForwards.portDest`).

| Host de proxy | Puerto Proxy |  | Host externo | Puerto externo |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Ejemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexión SQL con JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexión SQL con JDBC DataSourcePool</a></strong></div>
      <p>
            AEM Ejemplo de código Java™ conectarse a bases de datos SQL externas configurando el grupo de fuentes de datos JDBC de la.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexión SQL con API de Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexión SQL con las API de Java™</a></strong></div>
      <p>
            Ejemplo de código Java™ conectarse a bases de datos SQL externas mediante las API SQL de Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Red privada virtual (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servicio de correo electrónico</a></strong></div>
      <p>
        AEM Ejemplo de configuración de OSGi que utiliza la conexión con los servicios de correo electrónico externos mediante el uso de la.
      </p>
    </td>   
</tr></table>

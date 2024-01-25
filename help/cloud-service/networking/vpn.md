---
title: Red privada virtual (VPN)
description: AEM Aprenda a conectarse as a Cloud Service AEM con su VPN para crear canales de comunicación seguros entre los servicios internos y los de la red de servicios de red (VPNs) de la red social de.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
duration: 948
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 2%

---

# Red privada virtual (VPN)

AEM Aprenda a conectarse as a Cloud Service AEM con su VPN para crear canales de comunicación seguros entre los servicios internos y los de la red de servicios de red (VPNs) de la red social de.

## ¿Qué es la red privada virtual?

AEM La red privada virtual (VPN) permite que un cliente as a Cloud Service se conecte a un servicio de red privada virtual (VPN) **AEM el entorno de la** dentro de un programa de Cloud Manager a un existente, [compatible](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) VPN. AEM Esto permite conexiones seguras y controladas entre los servicios as a Cloud Service y los servicios dentro de la red del cliente.

Un programa de Cloud Manager solo puede tener un __soltero__ tipo de infraestructura de red. Asegúrese de que la red privada virtual sea la más activa [tipo adecuado de infraestructura de red](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#general-vpn-considerations) AEM para el recurso as a Cloud Service antes de ejecutar los siguientes comandos.

>[!NOTE]
>
>Tenga en cuenta que no se admite la conexión del entorno de compilación de Cloud Manager a una VPN. Si debe acceder a artefactos binarios desde un repositorio privado, debe configurar un repositorio seguro y protegido por contraseña con una URL disponible en la Internet pública [como se describe aquí](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> AEM Leer el recurso as a Cloud Service [documentación de configuración de red avanzada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) para obtener más información sobre la red privada virtual.

## Requisitos previos

Se requiere lo siguiente al configurar la red privada virtual:

+ Cuenta de Adobe con [Permisos de propietario de empresa de Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acceso a [Credenciales de autenticación de la API de Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID de organización (también conocido como ID de organización de IMS)
   + ID del cliente (también conocido como clave API)
   + Token de acceso (también conocido como Token de portador)
+ El ID de programa de Cloud Manager
+ Los ID de entorno de Cloud Manager
+ A **Basado en ruta** Red privada virtual, con acceso a todos los parámetros de conexión necesarios.

Para obtener más información, consulte el siguiente tutorial sobre cómo configurar y obtener credenciales de API de Cloud Manager y cómo utilizarlas para realizar una llamada de API de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Este tutorial utiliza `curl` para realizar las configuraciones de la API de Cloud Manager. El proporcionado `curl` Los comandos de asumen una sintaxis de Linux/macOS. Si utiliza el símbolo del sistema de Windows, reemplace la variable `\` salto de línea con `^`.

## Habilitar red privada virtual por programa

AEM Comience habilitando la red privada virtual en el as a Cloud Service de la red de.

1. En primer lugar, determine la región en la que se necesita la red avanzada mediante la API de Cloud Manager [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. El `region name` es necesario para realizar llamadas posteriores a la API de Cloud Manager. Normalmente, se utiliza la región en la que reside el entorno de producción.

   AEM Encuentre la región de su entorno as a Cloud Service de la en [Cloud Manager](https://my.cloudmanager.adobe.com) en el [detalles del entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). El nombre de región que se muestra en Cloud Manager puede ser [asignado al código de región](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) se utiliza en la API de Cloud Manager.

   __solicitud HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Habilitar la red privada virtual para un programa de Cloud Manager mediante las API de Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. Utilice el adecuado `region` código obtenido de la API de Cloud Manager `listRegions` operación.

   __solicitud HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Defina los parámetros JSON en una `vpn-create.json` y se proporciona para rizar mediante `... -d @./vpn-create.json`.

   [Descargue el ejemplo vpn-create.json](./assets/vpn-create.json).  Este archivo es solo un ejemplo. Configure el archivo según sea necesario en función de los campos opcionales/obligatorios documentados en [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
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

   Espere entre 45 y 60 minutos para que el programa Cloud Manager aprovisione la infraestructura de red.

1. Compruebe que el entorno haya finalizado __Red privada virtual__ Configuración de mediante la API de Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) operación, usando el `id` devuelto por la solicitud HTTP createNetworkInfrastructure del paso anterior.

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

1. Habilite y configure el __Red privada virtual__ AEM Configuración de en cada entorno as a Cloud Service de mediante la API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación.

   __enableEnvironmentAdvancedNetworkingConfiguration petición HTTP__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Defina los parámetros JSON en una `vpn-configure.json` y se proporciona para rizar mediante `... -d @./vpn-configure.json`.

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

   `nonProxyHosts` declara un conjunto de hosts para los que el puerto 80 o 443 debe enrutarse a través de los intervalos de direcciones IP compartidos predeterminados en lugar de la IP de salida dedicada. `nonProxyHosts` puede resultar útil, ya que la salida de tráfico a través de direcciones IP compartidas puede optimizarse aún más automáticamente mediante el Adobe.

   Para cada `portForwards` , la red avanzada define la siguiente regla de reenvío:

   | Host de proxy | Puerto Proxy |  | Host externo | Puerto externo |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEM Si su implementación de la __solamente__ requiere conexiones HTTP/HTTPS con un servicio externo, deje el `portForwards` matriz vacía, ya que estas reglas solo son necesarias para solicitudes que no son HTTP/HTTPS.


1. Para cada entorno, valide que las reglas de enrutamiento vpn están en vigor mediante las API de Cloud Manager [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación.

   __getEnvironmentAdvancedNetworkingConfiguration petición HTTP__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Las configuraciones de proxy de red privada virtual se pueden actualizar mediante las API de Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) operación. Recordar `enableEnvironmentAdvancedNetworkingConfiguration` es un `PUT` , por lo que todas las reglas deben proporcionarse con cada invocación de esta operación.

1. AEM Ahora puede utilizar la configuración de salida de red privada virtual en el código de y la configuración personalizados.

## Conexión a servicios externos a través de la red privada virtual

AEM Con la red privada virtual habilitada, el código y la configuración de la red privada virtual pueden utilizarlos para realizar llamadas a servicios externos a través de la VPN. AEM Existen dos tipos de llamadas externas que se tratan de manera diferente en el modo que se hace:

1. Llamadas HTTP/HTTPS a servicios externos
   + Incluye llamadas HTTP/HTTPS realizadas a servicios que se ejecutan en puertos que no son los puertos estándar 80 o 443.
1. llamadas no HTTP/HTTPS a servicios externos
   + Incluye cualquier llamada que no sea HTTP, como conexiones con servidores de correo, bases de datos SQL o servicios que se ejecutan en otros protocolos que no son HTTP/HTTPS.

AEM Las solicitudes HTTP/HTTPS de los puertos estándar (80/443) están permitidas de forma predeterminada, pero no utilizarán la conexión VPN si no se configura correctamente como se describe a continuación.

### HTTP/HTTPS

AEM AEM Cuando se crean conexiones HTTP/HTTPS desde la red de redes privadas virtuales (HTTP/HTTPS) desde la red de redes privadas virtuales (HTTP/HTTPS), las conexiones HTTP/HTTPS se procesan automáticamente como proxy fuera de la red de conexiones de red de red de red de red de área de trabajo (). No se requiere código ni configuración adicional para admitir conexiones HTTP/HTTPS.

>[!TIP]
>
> AEM Consulte la documentación de la red privada virtual de as a Cloud Service para [el conjunto completo de reglas de enrutamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### Ejemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        AEM Ejemplo de código Java™ que establece la conexión HTTP/HTTPS de manera as a Cloud Service a un servicio externo que utiliza el protocolo HTTP/HTTPS.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Ejemplos de código de conexiones no HTTP/HTTPS

Al crear conexiones no HTTP/HTTPS (por ejemplo, AEM AEM SQL, SMTP, etc.) desde el punto de vista de la seguridad, la conexión debe realizarse a través de un nombre de host especial proporcionado por el usuario de la red de seguridad de la red de datos (SQL, SMTP, etc.) de la red de seguridad de la red de datos

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy para conexiones no HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


A continuación, se llama a las conexiones a servicios externos a través de `AEM_PROXY_HOST` y el puerto asignado (`portForwards.portOrig`AEM ), que luego enruta al nombre de host externo asignado (`portForwards.name`) y el puerto (`portForwards.portDest`).

| Host de proxy | Puerto Proxy |  | Host externo | Puerto externo |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### Ejemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexión SQL con JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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

### AEM Limitar el acceso a los as a Cloud Service mediante VPN

AEM La configuración de Red privada virtual limita el acceso a entornos as a Cloud Service de la red virtual a una VPN.

#### Ejemplos de configuración

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Aplicación de una lista de permitidos IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Aplicación de una lista de permitidos IP</a></strong></div>
      <p>
            Configure una lista de permitidos AEM IP de modo que solo el tráfico VPN pueda acceder a los datos de acceso a la red de red (VPNs.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM Restricciones de acceso a VPN basadas en rutas para publicar en la" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM Restricciones de acceso a VPN basadas en rutas para publicar en la</a></strong></div>
      <p>
            AEM Requerir acceso VPN para rutas específicas en Publicación de.
      </p>
    </td>
   <td></td>
</tr></table>

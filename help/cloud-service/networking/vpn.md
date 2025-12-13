---
title: Red privada virtual (VPN)
description: Aprenda a conectar AEM as a Cloud Service con su VPN para crear canales de comunicación seguros entre AEM y los servicios internos.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
last-substantial-update: 2024-04-27T00:00:00Z
duration: 919
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1531'
ht-degree: 2%

---

# Red privada virtual (VPN)

Aprenda a conectar AEM as a Cloud Service con su VPN para crear canales de comunicación seguros entre AEM y los servicios internos.

>[!IMPORTANT]
>
>Puede configurar las VPN y el reenvío de puertos a través de la interfaz de usuario de Cloud Manager o mediante llamadas a la API. Este tutorial se centra en el método API.
>
>Si prefiere usar la interfaz de usuario, consulte [Configuración de redes avanzadas para AEM as a Cloud Service](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).

## ¿Qué es una red privada virtual?

La red privada virtual (VPN) permite que un cliente de AEM as a Cloud Service conecte **los entornos de AEM** dentro de un programa de Cloud Manager a una VPN existente [admitida](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking). La VPN permite conexiones seguras y controladas entre AEM as a Cloud Service y los servicios dentro de la red del cliente.

Un programa Cloud Manager solo puede tener un tipo de infraestructura de red __single__. Asegúrese de que Virtual Private Network sea el tipo de infraestructura de red [más apropiado](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) para su AEM as a Cloud Service antes de ejecutar los siguientes comandos.

>[!NOTE]
>
>Tenga en cuenta que no se admite la conexión del entorno de compilación de Cloud Manager a una VPN. Si debe acceder a artefactos binarios desde un repositorio privado, debe configurar un repositorio seguro y protegido por contraseña con una URL que esté disponible en el Internet público [tal como se describe aquí](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project).

>[!MORELIKETHIS]
>
> Lea la [documentación de configuración de red avanzada](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) de AEM as a Cloud Service para obtener más información sobre la red privada virtual.

## Requisitos previos

Se requiere lo siguiente al configurar una red privada virtual mediante las API de Cloud Manager:

+ Cuenta de Adobe con [permisos de propietario de Cloud Manager Business](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Acceso a [credenciales de autenticación de la API de Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + ID de organización (también conocido como ID de organización de IMS)
   + ID del cliente (también conocido como clave API)
   + Token de acceso (también conocido como Token de portador)
+ El ID del programa Cloud Manager
+ Los ID de entorno de Cloud Manager
+ Una red privada virtual **basada en rutas**, con acceso a todos los parámetros de conexión necesarios.

Para obtener más información [revise cómo configurar y obtener las credenciales de la API de Cloud Manager](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth) para utilizarlas en una llamada de la API de Cloud Manager.

>[!IMPORTANT]
>
>Este tutorial usa `curl` para realizar las configuraciones de la API de Cloud Manager—*si prefiere un enfoque programático*. Los comandos `curl` proporcionados suponen una sintaxis de Linux® o macOS. Si usa el símbolo del sistema de Windows, reemplace el carácter de salto de línea `\` por `^`.
>
>También puede completar la misma tarea a través de la interfaz de usuario de Cloud Manager. *Si prefiere el enfoque de la interfaz de usuario*, consulte [Configuración de redes avanzadas para AEM as a Cloud Service](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).

## Habilitar red privada virtual por programa

Comience habilitando la red privada virtual en AEM as a Cloud Service.


>[!BEGINTABS]

>[!TAB Cloud Manager]

La salida de puerto flexible se puede activar mediante Cloud Manager. Los siguientes pasos describen cómo habilitar la salida de puerto flexible en AEM as a Cloud Service mediante Cloud Manager.

1. Inicie sesión en [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/) como propietario de Cloud Manager Business.
1. Vaya al programa deseado.
1. En el menú de la izquierda, vaya a __Servicios > Infraestructura de red__.
1. Seleccione el botón __Agregar infraestructura de red__.

   ![Agregar infraestructura de red](./assets/cloud-manager__add-network-infrastructure.png)

1. En el cuadro de diálogo __Agregar infraestructura de red__, seleccione la opción __Red privada virtual__. Rellene los campos y seleccione __Continuar__. Póngase en contacto con el administrador de red de su organización para obtener los valores correctos.

   ![Agregar VPN](./assets/vpn/select-type.png)

1. Cree al menos una conexión VPN. Asigne un nombre significativo a la conexión y seleccione el botón __Agregar conexión__.

   ![Agregar conexión VPN](./assets/vpn/add-connection.png)

1. Configure la conexión VPN. Póngase en contacto con el administrador de red de su organización para obtener los valores correctos. Seleccione __Guardar__ para confirmar la adición de la conexión.

   ![Configurar conexión VPN](./assets/vpn/configure-connection.png)

1. Si se requieren varias conexiones VPN, agregue más conexiones según sea necesario. Cuando se agreguen todas las conexiones VPN, seleccione __Continuar__.

   ![Configurar conexión VPN](./assets/vpn/connections.png)

1. Seleccione __Guardar__ para confirmar la adición de la VPN y todas las conexiones configuradas.

   ![Confirmar creación de VPN](./assets/vpn/confirmation.png)

1. Espere a que se cree la infraestructura de red y se marque como __Listo__. Este proceso puede tardar hasta 1 hora.

   ![Estado de creación de VPN](./assets/vpn/creating.png)

Con la VPN creada, ahora puede configurarla mediante las API de Cloud Manager como se describe a continuación.

>[!TAB API de Cloud Manager]

La red privada virtual se puede habilitar mediante las API de Cloud Manager. Los siguientes pasos describen cómo habilitar una VPN en AEM as a Cloud Service mediante la API de Cloud Manager.

1. En primer lugar, determine la región en la que se necesita la conexión avanzada mediante la operación [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de la API de Cloud Manager. Se requiere `region name` para realizar llamadas subsiguientes a la API de Cloud Manager. Normalmente, se utiliza la región en la que reside el entorno de producción.

   Busque la región de su entorno de AEM as a Cloud Service en [Cloud Manager](https://my.cloudmanager.adobe.com) en los [detalles del entorno](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments). El nombre de región mostrado en Cloud Manager se puede [asignar al código de región](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) utilizado en la API de Cloud Manager.

   __solicitud HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Habilite la red privada virtual para un programa de Cloud Manager mediante la operación de las API de Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/). Utilice el código `region` apropiado obtenido de la operación de la API de Cloud Manager `listRegions`.

   __solicitud HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Defina los parámetros JSON en un `vpn-create.json` y proporcione para ondularse mediante `... -d @./vpn-create.json`.

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

1. Compruebe que el entorno haya finalizado la configuración de la __red privada virtual__ mediante la operación [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) de la API de Cloud Manager, utilizando la `id` devuelta desde la solicitud HTTP `createNetworkInfrastructure` del paso anterior.

   __petición HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Compruebe que la respuesta HTTP contiene un __estado__ de __listo__. Si aún no está listo, vuelva a comprobar el estado cada pocos minutos.


Con la VPN creada, ahora puede configurarla mediante las API de Cloud Manager como se describe a continuación.

>[!ENDTABS]

## Configuración de proxies de red privada virtual por entorno

1. Habilite y configure la configuración de __Red privada virtual__ en cada entorno de AEM as a Cloud Service mediante la operación [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de la API de Cloud Manager.

   __petición HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Defina los parámetros JSON en un `vpn-configure.json` y proporcione para ondularse mediante `... -d @./vpn-configure.json`.

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

   `nonProxyHosts` declara un conjunto de hosts para los que el puerto 80 o 443 debe enrutarse a través de los intervalos de direcciones IP compartidos predeterminados en lugar de la IP de salida dedicada. `nonProxyHosts` puede resultar útil ya que la salida de tráfico a través de direcciones IP compartidas se optimiza automáticamente con Adobe.

   Para cada asignación `portForwards`, la red avanzada define la siguiente regla de reenvío:

   | Host de proxy | Puerto Proxy |  | Host externo | Puerto externo |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Si su implementación de AEM __only__ requiere conexiones HTTP/HTTPS con un servicio externo, deje vacía la matriz `portForwards`, ya que estas reglas solo son necesarias para solicitudes que no sean HTTP/HTTPS.


&#x200B;2. Para cada entorno, valide que las reglas de enrutamiento VPN estén en vigor mediante la operación [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de la API de Cloud Manager.

   __petición HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

&#x200B;3. Las configuraciones de proxy de red privada virtual se pueden actualizar mediante la operación [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de la API de Cloud Manager. Recuerde que `enableEnvironmentAdvancedNetworkingConfiguration` es una operación de `PUT`, por lo que todas las reglas deben proporcionarse con cada invocación de esta operación.

&#x200B;4. Ahora puede utilizar la configuración de salida de red privada virtual en su código y configuración personalizados de AEM.

## Conexión a servicios externos a través de la red privada virtual

Con la red privada virtual habilitada, el código y la configuración de AEM pueden utilizarlos para realizar llamadas a servicios externos a través de la VPN. Hay dos tipos de llamadas externas que AEM trata de manera diferente:

1. Llamadas HTTP/HTTPS a servicios externos
   + Estos servicios externos incluyen llamadas HTTP/HTTPS realizadas a servicios que se ejecutan en puertos que no son los puertos estándar 80 o 443.
1. Llamadas no HTTP/HTTPS a servicios externos
   + Estos servicios externos incluyen cualquier llamada que no sea HTTP, como conexiones a servidores de correo, bases de datos SQL o servicios que utilizan protocolos distintos de HTTP/HTTPS.

Las solicitudes HTTP/HTTPS de AEM en puertos estándar (80/443) están permitidas de forma predeterminada, pero no utilizan la conexión VPN si no se configura correctamente como se describe a continuación.

### HTTP/HTTPS

Al crear conexiones HTTP/HTTPS desde AEM, al utilizar una VPN, las conexiones HTTP/HTTPS se procesan como proxy automáticamente fuera de AEM. No se requiere código ni configuración adicional para admitir conexiones HTTP/HTTPS.

>[!TIP]
>
> Consulte la documentación de la red privada virtual de AEM as a Cloud Service para [obtener el conjunto completo de reglas de enrutamiento](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking).

#### Ejemplos de código

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Ejemplo de código Java™ conexión HTTP/HTTPS de AEM as a Cloud Service a un servicio externo mediante el protocolo HTTP/HTTPS.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Ejemplos de código de conexiones no HTTP/HTTPS

Al crear conexiones no HTTP/HTTPS (por ejemplo, SQL, SMTP, etc.) de AEM, la conexión debe realizarse mediante un nombre de host especial proporcionado por AEM.

| Nombre de variable | Uso | Código Java™ | Configuración de OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Host proxy para conexiones no HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


A continuación, se llama a las conexiones a servicios externos a través de `AEM_PROXY_HOST` y del puerto asignado (`portForwards.portOrig`), que AEM enruta al nombre de host externo asignado (`portForwards.name`) y al puerto (`portForwards.portDest`).

| Host de proxy | Puerto Proxy |  | Host externo | Puerto externo |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### Ejemplos de código

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexión SQL con JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexión SQL con el conjunto de datos JDBC</a></strong></div>
      <p>
            Ejemplo de código Java™ conectarse a bases de datos SQL externas configurando el grupo de fuentes de datos JDBC de AEM.
      </p>
    </td>
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexión SQL con API de Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexión SQL con API de Java™</a></strong></div>
      <p>
            Ejemplo de código Java™ conectarse a bases de datos SQL externas mediante las API SQL de Java™.
      </p>
    </td>
   <td>
      <a  href="./examples/email-service.md"><img alt="Red privada virtual (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servicio de correo electrónico</a></strong></div>
      <p>
        Ejemplo de configuración de OSGi que utiliza AEM para conectarse a servicios de correo electrónico externos.
      </p>
    </td>
</tr></table>

### Limitar el acceso a AEM as a Cloud Service a través de la VPN

La configuración de Red privada virtual limita el acceso a los entornos de AEM as a Cloud Service a una VPN.

#### Ejemplos de configuración

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list"><img alt="Aplicación de una lista de permitidos IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list">Aplicando una lista de permitidos IP</a></strong></div>
      <p>
            Configure una lista de permitidos IP de modo que solo el tráfico VPN pueda acceder a AEM.
      </p>
    </td>
    </td>
   <td></td>
</tr></table>

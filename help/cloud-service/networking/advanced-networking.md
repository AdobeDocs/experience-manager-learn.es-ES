---
title: Red avanzada
description: Obtenga información sobre las opciones de red avanzadas de AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 3%

---

# Red avanzada

AEM as a Cloud Service proporciona funciones de red avanzadas que permiten una gestión precisa de las conexiones a y desde AEM programas as a Cloud Service.

|  | [Programas de producción](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programas del Simulador para pruebas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Admite redes avanzadas | š | ü |


AEM red avanzada consta de tres opciones para administrar la conectividad con servicios externos. Un programa de Cloud Manager y sus AEM entornos as a Cloud Service solo pueden utilizar un único tipo de configuración avanzada de red a la vez, por lo que debe asegurarse de que se selecciona el tipo más adecuado.

|  | HTTP/HTTPS en puertos estándar | HTTP/HTTPS en puertos no estándar | Conexiones no HTTP/HTTPS | IP de salida dedicada | Lista de &quot;hosts sin proxy&quot; | Conectarse a servicios protegidos por VPN | Limitar el tráfico de AEM Publish por IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Sin redes avanzadas__ | š | ü | ü | ü | ü | ü | ü |
| [__Salida de puerto flexible__](./flexible-port-egress.md) | š | š | š | ü | ü | ü | ü |
| [__Dirección IP de salida dedicada__](./dedicated-egress-ip-address.md) | š | š | š | š | š | ü | ü |
| [__Red privada virtual__](./vpn.md) | š | š | š | š | š | š | š |


Para obtener más información sobre las consideraciones que deben tenerse en cuenta al seleccionar el tipo de red avanzado adecuado, consulte [documentación de red avanzada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Tutoriales avanzados de red

Una vez identificada la opción de red avanzada más adecuada según las necesidades de su organización, haga clic en el tutorial correspondiente a continuación para obtener instrucciones paso a paso y ejemplos de código.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Salida de puerto flexible" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Salida de puerto flexible</a></strong></div>
      <p>
          Permitir tráfico as a Cloud Service AEM saliente en puertos no estándar.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Dirección IP de salida dedicada" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Dirección IP de salida dedicada</a></strong></div>
      <p>
        Origine tráfico AEM salida as a Cloud Service a partir de una IP dedicada.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Red privada virtual (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Red privada virtual (VPN)</a></strong></div>
      <p>
        Tráfico seguro entre una infraestructura de cliente o proveedor y AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Ejemplos de código

Esta colección proporciona ejemplos de la configuración y el código necesarios para aprovechar las funciones de red avanzadas para casos de uso específicos.

Asegúrese de que [configuración avanzada de redes](#advanced-networking) se ha configurado antes de seguir estos tutoriales.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Red privada virtual (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servicio de correo electrónico</a></strong></div>
      <p>
        Ejemplo de configuración de OSGi utilizando AEM para conectarse a servicios de correo electrónico externos.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Ejemplo de código Java™ que establece la conexión HTTP/HTTPS de AEM as a Cloud Service a un servicio externo mediante el protocolo HTTP/HTTPS.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexión SQL utilizando JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexión SQL utilizando JDBC DataSourcePool</a></strong></div>
      <p>
            Ejemplo de código Java™ conectándose a bases de datos SQL externas configurando AEM grupo de fuentes de datos JDBC.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexión SQL mediante API de Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexión SQL mediante API de Java™</a></strong></div>
      <p>
            Ejemplo de código de Java™ que se conecta a bases de datos SQL externas mediante las API SQL de Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Aplicación de una lista de permitidos IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Aplicación de una lista de permitidos IP</a></strong></div>
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
</tr>
</table>

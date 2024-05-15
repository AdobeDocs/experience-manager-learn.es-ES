---
title: Redes avanzadas
description: AEM Obtenga información acerca de las opciones avanzadas de red de as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 3%

---

# Redes avanzadas

AEM as a Cloud Service AEM proporciona funciones de red avanzadas que permiten una administración precisa de las conexiones desde y hacia programas as a Cloud Service de la.

|                                                   | [Programas de producción](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programas del Simulador para pruebas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Admite redes avanzadas | ✔ | ✘ |


AEM La red avanzada de redes consta de tres opciones para administrar la conectividad con servicios externos. AEM Un programa de Cloud Manager y sus entornos as a Cloud Service solo pueden utilizar un único tipo de configuración avanzada de red a la vez, por lo que debe asegurarse de que se selecciona el tipo más adecuado.

|                                   | HTTP/HTTPS en puertos estándar | HTTP/HTTPS en puertos no estándar | Conexiones que no son HTTP/HTTPS | IP de salida dedicada | Lista &quot;Hosts no proxy&quot; | Conexión a servicios protegidos por VPN | AEM Limitar tráfico de publicación por dirección IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Sin redes avanzadas__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Salida de puerto flexible__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Dirección IP de salida dedicada__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Red privada virtual__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Para obtener más información acerca de las consideraciones implicadas al seleccionar el tipo de red avanzado adecuado, consulte [documentación avanzada de redes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Tutoriales de red avanzados

Una vez identificada la opción de red avanzada más adecuada según las necesidades de su organización, haga clic en el tutorial correspondiente a para obtener instrucciones paso a paso y ejemplos de código.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Salida de puerto flexible" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Salida de puerto flexible</a></strong></div>
      <p>
          AEM Permitir el tráfico as a Cloud Service de salida en puertos no estándar.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Dirección IP de salida dedicada al archivo" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Dirección IP de salida dedicada</a></strong></div>
      <p>
        AEM Originar tráfico as a Cloud Service de salida de un destino desde una IP específica.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Red privada virtual (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Red privada virtual (VPN)</a></strong></div>
      <p>
        AEM Proteja el tráfico entre una infraestructura de cliente o proveedor y el servicio de asistencia as a Cloud Service a la.
      </p>
    </td>   
  </tr>
</table>

## Ejemplos de código

Esta colección proporciona ejemplos de la configuración y el código necesarios para aprovechar las funciones de red avanzadas para casos de uso específicos.

Asegúrese de que el adecuado [configuración avanzada de red](#advanced-networking) se ha configurado antes de seguir estos tutoriales.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Red privada virtual (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servicio de correo electrónico</a></strong></div>
      <p>
        AEM Ejemplo de configuración de OSGi que utiliza la conexión con los servicios de correo electrónico externos mediante el uso de la.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            AEM Ejemplo de código Java™ que establece la conexión HTTP/HTTPS de manera as a Cloud Service a un servicio externo que utiliza el protocolo HTTP/HTTPS.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexión SQL con JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexión SQL con JDBC DataSourcePool</a></strong></div>
      <p>
            AEM Ejemplo de código Java™ conectarse a bases de datos SQL externas configurando el grupo de fuentes de datos JDBC de la.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexión SQL con API de Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexión SQL con las API de Java™</a></strong></div>
      <p>
            Ejemplo de código Java™ conectarse a bases de datos SQL externas mediante las API SQL de Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="Aplicación de una lista de permitidos IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Aplicación de una lista de permitidos IP</a></strong></div>
      <p>
            Configure una lista de permitidos AEM IP de modo que solo el tráfico VPN pueda acceder a los datos de acceso a la red de red (VPNs.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM Restricciones de acceso a VPN basadas en rutas para publicar en la" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM Restricciones de acceso a VPN basadas en rutas para publicar en la</a></strong></div>
      <p>
            AEM Requerir acceso VPN para rutas específicas en Publicación de.
      </p>
    </td>
</tr>
</table>

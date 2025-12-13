---
title: Redes avanzadas
description: Obtenga información acerca de las opciones avanzadas de red de AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 85
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 4%

---

# Redes avanzadas

AEM as a Cloud Service proporciona funciones de red avanzadas que permiten una administración precisa de las conexiones desde y hacia programas de AEM as a Cloud Service.

|                                                   | [Programas de producción](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=es) | [Programas del Simulador para pruebas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=es) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Admite redes avanzadas | ✔ | ✘ |


La red avanzada de AEM consta de tres opciones para administrar la conectividad con servicios externos. Un programa de Cloud Manager y sus entornos de AEM as a Cloud Service solo pueden utilizar un único tipo de configuración avanzada de red a la vez, por lo que debe asegurarse de seleccionar el tipo más adecuado.

|                                   | HTTP/HTTPS en puertos estándar | HTTP/HTTPS en puertos no estándar | Conexiones que no son HTTP/HTTPS | IP de salida dedicada | Lista &quot;Hosts no proxy&quot; | Conexión a servicios protegidos por VPN | Limitar el tráfico de publicación de AEM por dirección IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Sin red avanzada__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Salida de puerto flexible__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Dirección IP de salida dedicada__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Red privada virtual__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Para obtener más información sobre las consideraciones que se deben tener en cuenta al seleccionar el tipo de red avanzada adecuado, consulte [documentación de red avanzada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=es).

## Tutoriales de red avanzados

Una vez identificada la opción de red avanzada más adecuada según las necesidades de su organización, haga clic en el tutorial correspondiente a para obtener instrucciones paso a paso y ejemplos de código.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Salida de puerto flexible" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Salida de puerto flexible</a></strong></div>
      <p>
          Permitir el tráfico de AEM as a Cloud Service saliente en puertos no estándar.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Dirección IP de salida dedicada al archivo" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Dirección IP de salida dedicada</a></strong></div>
      <p>
        Origine el tráfico saliente de AEM as a Cloud Service desde una IP dedicada.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Red privada virtual (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Red privada virtual (VPN)</a></strong></div>
      <p>
        Proteja el tráfico entre una infraestructura de cliente o proveedor y AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Ejemplos de código

Esta colección proporciona ejemplos de la configuración y el código necesarios para aprovechar las funciones de red avanzadas para casos de uso específicos.

Asegúrese de que la [configuración avanzada de red](#advanced-networking) adecuada se haya configurado antes de seguir estos tutoriales.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Red privada virtual (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Servicio de correo electrónico</a></strong></div>
      <p>
        Ejemplo de configuración de OSGi que utiliza AEM para conectarse a servicios de correo electrónico externos.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Ejemplo de código Java™ que establece una conexión HTTP/HTTPS desde AEM as a Cloud Service a un servicio externo mediante el protocolo HTTP/HTTPS.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Conexión SQL con JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Conexión SQL con el conjunto de datos JDBC</a></strong></div>
      <p>
            Ejemplo de código Java™ conectarse a bases de datos SQL externas configurando el grupo de fuentes de datos JDBC de AEM.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Conexión SQL con API de Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Conexión SQL con API de Java™</a></strong></div>
      <p>
            Ejemplo de código Java™ conectarse a bases de datos SQL externas mediante las API SQL de Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=es"><img alt="Aplicación de una lista de permitidos IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=es">Aplicando una lista de permitidos IP</a></strong></div>
      <p>
            Configure una lista de permitidos IP de modo que solo el tráfico VPN pueda acceder a AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=es#restrict-vpn-to-ingress-connections"><img alt="Restricciones de acceso VPN basadas en rutas a AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=es#restrict-vpn-to-ingress-connections">Restricciones de acceso VPN basadas en rutas a AEM Publish</a></strong></div>
      <p>
            Requerir acceso VPN para rutas específicas en AEM Publish.
      </p>
    </td>
</tr>
</table>

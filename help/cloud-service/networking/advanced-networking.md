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
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# Red avanzada

AEM as a Cloud Service ofrece tres opciones para administrar la conectividad con servicios externos. Un programa de Cloud Manager y sus AEM entornos as a Cloud Service solo pueden utilizar un único tipo de configuración avanzada de red a la vez, por lo que debe asegurarse de que se selecciona el tipo más adecuado.

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

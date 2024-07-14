---
title: Explicación de la prevención de DoS/DDoS
description: AEM Aprenda a evitar y mitigar los ataques DoS y DDoS contra los ataques de tipo de ataque de tipo de contra de la.
version: 6.5, Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# AEM Explicación de la prevención de DoS/DDoS en el ámbito de la

AEM Obtenga información acerca de las opciones disponibles para prevenir y mitigar los ataques DoS y DDoS en su entorno de trabajo de la comunidad de la que se está hablando. Antes de sumergirse en los mecanismos de prevención, obtenga información general sobre [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) y [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- Los ataques DoS (Denegación de servicio) y DDoS (Denegación de servicio distribuida) son intentos malintencionados de interrumpir el funcionamiento normal de un servidor, servicio o red de destino, lo que hace que sea inaccesible para los usuarios a los que va dirigido.
- Los ataques DoS suelen proceder de una sola fuente, mientras que los ataques DDoS proceden de varias fuentes.
- Los ataques DDoS suelen ser de mayor escala en comparación con los ataques DoS debido a los recursos combinados de múltiples dispositivos de ataque.
- Estos ataques se llevan a cabo inundando el objetivo con tráfico excesivo y explotando las vulnerabilidades en los protocolos de red.

En la tabla siguiente se describe cómo evitar y mitigar los ataques DoS y DDoS:

<table>
    <tbody>
        <tr>
            <td><strong>Mecanismo de prevención</strong></td>
            <td><strong>Descripción</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM,5 (AMS)</strong></td>
            <td><strong>AEM.5 (local)</strong></td>
        </tr>
        <tr>
            <td>Firewall de aplicaciones web (WAF)</td>
            <td>Una solución de seguridad diseñada para proteger las aplicaciones web de diversos tipos de ataques.</td>
            <td>
            <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">Licencia de protección WAF-DDoS</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> o <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF mediante contrato AMS.</td>
            <td>Su WAF preferido</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (también conocido como módulo Apache "mod_security") es una solución de código abierto y multiplataforma que proporciona protección contra una amplia gama de ataques contra aplicaciones web.<br/> En AEM as a Cloud Service AEM, esto solo es aplicable al servicio de Publish AEM, ya que no hay ningún servidor web Apache y no hay ningún servidor web de Apache que se encuentre frente a un servicio de autor de la aplicación, ya que no hay un servidor web de Apache que se encuentre frente a un servidor de Dispatcher AEM en el servicio de autor de la.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/es/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">Habilitar ModSecurity </a></td>
        </tr>
        <tr>
            <td>Reglas de filtro de tráfico</td>
            <td>Las reglas de filtro de tráfico se pueden utilizar para bloquear o permitir solicitudes en la capa de CDN.</td>
            <td><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Ejemplo de reglas de filtro de tráfico</a></td>
            <td>Funciones de limitación de reglas de <a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> o <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a>.</td>
            <td>Su solución preferida</td>
        </tr>
    </tbody>
</table>

## Análisis de incidentes y mejora continua de Post

Aunque no hay un flujo estándar único para identificar y prevenir ataques DoS/DDoS y depende del proceso de seguridad de su organización. El **análisis posterior al incidente y la mejora continua** es un paso crucial en el proceso. Estas son algunas prácticas recomendadas que debe tener en cuenta:

- Identifique la causa raíz del ataque DoS/DDoS realizando un análisis posterior al incidente, que incluya la revisión de los registros, el tráfico de red y las configuraciones del sistema.
- Mejorar los mecanismos de prevención sobre la base de los resultados del análisis posterior a los incidentes.


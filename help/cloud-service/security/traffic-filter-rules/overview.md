---
title: Protección de sitios web con reglas de filtro de tráfico (incluidas las reglas WAF)
description: Obtenga información acerca de las reglas de filtro de tráfico, incluida su subcategoría de reglas de firewall de aplicaciones web (WAF). Cómo crear, implementar y probar las reglas. AEM Además, analice los resultados para proteger los sitios de la.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
duration: 165
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 14%

---

# Protección de sitios web con reglas de filtro de tráfico (incluidas las reglas WAF)

Obtenga información acerca de **reglas de filtro de tráfico**, incluida su subcategoría de **reglas de firewall de aplicaciones web (WAF)** en AEM as a Cloud Service (AEMCS). Obtenga información sobre cómo crear, implementar y probar las reglas. AEM Además, analice los resultados para proteger los sitios de la.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Información general

Reducir el riesgo de violaciones de seguridad es una prioridad para cualquier organización. AEM CS ofrece la función de reglas de filtro de tráfico, incluidas las reglas WAF, para proteger sitios web y aplicaciones.

AEM Las reglas de filtro de tráfico se implementan en la [red de distribución de contenido (CDN) integrada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=es) y se evalúan antes de que la solicitud llegue a la infraestructura de la. AEM Con esta función, puede mejorar significativamente la seguridad del sitio web, lo que garantiza que solo se permita el acceso a la infraestructura de la red a las solicitudes legítimas.

Este tutorial le guía a través del proceso de creación, implementación, prueba y análisis de los resultados de las reglas de filtro de tráfico, incluidas las reglas WAF.

Puede obtener más información sobre las reglas de filtro de tráfico en [este artículo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> Una subcategoría de reglas de filtro de tráfico denominada &quot;reglas WAF&quot; requiere una licencia de protección WAF-DDoS o de seguridad mejorada.

Le invitamos a hacernos llegar sus comentarios o preguntas sobre las reglas de filtro de tráfico enviando un correo electrónico a **aemcs-waf-adopter@adobe.com**.

## Siguiente paso

Obtenga información sobre [cómo configurar](./how-to-setup.md) la característica para que pueda crear, implementar y probar reglas de filtro de tráfico. Obtenga información acerca de la configuración de las herramientas del panel de pila **Elasticsearch, Logstash y Kibana (ELK)** para analizar los resultados de los registros de CDN de AEM CS.



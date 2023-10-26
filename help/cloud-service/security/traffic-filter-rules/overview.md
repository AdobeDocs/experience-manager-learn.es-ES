---
title: Protección de sitios web con reglas de filtro de tráfico (incluidas las reglas WAF)
description: Obtenga información acerca de las reglas de filtro de tráfico, incluida su subcategoría de reglas de firewall de aplicaciones web (WAF). Cómo crear, implementar y probar las reglas. AEM Además, analice los resultados para proteger los sitios de la.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: fa28ae232a5353eb34788fd2abe8402b42a62f66
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 4%

---


# Protección de sitios web con reglas de filtro de tráfico (incluidas las reglas WAF)

Más información **reglas de filtro de tráfico**, incluida su subcategoría de **Reglas del cortafuegos de aplicaciones web (WAF)** AEM en as a Cloud Service (AEMCS). Obtenga información sobre cómo crear, implementar y probar las reglas. AEM Además, analice los resultados para proteger los sitios de la.

## Información general

Reducir el riesgo de violaciones de seguridad es una prioridad para cualquier organización. AEM CS ofrece la función de reglas de filtro de tráfico, incluidas las reglas WAF, para proteger sitios web y aplicaciones.

Las reglas de filtro de tráfico se implementan en [CDN integrada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=es) AEM y se evalúan antes de que la solicitud llegue a la infraestructura de la. AEM Con esta función, puede mejorar significativamente la seguridad del sitio web, lo que garantiza que solo se permita el acceso a la infraestructura de la red a las solicitudes legítimas.

Este tutorial le guía a través del proceso de creación, implementación, prueba y análisis de los resultados de las reglas de filtro de tráfico, incluidas las reglas WAF.

Puede obtener más información sobre las reglas de filtro de tráfico en [este artículo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en)

>[!IMPORTANT]
>
> Una subcategoría de reglas de filtro de tráfico denominada &quot;reglas WAF&quot; requiere una licencia de protección WAF-DDoS


## Siguiente paso

Aprender [cómo realizar la configuración](./how-to-setup.md) Utilice la función para poder crear, implementar y probar reglas de filtro de tráfico. Obtenga información acerca de la configuración de **Elasticsearch, Logstash y Kibana (ELK)** apilar herramientas del tablero para analizar los resultados de los registros de CDN de AEM CS.




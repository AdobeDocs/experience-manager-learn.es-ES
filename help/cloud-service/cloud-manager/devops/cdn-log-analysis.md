---
title: Herramientas de análisis de registro de CDN
description: Obtenga información acerca de la herramienta de análisis de registro de CDN de AEM Cloud Service que proporciona Adobe AEM y cómo ayuda a obtener información sobre el rendimiento de CDN y la implementación de la.
version: Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 4111ae0cf8777ce21c224991b8b1c66fb01041b3
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Herramientas de análisis de registro de CDN

Obtenga información acerca de la _Herramienta de análisis de registro de CDN de AEM Cloud Service_ que proporciona Adobe AEM y cómo le ayuda a obtener información sobre el rendimiento de CDN y la implementación de la.
 
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## Información general

La [Herramienta de análisis de registro de CDN de AEM as a Cloud Service](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) ofrece paneles pregenerados que puedes integrar con [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) o la [pila ELK](https://www.elastic.co/elastic-stack) para monitorizar y analizar en tiempo real los registros de CDN.

Con estas herramientas, puede conseguir una monitorización en tiempo real y una detección proactiva de problemas. De este modo, se garantiza una entrega de contenido optimizada y se aplican las medidas de seguridad adecuadas contra los ataques de denegación de servicio (DoS) y de denegación de servicio distribuido (DDoS).

## Funciones principales

- Análisis de registro optimizado
- Monitorización en tiempo real
- Integración perfecta
- Paneles para
   - Identificar posibles amenazas a la seguridad
   - Experiencia del usuario final más rápida

## Información general del panel

Para iniciar rápidamente el análisis de registro, Adobe proporciona paneles pregenerados para la pila de Splunk y ELK.

- **Proporción de aciertos de caché de CDN**: proporciona información sobre la proporción total de aciertos de caché y el recuento total de solicitudes por estado HIT, PASS y MISS. También proporciona las principales direcciones URL HIT, PASS y MISS.

  ![Proporción de aciertos de caché de CDN](assets/CHR-dashboard.png)

- **Tablero de tráfico de CDN**: proporciona información sobre el tráfico a través de la tasa de solicitudes de CDN y origen, tasas de error 4xx y 5xx, y solicitudes no almacenadas en caché. También proporciona el máximo de solicitudes de CDN y de origen por segundo por dirección IP del cliente y más perspectivas para optimizar las configuraciones de CDN.

  ![Tablero de tráfico de CDN](assets/Traffic-dashboard.png)

- **Panel WAF**: proporciona información a través de solicitudes analizadas, marcadas y bloqueadas. También proporciona ataques principales por ID de indicador WAF, los 100 principales atacantes por IP del cliente, país y agente de usuario y más perspectivas para optimizar las configuraciones de WAF.

  ![Tablero WAF](assets/WAF-Dashboard.png)

## Integración de Splunk

Para organizaciones que aprovechan [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) y que han habilitado el reenvío de registros de AEM CS a sus instancias de Splunk, pueden importar rápidamente paneles creados previamente. AEM Esta configuración facilita el análisis acelerado del registro y proporciona perspectivas procesables para optimizar las implementaciones de la y mitigar las amenazas a la seguridad, como los ataques DOS.

Puede empezar a utilizar la guía de [Paneles de Splunk para el análisis de registro de CDN de AEM CS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).


## Integración de ELK

La pila [ELK](https://www.elastic.co/elastic-stack), que incluye Elasticsearch, Logstash y Kibana, es otra opción poderosa para el análisis de registros. Resulta útil para las organizaciones que no tienen acceso a una configuración de Splunk o a las funcionalidades de reenvío de registros. La configuración de la pila ELK localmente es sencilla, la herramienta proporciona el archivo Docker Compose para comenzar rápidamente. A continuación, puede importar los paneles creados previamente e introducir los registros de CDN que se descargan mediante el Cloud Manager de Adobe.

Puede empezar a utilizar el contenedor de Docker [ELK para el análisis de registro de CDN de AEM CS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis).

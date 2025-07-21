---
title: 'Información general: Protección de sitios web de AEM'
description: Obtenga información sobre cómo proteger los sitios web de AEM de denegaciones de servicio, ataques DDoS y tráfico malintencionado mediante reglas de filtro de tráfico, incluida su subcategoría de reglas de cortafuegos de aplicaciones web (WAF) en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 22a35b008de380bf2f2ef5dfde6743261346df89
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---

# Información general: Protección de sitios web de AEM

Aprenda a proteger sus sitios web de AEM de ataques de denegación de servicio (DoS), denegación de servicio distribuido (DDoS), tráfico malintencionado y ataques sofisticados mediante **reglas de filtro de tráfico**, incluida su subcategoría de **reglas de firewall de aplicaciones web (WAF)** en AEM as a Cloud Service.

También puede obtener información sobre las diferencias entre el filtro de tráfico estándar y las reglas del filtro de tráfico de WAF, cuándo utilizarlas y cómo empezar a utilizar las reglas recomendadas por Adobe.

>[!IMPORTANT]
>
> Las reglas de filtro de tráfico de WAF requieren una licencia adicional de **Protección WAF-DDoS** o de **Seguridad mejorada**. Las reglas estándar de filtro de tráfico están disponibles para los clientes de Sites y Forms de forma predeterminada.


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## Introducción a la seguridad del tráfico en AEM as a Cloud Service

AEM as a Cloud Service aprovecha una capa de CDN integrada para proteger y optimizar el envío de su sitio web. Uno de los componentes más críticos de la capa de CDN es la capacidad de definir y aplicar reglas de tráfico. Estas reglas actúan como un escudo protector para ayudar a proteger su sitio del abuso, el uso indebido y los ataques, sin sacrificar el rendimiento.

La seguridad del tráfico es esencial para mantener el tiempo de actividad, proteger los datos confidenciales y garantizar una experiencia fluida para los usuarios legítimos. AEM proporciona dos categorías de reglas de seguridad:

- **Reglas estándar de filtro de tráfico**
- **Reglas de filtro de tráfico de Firewall de aplicaciones web (WAF)**

Los conjuntos de reglas ayudan a los clientes a evitar amenazas web comunes y sofisticadas, reducir el ruido de clientes malintencionados o que se comportan incorrectamente y mejorar la observabilidad mediante el registro de solicitudes, el bloqueo y la detección de patrones.

## Diferencia entre las reglas de filtro de tráfico estándar y de WAF

| Función | Reglas estándar de filtro de tráfico | Reglas del filtro de tráfico de WAF |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| Función | Evite abusos como denegaciones de servicio, ataques DDoS, raspado o actividad de bots | Detecte y reaccione ante patrones de ataque sofisticados (por ejemplo, OWASP Top 10), que también protege de bots |
| Ejemplos | Limitación de velocidad, bloqueo geográfico, filtrado de usuario-agente | Inyección SQL, XSS, IP de ataque conocidas |
| Flexibilidad | Altamente configurable mediante YAML | Altamente configurable mediante YAML, con indicadores WAF predefinidos |
| Modo recomendado | Comenzar con el modo `log` y luego pasar al modo `block` | Comience con el modo `block` para el marcador WAF `ATTACK-FROM-BAD-IP` y el modo `log` para el marcador WAF `ATTACK`, y después cambie al modo `block` para ambos |
| Implementación | Definido en YAML e implementado mediante la canalización de configuración de Cloud Manager | Definido en YAML con `wafFlags` e implementado mediante la canalización de configuración de Cloud Manager |
| Licencias | Incluido con licencias de Sites y Forms | **Requiere protección WAF-DDoS o licencia de seguridad mejorada** |

Las reglas estándar de filtro de tráfico son útiles para aplicar directivas específicas de la empresa, como límites de velocidad o bloqueo de regiones específicas, así como para bloquear el tráfico en función de propiedades de solicitud y encabezados como dirección IP, ruta o agente de usuario.
Las reglas del filtro de tráfico de WAF, por otro lado, proporcionan una protección proactiva completa para las vulnerabilidades web conocidas y los vectores de ataque, y tienen inteligencia avanzada para limitar los falsos positivos (es decir, bloquear el tráfico legítimo).
Para definir ambos tipos de reglas, utilice la sintaxis YAML; consulte [Sintaxis de reglas de filtro de tráfico](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax) para obtener más información.

## Cuándo y por qué utilizarlas

**Usar reglas estándar de filtro de tráfico** cuando:

- Desea aplicar límites específicos de organización, como la limitación de velocidad de IP.
- Tiene en cuenta patrones específicos (por ejemplo, direcciones IP, regiones y encabezados maliciosos) que deben filtrarse.

**Usar reglas de filtro de tráfico WAF** cuando:

- Desea una protección completa y **proactiva** frente a patrones de ataques conocidos y generalizados (por ejemplo, inyección o abuso de protocolo), así como IP malintencionadas conocidas, recopiladas a partir de fuentes de datos de expertos.
- Desea denegar las solicitudes malintencionadas y limitar al mismo tiempo la posibilidad de bloquear el tráfico legítimo.
- Desea limitar la cantidad de esfuerzo para defenderse de amenazas comunes y sofisticadas mediante la aplicación de reglas de configuración sencillas.

En conjunto, estas reglas proporcionan una estrategia de defensa exhaustiva que permite a los clientes de AEM as a Cloud Service tomar medidas proactivas y reactivas para garantizar sus propiedades digitales.

## Reglas recomendadas por Adobe

Adobe proporciona reglas recomendadas para el filtro de tráfico estándar y reglas de filtro de tráfico de WAF para ayudarle a proteger rápidamente sus sitios de AEM.

- **Reglas estándar de filtro de tráfico** (disponibles de forma predeterminada): Aborde escenarios comunes de abuso como ataques DoS, DDoS y bots contra **CDN edge**, **origin** o tráfico de países sancionados.\
  Algunos ejemplos son:
   - IP de limitación de velocidad que realizan más de 500 solicitudes por segundo _en el perímetro de CDN_
   - IP de limitación de velocidad que realizan más de 100 solicitudes/segundo _en el origen_
   - Bloqueo del tráfico de los países enumerados por la Oficina de Control de Assets Extranjero (OFAC)

- **Reglas de filtro de tráfico de WAF** (requiere licencia de complemento): proporciona protección adicional contra amenazas sofisticadas, incluidas las [diez principales amenazas de OWASP](https://owasp.org/www-project-top-ten/) como inyección de SQL, ejecución de scripts en sitios múltiples (XSS) y otros ataques de aplicaciones web.
Algunos ejemplos son:
   - Bloqueo de solicitudes de direcciones IP erróneas conocidas
   - Registro o bloqueo de solicitudes sospechosas marcadas como ataques

>[!TIP]
>
> Comience por aplicar las **reglas recomendadas por Adobe** para beneficiarse de la experiencia en seguridad y las actualizaciones en curso de Adobe. Si su empresa tiene riesgos específicos o casos extremos, o observa cualquier falso positivo (bloqueo del tráfico legítimo), puede definir **reglas personalizadas** o ampliar el conjunto predeterminado para satisfacer sus necesidades.

## Introducción

Obtenga información sobre cómo definir, implementar, probar y analizar las reglas de filtro de tráfico, incluidas las reglas de WAF, en AEM as a Cloud Service siguiendo la guía de configuración y los casos de uso que se indican a continuación. Esto le proporciona los conocimientos básicos para que pueda aplicar más adelante con seguridad las reglas recomendadas por Adobe.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Configuración de reglas de filtro de tráfico, incluidas las reglas de WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="Configuración de reglas de filtro de tráfico, incluidas las reglas de WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Configuración de reglas de filtro de tráfico, incluidas las reglas de WAF">Cómo configurar reglas de filtro de tráfico, incluidas las reglas de WAF</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo configurar para crear, implementar, probar y analizar los resultados de las reglas de filtro de tráfico, incluidas las reglas de WAF.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Empiece ahora</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Guía de configuración de reglas recomendadas por Adobe

Esta guía proporciona instrucciones paso a paso para configurar e implementar las reglas de filtro de tráfico estándar recomendadas por Adobe y las reglas de filtro de tráfico de WAF en su entorno de AEM as a Cloud Service.

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Protección de sitios web de AEM mediante reglas estándar de filtro de tráfico" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Protección de sitios web de AEM mediante reglas estándar de filtro de tráfico"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protección de sitios web de AEM mediante reglas estándar de filtro de tráfico">Protección de sitios web de AEM mediante reglas estándar de filtro de tráfico</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM del abuso de DoS, DDoS y bots mediante las reglas de filtro de tráfico estándar recomendadas por Adobe en AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aplicar reglas</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="Protección de sitios web de AEM mediante reglas de WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Protección de sitios web de AEM mediante reglas de WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protección de sitios web de AEM mediante reglas de WAF">Protección de sitios web de AEM mediante reglas de WAF</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM de amenazas sofisticadas, como denegaciones de servicio, ataques DDoS y abusos de bots, mediante las reglas de filtro de tráfico del cortafuegos de aplicaciones web (WAF) recomendadas por Adobe en AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Casos de uso avanzados

Para escenarios más avanzados, puede explorar los siguientes casos de uso que muestran cómo implementar reglas de filtro de tráfico personalizadas en función de requisitos comerciales específicos:

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="Supervisión de solicitudes confidenciales" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="Supervisión de solicitudes confidenciales"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="Supervisión de solicitudes confidenciales">Supervisión de solicitudes confidenciales</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo monitorizar solicitudes confidenciales registrándolas mediante reglas de filtro de tráfico en AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="Restricción del acceso" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="Restricción del acceso"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="Restricción del acceso">Restricción del acceso</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo restringir el acceso bloqueando solicitudes específicas mediante reglas de filtro de tráfico en AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="Normalización de solicitudes" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="Normalización de solicitudes"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="Normalización de solicitudes">Normalizando solicitudes</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo normalizar solicitudes transformándolas mediante reglas de filtro de tráfico en AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Recursos adicionales

- [Reglas De Filtro De Tráfico Que Incluyen Reglas De WAF](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)

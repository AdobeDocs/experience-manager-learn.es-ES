---
title: 'Información general: Protección de sitios web de AEM'
description: Obtenga información sobre cómo proteger los sitios web de AEM frente a DoS, DDoS y tráfico malintencionado mediante las reglas de filtro de tráfico, incluida la subcategoría de reglas WAF (Web Application Firewall) en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 7b29187ef84bebebd4586374abb09ced947dff28
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 98%

---

# Información general: Protección de sitios web de AEM

Aprenda a proteger sus sitios web de AEM frente a ataques de denegación de servicio (DoS), denegación de servicio distribuido (DDoS), tráfico malintencionado y ataques sofisticados mediante las **reglas de filtro de tráfico**, incluida la subcategoría de reglas **WAF (Web Application Firewall)** en AEM as a Cloud Service.

También puede obtener información sobre las diferencias entre el filtro de tráfico estándar y las reglas del filtro de tráfico WAF, cuándo utilizarlas y cómo empezar a utilizar las reglas recomendadas por Adobe.

>[!IMPORTANT]
>
> Las reglas de filtro de tráfico de WAF requieren una licencia adicional de seguridad extendida (anteriormente denominada protección WAF-DDoS) o seguridad extendida para atención médica (anteriormente denominada seguridad mejorada). Las reglas de filtro de tráfico estándar están disponibles para los clientes de Sites y Forms de forma predeterminada.


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## Introducción a la seguridad del tráfico en AEM as a Cloud Service

AEM as a Cloud Service aprovecha una capa de CDN integrada para proteger y optimizar el envío de su sitio web. Uno de los componentes más cruciales de la capa de CDN es la posibilidad de definir y aplicar reglas de tráfico. Estas reglas actúan como un escudo protector para ayudar a proteger su sitio frente al abuso, el uso indebido y los ataques, sin sacrificar el rendimiento.

La seguridad del tráfico es esencial para mantener el tiempo de actividad, proteger los datos confidenciales y garantizar una experiencia fluida para los usuarios legítimos. AEM proporciona dos categorías de reglas de seguridad:

- **Reglas de filtro de tráfico estándar**
- **Reglas de filtro de tráfico WAF (Web Application Firewall)**

Los conjuntos de reglas ayudan a los clientes a evitar amenazas web comunes y sofisticadas, reducir el ruido de clientes malintencionados o que se comportan incorrectamente y mejorar la observabilidad mediante el registro de solicitudes, el bloqueo y la detección de patrones.

## Diferencia entre las reglas de filtro de tráfico estándar y WAF

| Función | Reglas de filtro de tráfico estándar | Reglas de filtro de tráfico WAF |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| Función | Evitar abusos como DoS, DDoS, la extracción o actividades de bots | Detectar y reaccionar ante patrones de ataque sofisticados (por ejemplo, OWASP Top 10), que también protege de los bots |
| Ejemplos | Limitación de velocidad, bloqueo geográfico, filtrado de usuario-agente | Inyección SQL, XSS, direcciones IP de ataque conocidas |
| Flexibilidad | Muy configurable mediante YAML | Muy configurable mediante YAML, con indicadores WAF predefinidos |
| Modo recomendado | Comenzar con el modo `log` y luego pasar al modo `block` | Comenzar con el modo `block` para el indicador WAF `ATTACK-FROM-BAD-IP` y el modo `log` para el indicador WAF `ATTACK`, y después cambiar al modo `block` para ambos |
| Implementación | Definido en YAML e implementado mediante la canalización de configuración de Cloud Manager | Definido en YAML con `wafFlags` e implementado mediante la canalización de configuración de Cloud Manager |
| Licencias | Incluido con las licencias de Sites y Forms | **Requiere una licencia de protección WAF-DDoS o de seguridad mejorada** |

Las reglas de filtro de tráfico estándar son útiles para aplicar directivas específicas de la empresa, como los límites de velocidad o el bloqueo de regiones específicas, así como para bloquear el tráfico en función de propiedades de solicitud y encabezados como la dirección IP, la ruta o el agente de usuario.
Las reglas del filtro de tráfico WAF, por otro lado, proporcionan una protección proactiva completa frente a las vulnerabilidades de seguridad web y los vectores de ataque conocidos, y cuentan con inteligencia avanzada para limitar los falsos positivos (es decir, bloquear el tráfico legítimo).
Para definir ambos tipos de reglas, utilice la sintaxis YAML; consulte [Sintaxis de reglas de filtro de tráfico](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax) para obtener más información.

## Cuándo y por qué utilizarlas

**Use las reglas estándar de filtro de tráfico** cuando:

- Desee aplicar límites específicos de la organización, como la limitación de velocidad de IP.
- Tenga en cuenta patrones específicos (por ejemplo, direcciones IP, regiones y encabezados maliciosos) que deben filtrarse.

**Use las reglas de filtro de tráfico WAF** cuando:

- Desee una protección completa y **proactiva** frente a patrones de ataques conocidos y generalizados (por ejemplo, inyección o abuso de protocolo), así como direcciones IP malintencionadas conocidas, recopiladas a partir de fuentes de datos de expertos.
- Desee denegar las solicitudes malintencionadas y limitar al mismo tiempo la posibilidad de bloquear el tráfico legítimo.
- Desee limitar la cantidad de esfuerzo para defenderse ante amenazas comunes y sofisticadas mediante la aplicación de reglas de configuración sencillas.

En conjunto, estas reglas proporcionan una estrategia de defensa exhaustiva que permite a los clientes de AEM as a Cloud Service tomar medidas proactivas y reactivas para garantizar sus propiedades digitales.

## Reglas recomendadas por Adobe

Adobe proporciona las reglas recomendadas para las reglas de filtro de tráfico estándar y de filtro de tráfico WAF que le ayudan a proteger rápidamente su AEM Sites.

- **Reglas de filtro de tráfico estándar** (disponibles de forma predeterminada): abordan escenarios comunes de abuso como los ataques DoS, DDoS y bots contra el **borde de CDN**, el **origen** o el tráfico procedente de países sancionados.\
  Algunos ejemplos son:
   - Direcciones IP de limitación de velocidad que realizan más de 500 solicitudes por segundo _en el borde de la CDN_
   - Direcciones IP de limitación de velocidad que realizan más de 100 solicitudes/segundo _en el origen_
   - Bloqueo del tráfico procedente de los países indicados por la OFAC (Office of Foreign Assets Control)

- **Reglas de filtro de tráfico WAF** (requiere licencia de complemento): proporciona protección adicional frente a amenazas sofisticadas, incluidas las de [OWASP Top Ten](https://owasp.org/www-project-top-ten/) como la inyección de SQL, la ejecución de scripts en sitios múltiples (XSS) y otros ataques de aplicaciones web.
Algunos ejemplos son:
   - Bloqueo de solicitudes de direcciones IP erróneas conocidas
   - Registro o bloqueo de solicitudes sospechosas marcadas como ataques

>[!TIP]
>
> Comience por aplicar las **reglas recomendadas por Adobe** para beneficiarse de la experiencia en seguridad y las actualizaciones en curso de Adobe. Si su empresa tiene riesgos específicos o casos extremos, o detecta falsos positivos (bloqueo del tráfico legítimo), puede definir **reglas personalizadas** o ampliar el conjunto predeterminado para satisfacer sus necesidades.

## Introducción

Obtenga información sobre cómo definir, implementar, probar y analizar las reglas de filtro de tráfico, incluidas las reglas WAF, en AEM as a Cloud Service siguiendo la guía de configuración y los casos de uso que se indican a continuación. Esto le proporciona los conocimientos básicos para que pueda aplicar más adelante con seguridad las reglas recomendadas por Adobe.

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
                    <a href="./setup.md" title="Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF">Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo crear, implementar, probar y analizar los resultados de las reglas de filtro de tráfico, incluidas las reglas WAF.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Iniciar ahora</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Guía de configuración de las reglas recomendadas por Adobe

Esta guía proporciona instrucciones paso a paso para configurar e implementar las reglas de filtro de tráfico estándar recomendadas por Adobe y las reglas de filtro de tráfico WAF en su entorno de AEM as a Cloud Service.

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
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar">Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM frente a ataques DoS, DDoS y el abuso de bots mediante las reglas de filtro de tráfico estándar recomendadas por Adobe en AEM as a Cloud Service.</p>
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
                    <a href="./use-cases/using-waf-rules.md" title="Protección de sitios web de AEM mediante reglas WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Protección de sitios web de AEM mediante reglas WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protección de sitios web de AEM mediante reglas WAF">Protección de sitios web de AEM mediante reglas WAF</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM frente a amenazas sofisticadas, como DoS, DDoS y el abuso de bots, mediante las reglas de filtro de tráfico WAF (Web Application Firewall) recomendadas por Adobe en AEM as a Cloud Service.</p>
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

En el caso de los escenarios más avanzados, puede explorar los siguientes casos de uso que muestran cómo implementar reglas de filtro de tráfico personalizadas en función de requisitos comerciales específicos:

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
                    <a href="./how-to/request-logging.md" title="Monitorización de solicitudes confidenciales" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="Monitorización de solicitudes confidenciales"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="Monitorización de solicitudes confidenciales">Monitorización de solicitudes confidenciales</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo monitorizar solicitudes confidenciales registrándolas mediante las reglas de filtro de tráfico de AEM as a Cloud Service.</p>
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
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="Normalización de solicitudes">Normalización de solicitudes</a>
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

- [Reglas de filtro de tráfico, incluidas las reglas WAF](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)

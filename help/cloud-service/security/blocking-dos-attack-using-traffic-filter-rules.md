---
title: Bloqueo de ataques DoS, DDoS y ataques sofisticados mediante reglas de filtro de tráfico
description: Obtenga información sobre cómo bloquear ataques DoS, DDoS y sofisticados mediante las reglas de filtro de tráfico en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 100%

---

# Bloqueo de ataques DoS, DDoS y ataques sofisticados mediante reglas de filtro de tráfico

Aprenda a bloquear los ataques de denegación de servicio (DoS), denegación de servicio distribuido (DDoS) y ataques sofisticados mediante las **reglas de filtro de tráfico** en la CDN administrada por AEM as a Cloud Service (AEMCS).

Estos ataques causan picos de tráfico en la CDN y potencialmente en el servicio de AEM Publish (el origen) y pueden afectar a la capacidad de respuesta y disponibilidad del sitio.

Este artículo proporciona información general sobre las protecciones predeterminadas de su sitio web de AEM y cómo ampliar esas protecciones mediante la configuración del cliente. También describe cómo analizar los patrones de tráfico y configurar las reglas de filtro de tráfico estándar para bloquear esos ataques.

## Protecciones predeterminadas en AEM as a Cloud Service

Conozcamos las protecciones predeterminadas DDoS de su sitio web de AEM:

- **Almacenamiento en caché:** con unas buenas políticas de almacenamiento en caché, el impacto de un ataque DDoS es más limitado porque la CDN evita que la mayoría de las solicitudes vayan al origen y provoquen una degradación del rendimiento.
- **Escalado automático:** los servicios de autor y publicación de AEM se escalan automáticamente para controlar los picos de tráfico, aunque pueden verse afectados por aumentos repentinos y masivos del tráfico.
- **Bloqueo:** la CDN de Adobe bloquea el tráfico en el origen si supera una frecuencia definida por Adobe desde una dirección IP en particular, por cada PoP (punto de presencia) de la CDN.
- **Alertas:** el Centro de acciones envía un pico de tráfico en la notificación de alerta de origen cuando el tráfico supera una determinada frecuencia. Esta alerta se activa cuando el tráfico a cualquier PoP de CDN determinada supera una frecuencia de solicitud _definida por Adobe_ por cada dirección IP. Consulte [Alertas sobre reglas de filtro de tráfico](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) para obtener más información.

Estas protecciones integradas deben considerarse una línea de base para que una organización pueda minimizar el impacto de un ataque DDoS en el rendimiento. Dado que cada sitio web presenta diferentes características de rendimiento y puede detectar esa degradación del rendimiento antes de que se cumpla el límite de frecuencia definido por Adobe, se recomienda ampliar las protecciones predeterminadas mediante la _configuración del cliente_.

## Ampliación de la protección con reglas de filtro de tráfico

Examinemos algunas medidas adicionales recomendadas que los clientes pueden tomar para proteger sus sitios web frente a ataques DDoS:

- Implemente las [reglas de filtro de tráfico estándar](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md) recomendadas por Adobe para identificar patrones de tráfico potencialmente maliciosos mediante el registro y las alertas de comportamientos sospechosos.
- Use el complemento **Protección WAF-DDoS** o **Seguridad mejorada** e implemente las [reglas de filtro de tráfico WAF](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md) recomendadas por Adobe para defenderse de ataques sofisticados, incluidos los que utilizan técnicas avanzadas basadas en protocolos o cargas útiles.
- Aumente la cobertura de la caché configurando las [transformaciones de solicitud](./traffic-filter-and-waf-rules/how-to/request-transformation.md) para omitir los parámetros de consulta que no son necesarios.

## Introducción

Explore los siguientes tutoriales para configurar las reglas recomendadas por Adobe para bloquear ataques.

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF">Cómo configurar las reglas de filtro de tráfico, incluidas las reglas WAF</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo crear, implementar, probar y analizar los resultados de las reglas de filtro de tráfico, incluidas las reglas WAF.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Iniciar ahora</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar">Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM frente a ataques DoS, DDoS y el abuso de bots mediante las reglas de filtro de tráfico estándar recomendadas por Adobe en AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aplicar reglas</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="Protección de sitios web de AEM mediante las reglas de filtro de tráfico WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="Protección de sitios web de AEM mediante las reglas de filtro de tráfico WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protección de sitios web de AEM mediante las reglas de filtro de tráfico WAF">Protección de sitios web de AEM mediante reglas de filtro de tráfico WAF</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM frente a amenazas sofisticadas, como DoS, DDoS y el abuso de bots, mediante las reglas de filtro de tráfico WAF (Web Application Firewall) recomendadas por Adobe en AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

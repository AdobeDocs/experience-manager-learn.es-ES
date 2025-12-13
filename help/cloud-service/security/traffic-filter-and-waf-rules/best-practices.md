---
title: Prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF
description: Conozca las prácticas recomendadas para configurar las reglas de filtro de tráfico, incluidas las reglas WAF en AEM as a Cloud Service, para mejorar la seguridad y mitigar los riesgos.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 100%

---

# Prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF

Conozca las prácticas recomendadas para configurar las reglas de filtro de tráfico, incluidas las reglas WAF en AEM as a Cloud Service, para mejorar la seguridad y mitigar los riesgos.

>[!IMPORTANT]
>
>Las prácticas recomendadas que se describen en este artículo no son exhaustivas y no pretenden sustituir a sus propias políticas y procedimientos de seguridad.

## Prácticas recomendadas generales

- Comience con el [conjunto recomendado](./overview.md#adobe-recommended-rules) de reglas de filtro de tráfico estándar y WAF proporcionadas por Adobe y ajústelas en función de las necesidades específicas de su aplicación y el panorama de amenazas.
- Colabore con su equipo de seguridad para determinar qué reglas se alinean con la postura de seguridad y los requisitos de cumplimiento de su organización.
- Pruebe siempre reglas nuevas o actualizadas en entornos de desarrollo antes de promocionarlas a entornos de ensayo y producción.
- Al declarar y validar reglas, comience por el tipo de `action` `log` para observar el comportamiento sin bloquear el tráfico legítimo.
- Muévase de `log` a `block` solo después de analizar suficientes datos de tráfico y confirmar que no haya solicitudes válidas afectadas.
- Introduzca reglas de forma gradual, que incluyan equipos de pruebas de control de calidad, rendimiento y seguridad para identificar los efectos secundarios no deseados.
- Revise y analice con regularidad la eficacia de las reglas mediante las [herramientas del panel de control](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). La frecuencia de la revisión (diaria, semanal, mensual) debe coincidir con el volumen de tráfico y el perfil de riesgo del sitio.
- Perfeccione continuamente las reglas en función de nuevos datos de inteligencia de amenazas, comportamiento del tráfico y resultados de la auditoría.

## Prácticas recomendadas para reglas de filtros de tráfico

- Use las [reglas de filtro de tráfico estándar recomendadas por Adobe](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) como línea de base, que incluye reglas para el borde, la protección del origen y las restricciones basadas en OFAC.
- Revise las alertas y los registros regularmente para identificar patrones de abuso o una configuración incorrecta.
- Ajuste los valores de umbral para los límites de velocidad en función de los patrones de tráfico de la aplicación y el comportamiento del usuario.

  Consulte la siguiente tabla para obtener instrucciones sobre cómo elegir los valores de umbral:

  | Variación | Valor |
  | :--------- | :------- |
  | Origen | Tome el valor más alto del número máximo de solicitudes de origen por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |
  | Edge | Tome el valor más alto del número máximo de solicitudes Edge por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |

  Consulte también la sección [Selección de valores de umbral](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) para obtener más información.

- Muévase a la acción `block` solo después de confirmar que la acción `log` no afecta al tráfico legítimo.

## Prácticas recomendadas para reglas WAF

- Comience con las [reglas WAF recomendadas](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) por Adobe, que incluyen reglas para bloquear direcciones IP incorrectas conocidas, detectar ataques DDoS y mitigar el abuso de bots.
- El indicador de WAF `ATTACK` debe alertarle sobre posibles amenazas. Asegúrese de que no haya falsos positivos antes de pasar a `block`.
- Si las reglas WAF recomendadas no cubren amenazas específicas, considere la posibilidad de crear reglas personalizadas basadas en los requisitos únicos de su aplicación. Consulte la lista completa de [indicadores de WAF](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) en la documentación.

## Implementación de reglas

Obtenga información sobre cómo implementar reglas de filtro de tráfico y reglas WAF en AEM as a Cloud Service:

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
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
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM frente a amenazas sofisticadas, como DoS, DDoS y abusos de bots, mediante las reglas WAF (Web Application Firewall) recomendadas por Adobe en AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Recursos adicionales

- [Reglas de filtro de tráfico, incluidas las reglas WAF](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Explicación de la prevención de DoS/DDoS en AEM](https://experienceleague.adobe.com/es/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Bloqueo de ataques DoS y DDoS mediante reglas de filtro de tráfico](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

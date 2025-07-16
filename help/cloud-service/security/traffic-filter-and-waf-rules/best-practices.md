---
title: Prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF
description: Conozca las prácticas recomendadas para configurar reglas de filtro de tráfico, incluidas las reglas de WAF en AEM as a Cloud Service, para mejorar la seguridad y mitigar los riesgos.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# Prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF

Conozca las prácticas recomendadas para configurar reglas de filtro de tráfico, incluidas las reglas de WAF en AEM as a Cloud Service, para mejorar la seguridad y mitigar los riesgos.

>[!IMPORTANT]
>
>Las prácticas recomendadas descritas en este artículo no son exhaustivas y no pretenden sustituir a sus propias políticas y procedimientos de seguridad.

## Prácticas recomendadas generales

- Comience con el [conjunto recomendado](./overview.md#adobe-recommended-rules) de reglas estándar de filtro de tráfico y WAF proporcionadas por Adobe y ajústelas en función de las necesidades específicas de su aplicación y el panorama de amenazas.
- Colabore con su equipo de seguridad para determinar qué reglas se alinean con la postura de seguridad y los requisitos de cumplimiento de su organización.
- Pruebe siempre reglas nuevas o actualizadas en entornos de desarrollo antes de promocionarlas a Ensayo y Producción.
- Al declarar y validar reglas, comience por el tipo `action` `log` para observar el comportamiento sin bloquear el tráfico legítimo.
- Moverse de `log` a `block` solo después de analizar suficientes datos de tráfico y confirmar que no hay solicitudes válidas afectadas.
- Introduzca reglas de forma gradual, que incluyan equipos de pruebas de control de calidad, rendimiento y seguridad para identificar efectos secundarios no deseados.
- Revise y analice con regularidad la eficacia de las reglas mediante [herramientas de tablero](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). La frecuencia de la revisión (diaria, semanal, mensual) debe coincidir con el volumen de tráfico y el perfil de riesgo del sitio.
- Restringir continuamente las reglas en función de nuevos datos de inteligencia de amenazas, comportamiento del tráfico y resultados de auditoría.

## Prácticas recomendadas para reglas de filtros de tráfico

- Use las reglas de filtro de tráfico estándar recomendadas por Adobe [1&rbrace; como línea de base, que incluye reglas para Edge, protección de origen y restricciones basadas en OFAC.](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)
- Revise las alertas y los registros regularmente para identificar patrones de abuso o configuración incorrecta.
- Ajuste los valores de umbral para los límites de tasa en función de los patrones de tráfico de la aplicación y el comportamiento del usuario.

  Consulte la siguiente tabla para obtener instrucciones sobre cómo elegir los valores de umbral:

  | Variación | Valor |
  | :--------- | :------- |
  | Origen | Tome el valor más alto del número máximo de solicitudes de origen por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |
  | Edge | Tome el valor más alto del número máximo de solicitudes Edge por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |

  Consulte también la sección [elección de valores de umbral](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) para obtener más información.

- Mover a la acción `block` solo después de confirmar que la acción `log` no afecta al tráfico legítimo.

## Prácticas recomendadas para reglas WAF

- Comience con las [reglas recomendadas de WAF](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) de Adobe, que incluyen reglas para bloquear direcciones IP incorrectas conocidas, detectar ataques DDoS y mitigar el abuso de bots.
- El indicador de WAF `ATTACK` debe alertarle sobre posibles amenazas. Asegúrese de que no haya falsos positivos antes de pasar a `block`.
- Si las reglas de WAF recomendadas no cubren amenazas específicas, considere la posibilidad de crear reglas personalizadas basadas en los requisitos únicos de su aplicación. Consulte la lista completa de [indicadores de WAF](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) en la documentación.

## Implementación de reglas

Obtenga información sobre cómo implementar reglas de filtro de tráfico y reglas de WAF en AEM as a Cloud Service:

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
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM de amenazas sofisticadas, como denegaciones de servicio, ataques DDoS y abusos de bots, mediante las reglas de cortafuegos de aplicaciones web (WAF) recomendadas por Adobe en AEM as a Cloud Service.</p>
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

- [Reglas De Filtro De Tráfico Que Incluyen Reglas De WAF](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Explicación de la prevención de DoS/DDoS en AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Bloqueo de ataques DoS y DDoS mediante reglas de filtro de tráfico](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)


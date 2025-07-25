---
title: Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar
description: Obtenga información sobre cómo proteger los sitios web de AEM frente a la Denegación de servicio (DoS) y el abuso de bots mediante las reglas de filtro de tráfico estándar recomendadas por Adobe en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18307
thumbnail: null
exl-id: 5e235220-82f6-46e4-b64d-315f027a7024
source-git-commit: b7f567da159865ff04cb7e9bd4dae0b140048e7d
workflow-type: ht
source-wordcount: '1780'
ht-degree: 100%

---

# Protección de sitios web de AEM mediante reglas de filtro de tráfico estándar

Aprenda a proteger los sitios web de AEM frente a la denegación de servicio (DoS), la denegación de servicio distribuido (DDoS) y el abuso de bots mediante las _reglas de filtro de tráfico estándar recomendadas por Adobe_**en AEM as a Cloud Service.**


>[!VIDEO](https://video.tv.adobe.com/v/3469396/?quality=12&learn=on)

## Objetivos de aprendizaje

- Revise las reglas de filtro de tráfico estándar recomendadas por Adobe.
- Defina, implemente, pruebe y analice los resultados de las reglas.
- Conozca cuándo y cómo perfeccionar las reglas en función de los patrones de tráfico.
- Aprenda a utilizar el centro de acciones de AEM para revisar las alertas generadas por las reglas.

### Información general sobre la implementación

Los pasos de implementación incluyen los siguientes:

- Añadir las reglas de filtro de tráfico estándar al archivo `/config/cdn.yaml` del proyecto WKND de AEM.
- Confirmar e insertar los cambios en el repositorio de Git de Cloud Manager.
- Implementar los cambios en el entorno de AEM mediante la canalización de configuración de Cloud Manager.
- Probar las reglas simulando un ataque DoS con [Vegeta](https://github.com/tsenart/vegeta)
- Analizar los resultados mediante los registros de CDN de AEMCS y la herramienta del panel de control ELK.

## Requisitos previos

Antes de continuar, asegúrese de haber completado los pasos previos necesarios tal como se describe en el tutorial [Cómo configurar el filtro de tráfico y las reglas WAF](../setup.md). Además, ha clonado e implementado el [proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd) en su entorno de AEM.

## Acciones clave de las reglas

Antes de profundizar en los detalles de las reglas de filtro de tráfico estándar, vamos a conocer las acciones clave que realizan estas reglas. El atributo `action` de cada regla define cómo debe responder el filtro de tráfico cuando se cumplen las condiciones. Las acciones incluyen las siguientes:

- **Registro**: las reglas registran los eventos para la monitorización y el análisis, lo que permite revisar los patrones de tráfico y ajustar los umbrales según sea necesario. Se especifica mediante el atributo `type: log`.

- **Alerta**: las reglas activan alertas cuando se cumplen las condiciones, lo que le ayuda a identificar posibles problemas. Se especifica mediante el atributo `alert: true`.

- **Bloqueo**: las reglas bloquean el tráfico cuando se cumplen las condiciones, lo que impide el acceso al sitio de AEM. Se especifica mediante el atributo `action: block`.

## Revisión y definición de reglas

Las reglas de filtro de tráfico estándar recomendadas por Adobe sirven como capa base para identificar patrones de tráfico potencialmente maliciosos mediante el registro de eventos como el exceso de límites de velocidad basados en IP y el bloqueo del tráfico procedente de países específicos. Estos registros ayudan a los equipos a validar los umbrales y a tomar decisiones fundamentadas para, en última instancia, **pasar a las reglas de modo de bloqueo** sin interrumpir el tráfico legítimo.

Revisemos las tres reglas de filtro de tráfico estándar que hay que añadir al archivo `/config/cdn.yaml` del proyecto WKND de AEM:

- **Impedir DoS en el borde**: esta regla detecta posibles ataques de Denegación de servicio (DoS) en el borde de la CDN mediante la monitorización de solicitudes por segundo (RPS) desde las direcciones IP del cliente.
- **Impedir DoS en el origen**: esta regla detecta posibles ataques de Denegación de servicio (DoS) en el origen mediante la monitorización de las solicitudes de recuperación desde las direcciones IP del cliente.
- **Bloquear países OFAC**: esta regla bloquea el acceso desde países específicos que están sujetos a las restricciones de OFAC (Office of Foreign Assets Control).

### &#x200B;1. Impedir DoS en el borde

Esta regla **envía una alerta** cuando detecta un posible ataque de Denegación de servicio (DoS) en la CDN. El criterio para activar esta regla es cuando un cliente supera las **500 solicitudes por segundo** (un promedio de más de 10 segundos) por CDN POP (punto de presencia) en el borde.

Cuenta **todas** las solicitudes y las agrupa por dirección IP del cliente.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    - name: prevent-dos-attacks-edge
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 500
        window: 10
        penalty: 300
        count: all
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

El atributo `action` especifica que la regla debe registrar los eventos y activar una alerta cuando se cumplen las condiciones. Por lo tanto, le ayuda a monitorizar los potenciales ataques DoS sin bloquear el tráfico legítimo. Sin embargo, su objetivo es, finalmente, realizar la transición de esta regla al modo de bloqueo una vez que haya validado los patrones de tráfico y ajustado los umbrales.

### &#x200B;2. Evitar DoS en el origen

Esta regla **envía una alerta** cuando detecta un posible ataque de Denegación de servicio (DoS) en el origen. El criterio para activar esta regla es cuando un cliente supera las **100 solicitudes por segundo** (un promedio de más de 10 segundos) por IP de cliente en el origen.

Cuenta las **recuperaciones** (solicitudes que omiten la caché) y las agrupa por dirección IP del cliente.

```yaml
...
    - name: prevent-dos-attacks-origin
      when:
        reqProperty: tier
        equals: 'publish'
      rateLimit:
        limit: 100
        window: 10
        penalty: 300
        count: fetches
        groupBy:
          - reqProperty: clientIp
      action:
        type: log
        alert: true
```

El atributo `action` especifica que la regla debe registrar los eventos y activar una alerta cuando se cumplan las condiciones. Por lo tanto, le ayuda a monitorizar los potenciales ataques DoS sin bloquear el tráfico legítimo. Sin embargo, su objetivo es, finalmente, realizar la transición de esta regla al modo de bloqueo una vez que haya validado los patrones de tráfico y ajustado los umbrales.

### &#x200B;3. Bloquear países OFAC

Esta regla bloquea el acceso desde países específicos que se encuentran dentro de las restricciones de [OFAC](https://ofac.treasury.gov/sanctions-programs-and-country-information).
Puede revisar y modificar la lista de países según sea necesario.

```yaml
...
    - name: block-ofac-countries
      when:
        allOf:
          - { reqProperty: tier, in: ["author", "publish"] }
          - reqProperty: clientCountry
            in:
              - SY
              - BY
              - MM
              - KP
              - IQ
              - CD
              - SD
              - IR
              - LR
              - ZW
              - CU
              - CI
      action: block
```

El atributo `action` especifica que la regla debe bloquear el acceso desde los países especificados. Esto le ayuda a evitar el acceso al sitio de AEM desde regiones que pueden suponer riesgos de seguridad.

El archivo `cdn.yaml` completo con las reglas anteriores tiene el siguiente aspecto:

![Reglas WKND CDN YAML](../assets/use-cases/wknd-cdn-yaml-rules.png)

## Implementación de reglas

Para implementar las reglas, siga estos pasos:

- Confirme e inserte los cambios en el repositorio de Git de Cloud Manager.

- Implemente los cambios en el entorno de AEM mediante la canalización de configuración de Cloud Manager [creada anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

  ![Canalización de configuración de Cloud Manager](../assets/use-cases/cloud-manager-config-pipeline.png)

## Comprobación de las reglas

Para comprobar la eficacia de las reglas de filtro de tráfico estándar, tanto en el **borde de CDN** como en el **origen**, simule un tráfico elevado de solicitudes alto usando [Vegeta](https://github.com/tsenart/vegeta), una versátil herramienta de prueba de carga HTTP.

- Pruebe la regla DoS en el borde (límite de 500 rps). El siguiente comando simula 200 solicitudes por segundo durante 15 segundos, lo que supera el umbral del borde (500 rps).

  ```shell
  $echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html" | vegeta attack -rate=200 -duration=15s | vegeta report
  ```

  ![Ataque DoS Vegeta en el borde](../assets/use-cases/vegeta-dos-attack-edge.png)

  >[!IMPORTANT]
  >
  >  Observe los códigos de estado de éxito *100 %* y _200_ en el informe anterior. Como las reglas se establecen en `log` y `alert`, las solicitudes _no se bloquean_, pero se registran para la monitorización, el análisis y las alertas.

- Pruebe la regla DoS en el origen (límite de 100 rps). El siguiente comando simula 110 solicitudes de recuperación por segundo durante 1 segundo, lo que supera el umbral de origen (100 rps). Para simular solicitudes que omiten la caché, el archivo `targets.txt` se crea con parámetros de consulta únicos para garantizar que cada solicitud se trate como una solicitud de recuperación.

  ```shell
  # Create targets.txt with unique query parameters
  $for i in {1..110}; do
    echo "GET https://publish-p63947-e1249010.adobeaemcloud.com/us/en.html?ts=$(date +%s)$i"
  done > targets.txt
  
  # Use the targets.txt file to simulate fetch requests
  $vegeta attack -rate=110 -duration=1s -targets=targets.txt | vegeta report
  ```

  ![Ataque DoS Vegeta en el origen](../assets/use-cases/vegeta-dos-attack-origin.png)

  >[!IMPORTANT]
  >
  >  Observe los códigos de estado de éxito *100 %* y _200_ en el informe anterior. Como las reglas se establecen en `log` y `alert`, las solicitudes _no se bloquean_, pero se registran para la monitorización, análisis y alertas.

- Para simplificar, la regla OFAC no se prueba aquí.

## Revisión de las alertas

Las alertas se generan cuando se activan las reglas del filtro de tráfico. Puede revisar estas alertas en el [Centro de acciones de AEM](https://experience.adobe.com/aem/actions-center).

![Centro de acciones de WKND AEM](../assets/use-cases/wknd-aem-action-center.png)

## Análisis de resultados

Para analizar los resultados de las reglas del filtro de tráfico, puede utilizar los registros de CDN de AEMCS y la herramienta del panel de control ELK. Siga las instrucciones de la sección de configuración [Ingesta de registros de CDN](../setup.md#ingest-cdn-logs) para ingerir los registros de CDN en la pila ELK.

En la siguiente captura de pantalla, puede ver los registros de CDN del entorno de desarrollo de AEM introducidos en la pila ELK.

![Registros de WKND CDN ELK](../assets/use-cases/wknd-cdn-logs-elk.png)

Dentro de la aplicación ELK, el **panel de control de tráfico de CDN** debería mostrar el pico en el **borde** y en el **origen** durante los ataques DoS simulados.

Los dos paneles, _RPS en el borde por IP de cliente y POP_ y _RPS en el origen por IP de cliente y POP_, muestran las solicitudes por segundo (RPS) en el borde y el origen respectivamente, agrupadas por IP de cliente y punto de presencia (POP).

![Panel de control de tráfico en el borde de CDN de WKND](../assets/use-cases/wknd-cdn-edge-traffic-dashboard.png)

También puede usar otros paneles del panel de control de tráfico de CDN para analizar los patrones de tráfico, como las _IP de clientes principales_, los _países principales_ y los _agentes de usuarios principales_. Estos paneles le ayudan a identificar posibles amenazas y a ajustar las reglas de filtro de tráfico en consecuencia.

### Integración de Splunk

Los clientes que tienen [habilitado el reenvío de registros de Splunk](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) pueden crear nuevos paneles de control para analizar los patrones de tráfico.

Para crear paneles de control en Splunk, siga los pasos de [Paneles de control de Splunk para el análisis de registros CDN de AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

La siguiente captura de pantalla muestra un ejemplo de un panel de control Splunk que muestra el número máximo de solicitudes en el origen y el borde por IP, lo que puede ayudarle a identificar posibles ataques DoS.

![Panel de Splunk: número máximo de solicitudes en el origen y el borde por IP](../assets/use-cases/splunk-dashboard-max-origin-edge-requests.png)

## Cuándo y cómo perfeccionar las reglas

Su objetivo es evitar el bloqueo del tráfico legítimo mientras sigue protegiendo su sitio AEM ante posibles amenazas. Las reglas de filtro de tráfico estándar están diseñadas para alertar y registrar (y finalmente bloquear cuando se cambia el modo) las amenazas sin bloquear el tráfico legítimo.

Para perfeccionar las reglas, siga estos pasos:

- **Monitorizar patrones de tráfico**: utilice los registros de CDN y el panel de control ELK para monitorizar los patrones de tráfico e identificar cualquier anomalía o pico en el tráfico.
- **Ajustar umbrales**: en función de los patrones de tráfico, ajuste los umbrales (aumente o disminuya los límites de velocidad) de las reglas para que se ajusten mejor a sus requisitos específicos. Por ejemplo, si observa que el tráfico legítimo activó las alertas, puede aumentar los límites de velocidad o ajustar las agrupaciones.
Consulte la siguiente tabla para obtener instrucciones sobre cómo elegir los valores de umbral:

  | Variación | Valor |
  | :--------- | :------- |
  | Origen | Tome el valor más alto del número máximo de solicitudes de origen por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |
  | Edge | Tome el valor más alto del número máximo de solicitudes Edge por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |

  Consulte también la sección [Selección de valores de umbral](../../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) para obtener más información.

- **Pasar a reglas de bloqueo**: una vez validados los patrones de tráfico y ajustados los umbrales, debe pasar las reglas al modo de bloqueo.

## Resumen

En este tutorial, ha aprendido a proteger los sitios web de AEM frente a la denegación de servicio (DoS), la denegación de servicio distribuida (DDoS) y el abuso de bots mediante las reglas de filtro de tráfico estándar recomendadas por Adobe en AEM as a Cloud Service.

## Reglas WAF recomendadas

Aprenda a implementar las reglas WAF recomendadas por Adobe para proteger los sitios web de AEM ante amenazas sofisticadas que utilizan técnicas avanzadas para eludir las medidas de seguridad tradicionales.

<!-- CARDS
{target = _self}

* ./using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ../assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./using-waf-rules.md" title="Protección de sitios web de AEM mediante las reglas de filtro de tráfico WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/use-cases/using-waf-rules.png" alt="Protección de sitios web de AEM mediante las reglas de filtro de tráfico WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./using-waf-rules.md" target="_self" rel="referrer" title="Protección de sitios web de AEM mediante las reglas de filtro de tráfico WAF">Protección de sitios web de AEM mediante reglas de filtro de tráfico WAF</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo proteger los sitios web de AEM frente a amenazas sofisticadas, como DoS, DDoS y el abuso de bots, mediante las reglas de filtro de tráfico WAF (Web Application Firewall) recomendadas por Adobe en AEM as a Cloud Service.</p>
                </div>
                <a href="./using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activar WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Casos de uso: más allá de las reglas estándar

En el caso de los escenarios más avanzados, puede explorar los siguientes casos de uso que muestran cómo implementar reglas de filtro de tráfico personalizadas en función de requisitos comerciales específicos:

<!-- CARDS
{target = _self}

* ../how-to/request-logging.md

* ../how-to/request-blocking.md

* ../how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-logging.md" title="Monitorización de solicitudes confidenciales" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/wknd-login.png" alt="Monitorización de solicitudes confidenciales"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-logging.md" target="_self" rel="referrer" title="Monitorización de solicitudes confidenciales">Monitorización de solicitudes confidenciales</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo monitorizar las solicitudes confidenciales registrándolas mediante reglas de filtro de tráfico en AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-blocking.md" title="Restricción del acceso" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/elk-tool-dashboard-blocked.png" alt="Restricción del acceso"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-blocking.md" target="_self" rel="referrer" title="Restricción del acceso">Restricción del acceso</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo restringir el acceso bloqueando solicitudes específicas mediante reglas de filtro de tráfico en AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../how-to/request-transformation.md" title="Normalización de solicitudes" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/how-to/aemrequest-log-transformation.png" alt="Normalización de solicitudes"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../how-to/request-transformation.md" target="_self" rel="referrer" title="Normalización de solicitudes">Normalización de solicitudes</a>
                    </p>
                    <p class="is-size-6">Obtenga información sobre cómo normalizar solicitudes transformándolas mediante reglas de filtro de tráfico en AEM as a Cloud Service.</p>
                </div>
                <a href="../how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Más información</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Recursos adicionales

- [Reglas de inicio recomendadas](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules)

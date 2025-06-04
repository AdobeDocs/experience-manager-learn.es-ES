---
title: Bloqueo de ataques DoS y DDoS mediante reglas de filtro de tráfico
description: Obtenga información sobre cómo bloquear ataques DoS y DDoS mediante las reglas de filtro de tráfico en la CDN proporcionada por AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1924'
ht-degree: 100%

---

# Bloqueo de ataques DoS y DDoS mediante reglas de filtro de tráfico

Aprenda a bloquear los ataques de denegación de servicio (DoS) y denegación de servicio distribuido (DDoS) mediante las reglas de **filtro de tráfico de límite de frecuencia** y otras estrategias en la CDN administrada por AEM as a Cloud Service (AEMCS). Estos ataques causan picos de tráfico en la CDN y potencialmente en el servicio de AEM Publish (origen aka) y pueden afectar a la capacidad de respuesta y disponibilidad del sitio.

Este tutorial sirve como guía sobre _cómo analizar los patrones de tráfico y configurar las [reglas de filtro de tráfico](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ de límite de frecuencia para mitigar esos ataques. El tutorial también describe cómo [configurar alertas](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) para que reciba una notificación cuando haya un ataque sospechoso.

## Descripción de las protecciones

Conozcamos las protecciones predeterminadas DDoS de su sitio web de AEM:

- **Almacenamiento en caché:** con unas buenas políticas de almacenamiento en caché, el impacto de un ataque DDoS es más limitado porque la CDN evita que la mayoría de las solicitudes vayan al origen y provoquen una degradación del rendimiento.
- **Escalado automático:** los servicios de autor y publicación de AEM se escalan automáticamente para controlar los picos de tráfico, aunque pueden verse afectados por aumentos repentinos y masivos del tráfico.
- **Bloqueo:** la CDN de Adobe bloquea el tráfico en el origen si supera una frecuencia definida por Adobe desde una dirección IP en particular, por cada PoP (punto de presencia) de la CDN.
- **Alertas:** el Centro de acciones envía un pico de tráfico en la notificación de alerta de origen cuando el tráfico supera una determinada frecuencia. Esta alerta se activa cuando el tráfico a cualquier PoP de CDN determinada supera una frecuencia de solicitud _definida por Adobe_ por cada dirección IP. Consulte [Alertas sobre reglas de filtro de tráfico](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) para obtener más información.

Estas protecciones integradas deben considerarse una línea de base para que una organización pueda minimizar el impacto de un ataque DDoS en el rendimiento. Dado que cada sitio web presenta diferentes características de rendimiento y puede detectar esa degradación del rendimiento antes de que se cumpla el límite de frecuencia definido por Adobe, se recomienda ampliar las protecciones predeterminadas mediante la _configuración del cliente_.

Examinemos algunas medidas adicionales recomendadas que los clientes pueden tomar para proteger sus sitios web frente a ataques DDoS:

- Declare las **reglas de filtro de tráfico de límite de frecuencia** para bloquear el tráfico que supere una determinada frecuencia de una sola dirección IP, por cada PoP. Normalmente, estos umbrales son inferiores al límite de frecuencia definido por Adobe.
- Configure las **alertas** en las reglas de filtro de tráfico de límite de frecuencia mediante una “acción de alerta”, de modo que cuando se active la regla, se envíe una notificación al Centro de acciones.
- Aumente la cobertura de la caché declarando **transformaciones de solicitud** para omitir los parámetros de consulta.

### Variaciones de las reglas de tráfico de límite de frecuencia {#rate-limit-variations}

Existen dos variaciones en las reglas de tráfico de límite de frecuencia:

1. Edge: bloquea las solicitudes en función de la frecuencia de todo el tráfico (incluido el que se puede servir desde la caché de CDN), para una IP determinada, por cada PoP.
1. Origen: bloquea las solicitudes en función de la frecuencia de tráfico destinado al origen, para una IP determinada, por cada PoP.

## Recorrido del cliente

Los pasos siguientes reflejan el proceso probable a través del cual los clientes deben proteger sus sitios web.

1. Reconozca la necesidad de una regla de filtro de tráfico de límite de frecuencia. Esto podría deberse a que se ha recibido el pico de tráfico predeterminado de Adobe en la alerta de origen, o bien podría tratarse de una decisión proactiva para tomar precauciones y reducir el riesgo de que se produzca un ataque DDoS con éxito.
1. Analice los patrones de tráfico mediante un panel de control, si el sitio ya está activo, para determinar los umbrales óptimos de las reglas de filtro de tráfico de frecuencia de velocidad. Si el sitio aún no está activo, elija valores basados en las expectativas de tráfico.
1. Con los valores del paso anterior, configure las reglas de filtro de tráfico de límite de frecuencia. Asegúrese de habilitar las alertas correspondientes para que reciba una notificación cada vez que se alcance el umbral.
1. Reciba alertas sobre reglas de filtro de tráfico cada vez que se produzcan picos de tráfico, lo que le proporcionará una información valiosa sobre si su organización está siendo objeto de agentes malintencionados.
1. Actúe sobre la alerta, según sea necesario. Analice el tráfico para determinar si el pico refleja solicitudes legítimas en lugar de un ataque. Aumente los umbrales si el tráfico es legítimo o, en caso contrario, reduzca su valor.

El resto de este tutorial le guía a través de este proceso.

## Reconocimiento de la necesidad de configurar reglas {#recognize-the-need}

Como se mencionó anteriormente, Adobe bloquea de forma predeterminada el tráfico en la CDN que supera una frecuencia determinada. Sin embargo, algunos sitios web pueden experimentar un rendimiento inferior al umbral establecido. Por lo tanto, es necesario configurar las reglas de filtro de tráfico de límite de frecuencia.

Lo ideal sería configurar las reglas antes de activarlas en producción. En la práctica, muchas organizaciones reaccionan declarando reglas solo después de que recibir una alerta sobre un pico de tráfico que indica un posible ataque.

Adobe envía un pico de tráfico en la alerta de origen como una [Notificación del centro de acciones](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/operations/actions-center) cuando se supera un umbral predeterminado de tráfico de una sola dirección IP, para un PoP determinado. Si ha recibido una alerta de este tipo, se recomienda configurar una regla de filtro de tráfico de límite de frecuencia. Esta alerta predeterminada es diferente de las alertas que los clientes deben habilitar explícitamente al definir las reglas de filtro de tráfico, que se explicarán en una sección posterior.

## Análisis de patrones de tráfico {#analyze-traffic}

Si el sitio ya está activo, puede analizar los patrones de tráfico mediante los registros de CDN y los paneles de control proporcionados por Adobe.

- **Panel de control de tráfico de CDN**: proporciona información sobre el tráfico a través de CDN y la frecuencia de solicitudes de origen, 4xx y 5xx, y las solicitudes no almacenadas en caché. También proporciona el número máximo de solicitudes de CDN y de origen por segundo por cada dirección IP de cliente y más información para optimizar las configuraciones de CDN.

- **Proporción de aciertos de caché de CDN**: proporciona información sobre la proporción total de aciertos de caché y el recuento total de solicitudes por estado HIT, PASS y MISS. También proporciona las principales direcciones URL de HIT, PASS y MISS.

Configure las herramientas del panel de control mediante _una de las siguientes opciones_:

### ELK: configuración de las herramientas del panel de control

Las herramientas del panel de control de **Elasticsearch, Logstash y Kibana (ELK)** proporcionadas por Adobe se pueden usar para analizar los registros de CDN. Estas herramientas incluyen un panel de control que visualiza los patrones de tráfico, lo que facilita establecer los umbrales óptimos para las reglas de filtro de tráfico de límite de frecuencia.

- Clone el repositorio de GitHub [AEMCS-CDN-Log-Analysis-Tooling](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling).
- Configure las herramientas siguiendo los pasos que se indican en [Cómo configurar el contenedor de ELK Docker](ttps://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#how-to-set-up-the-elk-docker-containerhow-to-setup-the-elk-docker-container).
- Como parte de la configuración, importe el archivo `traffic-filter-rules-analysis-dashboard.ndjson` para visualizar los datos. El panel de control _Tráfico de CDN_ incluye visualizaciones que muestran el número máximo de solicitudes por cada IP/POP en el Edge y Origen de CDN.
- Desde la tarjeta _Entornos_ de [Cloud Manager](https://my.cloudmanager.adobe.com/), descargue los registros de CDN del servicio de AEMCS Publish. 

  ![Descargas de registros de CDN de Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > Las nuevas solicitudes pueden tardar hasta 5 minutos en aparecer en los registros de CDN.

### Splunk: configuración de las herramientas del panel de control

Los clientes que tienen [habilitado el reenvío de registros de Splunk](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) pueden crear nuevos paneles de control para analizar los patrones de tráfico.

Para crear paneles de control en Splunk, siga los pasos de [Paneles de control de Splunk para el análisis de registros CDN de AEMCS](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).

### Consulta de datos

En los paneles de control de ELK y Splunk tiene a su disposición las siguientes visualizaciones:

- **Edge RPS por IP de cliente y POP**: esta visualización muestra el número máximo de solicitudes por cada IP/POP **en CDN Edge**. El pico en la visualización indica el número máximo de solicitudes.

  **Panel de control de ELK**:
  ![Panel de control de ELK: número máximo de solicitudes por IP/POP](./assets/elk-edge-max-per-ip-pop.png)

  **Panel de control de Splunk**:
  ![Panel de control de Splunk: número máximo de solicitudes por IP/POP](./assets/splunk-edge-max-per-ip-pop.png)

- **RPS de origen por IP de cliente y POP**: esta visualización muestra el número máximo de solicitudes por IP/POP **en el origen**. El pico en la visualización indica el número máximo de solicitudes.

  **Panel de control de ELK**:
  ![Panel de control de ELK: número máximo de solicitudes de origen por IP/POP](./assets/elk-origin-max-per-ip-pop.png)

  **Panel de control de Splunk**:
  ![Panel de control de Splunk: número máximo de solicitudes de origen por IP/POP](./assets/splunk-origin-max-per-ip-pop.png)

## Elección de los valores de umbral

Los valores de umbral para las reglas de filtro de tráfico de límite de frecuencia deben basarse en el análisis anterior y hay que asegurarse de que el tráfico legítimo no esté bloqueado. Consulte la siguiente tabla para obtener instrucciones sobre cómo elegir los valores de umbral:

| Variación | Valor |
| :--------- | :------- |
| Origen | Tome el valor más alto del número máximo de solicitudes de origen por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |
| Edge | Tome el valor más alto del número máximo de solicitudes Edge por IP/POP en condiciones de tráfico **normales** (es decir, no la frecuencia en el momento de un DDoS) y auméntelo por un múltiplo |

Los múltiples que se utilizan dependen de las expectativas de los picos normales de tráfico debido al tráfico orgánico, las campañas y otros eventos. Un múltiplo entre 5 y 10 puede ser razonable.

Si su sitio aún no está activo, no hay datos que analizar y debe realizar una estimación fundamentada sobre los valores adecuados que debe establecer para las reglas de filtro de tráfico de límite de frecuencia. Por ejemplo:

| Variación | Valor |
|------------------------------ |:-----------:|
| Edge | 500 |
| Origen | 100 |

## Reglas de configuración {#configure-rules}

Configure las reglas de **filtro de tráfico de límite de frecuencia** en el archivo `/config/cdn.yaml` de su proyecto de AEM, con valores basados en la explicación anterior. Si es necesario, consulte con su equipo de seguridad web para asegurarse de que los valores del límite de velocidad sean adecuados y no bloquean el tráfico legítimo.

Consulte [Crear reglas en su proyecto de AEM](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project) para obtener más información.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action:
          type: log
          alert: true
```

Tenga en cuenta que se declaran tanto las reglas de origen como las de Edge y que la propiedad de alerta se establece en `true` para que pueda recibir alertas cuando se alcance el umbral, lo que probablemente indique un ataque.

Se recomienda establecer el tipo de acción en registro inicial para poder monitorizar el tráfico durante unas horas o días, asegurándose de que el tráfico legítimo no supere estas frecuencias. Al cabo de unos días, cambie al modo de bloqueo.

Siga los siguientes pasos para implementar los cambios en su entorno de AEMCS:

- Confirme y envíe los cambios anteriores a su repositorio de Git de Cloud Manager.
- Implemente los cambios en el entorno de AEMCS mediante la canalización de configuración de Cloud Manager. Consulte [Implementar reglas a través de Cloud Manager](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) para obtener más información.
- Para comprobar que la **regla de filtro de tráfico de límite de frecuencia** funcione según lo previsto, puede simular un ataque como se describe en la sección [Simulación de un ataque](#attack-simulation). Limite el número de solicitudes a un valor superior al valor de límite de frecuencia establecido en la regla.

### Configuración de las reglas de transformación de solicitudes {#configure-request-transform-rules}

Además de las reglas de filtro de tráfico de límite de frecuencia, se recomienda usar [transformaciones de solicitud](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) para anular la configuración de los parámetros de consulta que la aplicación no necesita, a fin de minimizar las formas de omitir la caché mediante técnicas de eliminación de caché. Por ejemplo, si solo desea permitir los parámetros de consulta `search` y `campaignId`, se puede declarar la siguiente regla:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  requestTransformations:
    rules:
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## Recepción de las alertas de reglas de filtro de tráfico {#receiving-alerts}

Como se ha mencionado anteriormente, si la regla de filtro de tráfico incluye *alerta: true*, se recibe una alerta cuando se cumple la regla.

## Actuación ante las alertas {#acting-on-alerts}

A veces, la alerta es informativa, lo que le da una idea de la frecuencia de los ataques. Vale la pena analizar los datos de CDN mediante el panel de control descrito anteriormente, para validar que el pico de tráfico se debe a un ataque y no solo a un aumento en el volumen de tráfico legítimo. En este último caso, considere la posibilidad de aumentar el umbral.

## Simulación de ataque{#attack-simulation}

En esta sección se describen los métodos para simular un ataque DoS, que se pueden utilizar para generar datos para los paneles de control que se usan en este tutorial y para validar que cualquier regla configurada bloquee ataques correctamente.

>[!CAUTION]
>
> No siga estos pasos en un entorno de producción. Los siguientes pasos son solo para la simulación.
>
>Si ha recibido una alerta que indica un pico en el tráfico, continúe con la sección [Análisis de patrones de tráfico](#analyzing-traffic-patterns).

Para simular un ataque, se pueden usar herramientas como [Apache Benchmark]( https://httpd.apache.org/docs/2.4/programs/ab.html?lang=es), [Apache JMeter](https://jmeter.apache.org/), [Vegeta](https://github.com/tsenart/vegeta) y otras.

### Solicitudes Edge

Con el siguiente comando [Vegeta](https://github.com/tsenart/vegeta), puede realizar muchas solicitudes al sitio web:

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=60s | vegeta report
```

El comando anterior realiza 120 solicitudes durante 5 segundos y genera un informe. Suponiendo que el sitio web no tenga una frecuencia limitada, esto puede causar un pico en el tráfico.

### Solicitudes de origen

Para omitir la caché de la CDN y realizar solicitudes al origen (servicio de AEM Publish), puede añadir un parámetro de consulta único a la URL. Consulte el script de muestra de Apache JMeter de [Simulación de un ataque DoS mediante un script JMeter](https://experienceleague.adobe.com/es/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script)


---
title: Ejemplos y análisis de resultados de reglas de filtro de tráfico, incluidas las reglas WAF
description: Conozca varias reglas de filtro de tráfico, incluidos ejemplos de reglas WAF. Además, obtenga información sobre cómo analizar los resultados mediante los registros de CDN de AEM as a Cloud Service (AEM CS).
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
duration: 532
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1472'
ht-degree: 0%

---

# Ejemplos y análisis de resultados de reglas de filtro de tráfico, incluidas las reglas WAF

Obtenga información sobre cómo declarar varios tipos de reglas de filtro de tráfico y analizar los resultados mediante los registros de CDN y las herramientas de tablero de Adobe Experience Manager as a Cloud Service (AEM CS).

En esta sección, explorará ejemplos prácticos de reglas de filtros de tráfico, incluidas las reglas WAF. AEM Aprenderá a registrar, permitir y bloquear solicitudes en función del URI (o la ruta), la dirección IP, el número de solicitudes y los distintos tipos de ataques mediante el [Proyecto de sitios WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) de .

Además, descubrirá cómo utilizar las herramientas de tablero que consumen registros de CDN de AEM CS para visualizar métricas esenciales a través de los tableros de muestra proporcionados por el Adobe.

AEM Para alinearse con sus requisitos específicos, puede mejorar y crear paneles personalizados, obteniendo así perspectivas más profundas y optimizando las configuraciones de reglas para sus sitios de.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## Ejemplos

Exploremos varios ejemplos de reglas de filtros de tráfico, incluidas las reglas WAF. AEM Asegúrese de haber completado el proceso de configuración necesario tal como se describe en el capítulo [cómo configurar](./how-to-setup.md) anterior y de haber clonado el [Proyecto de sitios WKND de](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### Registro de solicitudes

AEM Comience por **registrar solicitudes de rutas de inicio y cierre de sesión de WKND** en el servicio de Publish de la.

- Agregue la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND.

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
    # On AEM Publish service log WKND Login and Logout requests
      - name: publish-auth-requests
        when:
          allOf:
            - reqProperty: tier
              matches: publish
            - reqProperty: path
              in:
                - /system/sling/login/j_security_check
                - /system/sling/logout
        action: log
```

- Confirme y envíe los cambios al repositorio de Git de Cloud Manager.

- AEM Implemente los cambios en el entorno de desarrollo mediante la canalización de configuración de Cloud Manager `Dev-Config` [creada anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Canalización de configuración de Cloud Manager](./assets/cloud-manager-config-pipeline.png)

- Inicie sesión y cierre la sesión del sitio WKND del programa en el servicio Publish para probar la regla (por ejemplo, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Puede usar `asmith/asmith` como nombre de usuario y contraseña.

  ![Inicio de sesión WKND](./assets/wknd-login.png)

#### Análisis{#analyzing}

Analicemos los resultados de la regla `publish-auth-requests` descargando los registros de CDN de AEM CS de Cloud Manager y utilizando la [herramienta de tablero](how-to-setup.md#analyze-results-using-elk-dashboard-tool) que configuró en el capítulo anterior.

- Desde la tarjeta **Entornos** de [Cloud Manager](https://my.cloudmanager.adobe.com/), descargue los registros de CDN del servicio **Publish** de AEMCS.

  ![Descargas de registros de CDN de Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Las nuevas solicitudes pueden tardar hasta 5 minutos en aparecer en los registros de CDN.

- Copie el archivo de registro descargado (por ejemplo, `publish_cdn_2023-10-24.log` en la captura de pantalla siguiente) en la carpeta `logs/dev` del proyecto de herramienta Tablero elástico.

  ![Carpeta de registros de herramientas ELK](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Actualice la página Herramienta de tablero elástico.
   - En la sección **Filtro global** superior, edite el filtro `aem_env_name.keyword` y seleccione el valor de entorno `dev`.

     ![Filtro global de herramienta ELK](./assets/elk-tool-global-filter.png)

   - Para cambiar el intervalo de tiempo, haga clic en el icono de calendario en la esquina superior derecha y seleccione el intervalo de tiempo deseado.

     ![Intervalo de tiempo de herramienta ELK](./assets/elk-tool-time-interval.png)

- Revise las **solicitudes analizadas**, las **solicitudes marcadas** y los paneles de **Detalles de solicitudes marcadas** del tablero actualizado. Para las entradas de registro de CDN coincidentes, debe mostrar los valores de la IP de cliente (cli_ip), el host, la URL, la acción (waf_action) y el nombre de regla (waf_match) de cada entrada.

  ![Panel de herramientas ELK](./assets/elk-tool-dashboard.png)


### Bloqueo de solicitudes

En este ejemplo, vamos a agregar una página en una carpeta _internal_ en la ruta `/content/wknd/internal` en el proyecto WKND implementado. Luego declare una regla de filtro de tráfico que **bloquee el tráfico** a las subpáginas desde cualquier lugar que no sea una dirección IP especificada que coincida con su organización (por ejemplo, una VPN corporativa).

Puede crear su propia página interna (por ejemplo, `demo-page.html`) o usar el [paquete adjunto](./assets/demo-internal-pages-package.zip).

- Agregue la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND:

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

    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
      - name: block-internal-paths
        when:
          allOf:
            - reqProperty: path
              matches: /content/wknd/internal
            - reqProperty: clientIp
              notIn: [192.150.10.0/24]
        action: block
```

- Confirme y envíe los cambios al repositorio de Git de Cloud Manager.

- AEM Implemente los cambios en el entorno de desarrollo de mediante la canalización de configuración [creada anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` en Cloud Manager.

- Pruebe la regla accediendo a la página interna del sitio WKND, por ejemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` o utilizando el siguiente comando CURL:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Repita el paso anterior desde la dirección IP que utilizó en la regla y luego una dirección IP diferente (por ejemplo, usando su teléfono móvil).

#### Análisis

Para analizar los resultados de la regla `block-internal-paths`, siga los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Sin embargo, esta vez debería ver las **solicitudes bloqueadas** y los valores correspondientes en las columnas IP del cliente (cli_ip), host, URL, acción (waf_action) y nombre de regla (waf_match).

![Solicitud bloqueada del panel de herramientas ELK](./assets/elk-tool-dashboard-blocked.png)


### Prevención de ataques DoS

Vamos a **evitar ataques DoS** bloqueando solicitudes desde una dirección IP haciendo 100 solicitudes por segundo, causando que se bloquee durante 5 minutos.

- Agregue la siguiente regla de filtro de tráfico de límite de tasa [rate limit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) al archivo `/config/cdn.yaml` del proyecto WKND.

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
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
```

>[!WARNING]
>
>Para su entorno de producción, colabore con su equipo de seguridad web para determinar los valores apropiados para `rateLimit`,

- Confirme, inserte e implemente los cambios tal como se menciona en [ejemplos anteriores](#logging-requests).

- Para simular el ataque DoS, usa el siguiente comando [Vegeta](https://github.com/tsenart/vegeta).

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  Este comando realiza 120 solicitudes durante 5 segundos y genera un informe. Como puede ver, la tasa de éxito es del 32,5 %; se recibe un código de respuesta HTTP 406 para el resto, que muestra que el tráfico estaba bloqueado.

  ![Ataque DoS Vegeta](./assets/vegeta-dos-attack.png)

#### Análisis

Para analizar los resultados de la regla `prevent-dos-attacks`, siga los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Esta vez debería ver muchas **solicitudes bloqueadas** y los valores correspondientes en las columnas IP de cliente (cli_ip), host, url, acción (waf_action) y nombre de regla (waf_match).

![Solicitud DoS del tablero de herramientas ELK](./assets/elk-tool-dashboard-dos.png)

Además, los paneles **Principales 100 ataques de IP de cliente, país y agente de usuario** muestran detalles adicionales que se pueden usar para optimizar aún más la configuración de reglas.

![El panel de herramientas ELK hace las 100 solicitudes principales](./assets/elk-tool-dashboard-dos-top-100.png)

Para obtener más información sobre cómo evitar ataques DoS y DDoS, revise el tutorial [Bloqueo de ataques DoS y DDoS mediante reglas de filtro de tráfico](../blocking-dos-attack-using-traffic-filter-rules.md).

### Reglas de WAF

Todos los clientes de Sites y Forms pueden configurar los ejemplos de reglas de filtros de tráfico hasta el momento.

A continuación, vamos a explorar la experiencia de un cliente que ha adquirido una licencia de seguridad mejorada o protección WAF-DDoS, que le permite configurar reglas avanzadas para proteger los sitios web de ataques más sofisticados.

Antes de continuar, habilite la protección WAF-DDoS para su programa, tal como se describe en la documentación de reglas de filtro de tráfico [pasos de configuración](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### Sin indicadores WAFF

Veamos primero la experiencia incluso antes de que se declaren las reglas WAF. Cuando el WAF-DDoS está habilitado en su programa, su CDN registra de forma predeterminada cualquier coincidencia de tráfico malicioso, por lo que tiene la información correcta para llegar a las reglas adecuadas.

Empecemos atacando el sitio WKND sin agregar una regla WAF (o utilizando la propiedad `wafFlags`) y analicemos los resultados.

- Para simular un ataque, usa el siguiente comando [Nikto](https://github.com/sullo/nikto), que envía alrededor de 700 solicitudes malintencionadas en 6 minutos.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Simulación de ataque Nikto](./assets/nikto-attack.png)

  Para obtener más información acerca de la simulación de ataques, revise la documentación de [Nikto - Scan Tuning](https://github.com/sullo/nikto/wiki/Scan-Tuning), que le indica cómo especificar el tipo de ataques de prueba que se deben incluir o excluir.

##### Análisis

Para analizar los resultados de la simulación de ataques, siga los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Sin embargo, esta vez debería ver las **solicitudes marcadas** y los valores correspondientes en las columnas IP de cliente (cli_ip), host, url, acción (waf_action) y nombre de regla (waf_match). Esta información le permite analizar los resultados y optimizar la configuración de las reglas.

![Solicitud marcada WAF del tablero de herramientas ELK](./assets/elk-tool-dashboard-waf-flagged.png)

Observe cómo los paneles **WAF Flags distribution** y **Ataques principales** muestran detalles adicionales, que se pueden usar para optimizar aún más la configuración de reglas.

![El panel de herramientas ELK marca ataques de WAF Solicitud](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![Solicitud de ataques superiores WAF del tablero de herramientas ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Con WAFFlags

Ahora vamos a agregar una regla WAF que contiene la propiedad `wafFlags` como parte de la propiedad `action` y **bloquear las solicitudes de ataques simulados**.

Desde una perspectiva de sintaxis, las reglas WAF son similares a las vistas anteriormente, sin embargo, la propiedad `action` hace referencia a uno o más valores `wafFlags`. Para obtener más información acerca de `wafFlags`, consulte la sección [Lista de marcas WAF](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list).

- Agregue la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND. Observe cómo la regla `block-waf-flags` incluye algunos de los wafFlags que aparecieron en la herramienta de tablero cuando se atacaron con tráfico malintencionado simulado. De hecho, es una buena práctica con el tiempo analizar los registros para determinar qué nuevas reglas declarar, a medida que evoluciona el panorama de amenazas.

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
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
```

- Confirme, inserte e implemente los cambios tal como se menciona en [ejemplos anteriores](#logging-requests).

- Para simular un ataque, usa el mismo comando [Nikto](https://github.com/sullo/nikto) que antes.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Análisis

Repita los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Esta vez debería ver las entradas en **Solicitudes bloqueadas** y los valores correspondientes en las columnas IP de cliente (cli_ip), host, url, acción (waf_action) y nombre de regla (waf_match).

![Solicitud bloqueada WAF del tablero de herramientas ELK](./assets/elk-tool-dashboard-waf-blocked.png)

Además, los paneles **Distribución de marcas WAF** y **Ataques principales** muestran detalles adicionales.

![El panel de herramientas ELK marca ataques de WAF Solicitud](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![Solicitud de ataques superiores WAF del tablero de herramientas ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Análisis completo

En las secciones _analysis_ anteriores, ha aprendido a analizar los resultados de reglas específicas mediante la herramienta de tablero. Puede explorar más en profundidad el análisis de resultados mediante otros paneles, como:


- Solicitudes analizadas, marcadas y bloqueadas
- Distribución de indicadores WAF a lo largo del tiempo
- Reglas de filtro de tráfico activadas con el tiempo
- Ataques principales por ID de indicador WAF
- Filtro de tráfico activado principal
- Principales 100 atacantes por IP de cliente, país y agente de usuario

![Análisis exhaustivo del tablero de herramientas ELK](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![Análisis exhaustivo del tablero de herramientas ELK](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Siguiente paso

Familiarícese con las [prácticas recomendadas](./best-practices.md) para reducir el riesgo de violaciones de seguridad.

## Recursos adicionales

[Sintaxis de reglas de filtro de tráfico](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[Formato de registro de CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)


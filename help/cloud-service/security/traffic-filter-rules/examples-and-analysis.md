---
title: Ejemplos y análisis de resultados de reglas de filtro de tráfico, incluidas las reglas de WAF
description: Conozca varias reglas de filtro de tráfico, incluidos ejemplos de reglas de WAF. Además, obtenga información sobre cómo analizar los resultados mediante los registros de CDN de AEM as a Cloud Service (AEMCS).
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1472'
ht-degree: 100%

---

# Ejemplos y análisis de resultados de reglas de filtro de tráfico, incluidas las reglas de WAF

Obtenga información sobre cómo declarar varios tipos de reglas de filtro de tráfico y analizar los resultados mediante los registros de CDN y las herramientas de tablero de Adobe Experience Manager as a Cloud Service (AEMCS).

En esta sección, explorará ejemplos prácticos de reglas de filtro de tráfico, incluidas las reglas de WAF. Aprenderá a registrar, permitir y bloquear solicitudes en función del URI (o la ruta), la dirección IP, el número de solicitudes y los distintos tipos de ataques con el [Proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

Además, descubrirá cómo utilizar las herramientas de panel que consumen registros de CDN de AEMCS para visualizar métricas esenciales a través de los paneles de ejemplo que proporciona Adobe.

Para cumplir con sus requisitos específicos, puede mejorar y crear paneles personalizados, obteniendo así información más detallada y optimizando las configuraciones de reglas para sus sitios de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## Ejemplos

Analicemos varios ejemplos de reglas de filtros de tráfico, incluidas las reglas de WAF. Asegúrese de haber completado el proceso de configuración necesario tal como se describe en el capítulo [cómo configurar](./how-to-setup.md) anterior y de haber clonado el [proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### Registrar solicitudes

Comience por **registrar solicitudes de rutas de inicio y cierre de sesión de WKND** con el servicio AEM Publish.

- Añada la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND.

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

- Implemente los cambios en el entorno de desarrollo de AEM mediante la canalización de configuración de Cloud Manager `Dev-Config` [creada anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Canalización de configuración de Cloud Manager](./assets/cloud-manager-config-pipeline.png)

- Pruebe la regla iniciando sesión y cerrando sesión en el sitio WKND del programa en el servicio de publicación (por ejemplo, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Puede usar `asmith/asmith` como nombre de usuario y contraseña.

  ![Inicio de sesión WKND](./assets/wknd-login.png)

#### Análisis{#analyzing}

Analicemos los resultados de la regla `publish-auth-requests` descargando los registros de CDN de AEMCS de Cloud Manager y utilizando la [herramienta de tablero](how-to-setup.md#analyze-results-using-elk-dashboard-tool) que configuró en el capítulo anterior.

- Desde la tarjeta **Entornos** de [Cloud Manager](https://my.cloudmanager.adobe.com/), descargue los registros de CDN del servicio **Publish** de AEMCS.

  ![Descargas de registros de CDN de Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Las nuevas solicitudes pueden tardar hasta 5 minutos en aparecer en los registros de CDN.

- Copie el archivo de registro descargado (por ejemplo, `publish_cdn_2023-10-24.log` en la captura de pantalla siguiente) en la carpeta `logs/dev` del proyecto de herramienta panel elástico.

  ![Carpeta de registros de herramientas ELK](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Actualice la página de herramienta de panel elástico.
   - En la sección **Filtro global** superior, edite el filtro `aem_env_name.keyword` y seleccione el valor de entorno `dev`.

     ![Filtro global de herramienta ELK](./assets/elk-tool-global-filter.png)

   - Para cambiar el intervalo de tiempo, haga clic en el icono de calendario en la esquina superior derecha y seleccione el intervalo de tiempo deseado.

     ![Intervalo de tiempo de herramienta ELK](./assets/elk-tool-time-interval.png)

- Revise las **solicitudes analizadas**, las **solicitudes marcadas** y los paneles de **Datos de solicitudes marcadas** del panel actualizado. Para las entradas de registro de CDN coincidentes, debe mostrar los valores de la IP de cliente (cli_ip), el host, la URL, la acción (waf_action) y el nombre de regla (waf_match) de cada entrada.

  ![Panel de herramientas ELK](./assets/elk-tool-dashboard.png)


### Bloqueo de solicitudes

En este ejemplo, vamos a añadir una página en una carpeta _internal_ en la ruta `/content/wknd/internal` en el proyecto WKND implementado. Luego, declare una regla de filtro de tráfico que **bloquee el tráfico** a las subpáginas desde cualquier lugar que no sea una dirección IP especificada que coincida con su organización (por ejemplo, una VPN corporativa).

Puede crear su propia página interna (por ejemplo, `demo-page.html`) o usar el [paquete adjunto](./assets/demo-internal-pages-package.zip).

- Añada la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND:

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

- Implemente los cambios en el entorno de desarrollo de AEM usando la canalización de configuración [creada anteriormente](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` en Cloud Manager.

- Pruebe la regla accediendo a la página interna del sitio WKND, por ejemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` o utilizando el siguiente comando CURL:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Repita el paso anterior desde la dirección IP que utilizó en la regla y luego desde una dirección IP diferente (por ejemplo, usando su teléfono móvil).

#### Análisis

Para analizar los resultados de la regla `block-internal-paths`, siga los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Sin embargo, esta vez debería ver las **solicitudes bloqueadas** y los valores correspondientes en las columnas IP del cliente (cli_ip), host, URL, acción (waf_action) y nombre de regla (waf_match).

![Solicitud bloqueada del panel de herramientas ELK](./assets/elk-tool-dashboard-blocked.png)


### Prevención de ataques DoS

Vamos a **prevenir ataques DoS** bloqueando solicitudes desde una dirección IP haciendo 100 solicitudes por segundo, causando que se bloquee durante 5 minutos.

- Añada la siguiente [regla de filtro de tráfico de límite de tasa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=es#ratelimit-structure) al archivo `/config/cdn.yaml` del proyecto WKND.

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
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=60s | vegeta report
  ```

  Este comando realiza 120 solicitudes durante 5 segundos y genera un informe. Como puede ver, la tasa de éxito es del 32,5 %; se recibe un código de respuesta HTTP 406 para el resto, lo cual muestra que el tráfico estaba bloqueado.

  ![Ataque DoS Vegeta](./assets/vegeta-dos-attack.png)

#### Análisis

Para analizar los resultados de la regla `prevent-dos-attacks`, siga los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Esta vez, debería ver muchas **solicitudes bloqueadas** y los valores correspondientes en las columnas IP del cliente (cli_ip), host, url, acción (waf_action) y nombre de regla (waf_match).

![Solicitud DoS del tablero de herramientas ELK](./assets/elk-tool-dashboard-dos.png)

Además, los paneles **100 mejores ataques de IP de cliente, país y agente de usuario** muestran detalles adicionales que se pueden usar para optimizar aún más la configuración de reglas.

![100 mejores solicitudes DoS del tablero de herramientas ELK](./assets/elk-tool-dashboard-dos-top-100.png)

Para obtener más información sobre cómo prevenir ataques DoS y DDoS, revise el tutorial [Bloqueo de ataques DoS y DDoS mediante reglas de filtro de tráfico](../blocking-dos-attack-using-traffic-filter-rules.md).

### Reglas de WAF

Hasta el momento, todos los clientes de Sites y Forms pueden configurar los ejemplos de reglas de filtro de tráfico.

A continuación, vamos a explorar la experiencia de un cliente que ha adquirido una licencia de seguridad mejorada o de protección WAF-DDoS, que le permite configurar reglas avanzadas para proteger los sitios web de ataques más sofisticados.

Antes de continuar, habilite la protección WAF-DDoS para el programa, tal como se describe en la documentación de reglas de filtro de tráfico [pasos de configuración](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=es#setup).

#### Sin WAFFlags

Veamos primero la experiencia incluso antes de declarar las reglas de WAF. Cuando se habilita el WAF-DDoS en el programa, los logs de la CDN registran de forma predeterminada cualquier coincidencia de tráfico malicioso, por lo que tiene la información correcta para llegar a las reglas adecuadas.

Empecemos atacando el sitio WKND sin añadir una regla de WAF (o utilizando la propiedad `wafFlags`) y analicemos los resultados.

- Para simular un ataque, use el siguiente comando [Nikto]( https://github.com/sullo/nikto), que envía alrededor de 700 solicitudes malintencionadas en 6 minutos.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Simulación de ataque Nikto](./assets/nikto-attack.png)

  Para obtener más información acerca de la simulación de ataques, revise la documentación de [Nikto - Scan Tuning](https://github.com/sullo/nikto/wiki/Scan-Tuning), que indica cómo especificar el tipo de ataques de prueba que se deben incluir o excluir.

##### Análisis

Para analizar los resultados de la simulación de ataques, siga los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Sin embargo, esta vez debería ver las **solicitudes marcadas** y los valores correspondientes en las columnas IP del cliente (cli_ip), host, url, acción (waf_action) y nombre de regla (waf_match). Esta información le permite analizar los resultados y optimizar la configuración de las reglas.

![Solicitud marcada en WAF del panel de herramientas ELK](./assets/elk-tool-dashboard-waf-flagged.png)

Observe cómo los paneles **Distribución de indicadores WAF** y **Mejores ataques** muestran detalles adicionales, que se pueden utilizar para optimizar aún más la configuración de reglas.

![Solicitud de ataques de indicadores WAF del panel de herramientas ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![Solicitud de mejores ataques de WAF del panel de herramientas ELK](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Con WAFFlags

Ahora vamos a añadir una regla de WAF que contenga la propiedad `wafFlags` como parte de la propiedad `action` y **bloquear las solicitudes de ataques simulados**.

Desde una perspectiva de sintaxis, las reglas de WAF son similares a las vistas anteriormente, sin embargo, la propiedad `action` hace referencia a uno o más valores `wafFlags`. Para obtener más información sobre `wafFlags`, consulte la sección [Lista de indicadores de WAF](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=es#waf-flags-list).

- Añada la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND. Observe cómo la regla `block-waf-flags` incluye algunos de los wafFlags que aparecieron en la herramienta del panel cuando se atacaron con tráfico malintencionado simulado. De hecho, es una buena práctica con el tiempo analizar los registros para determinar qué nuevas reglas declarar, a medida que evoluciona el panorama de amenazas.

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

- Para simular un ataque, use el mismo comando [Nikto]( https://github.com/sullo/nikto) que antes.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Análisis

Repita los mismos pasos descritos en el [ejemplo anterior](#analyzing).

Esta vez, debería ver las entradas en **Solicitudes bloqueadas** y los valores correspondientes en las columnas IP del cliente (cli_ip), host, url, acción (waf_action) y nombre de regla (waf_match).

![Solicitud bloqueada de WAF del panel de herramientas ELK](./assets/elk-tool-dashboard-waf-blocked.png)

Además, los paneles **Distribución de indicadores de WAF** y **Mejores ataques** muestran detalles adicionales.

![Solicitud de ataques de indicadores WAF del panel de herramientas ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![Solicitud de mejores ataques de WAF del panel de herramientas ELK](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Análisis exhaustivo

En las secciones _análisis_ anteriores, ha aprendido a analizar los resultados de reglas específicas mediante la herramienta de panel. Puede explorar más en profundidad el análisis de resultados mediante otros paneles, como:


- Solicitudes analizadas, marcadas y bloqueadas
- Distribución de indicadores de WAF a lo largo del tiempo
- Reglas de filtro de tráfico activadas a lo largo del tiempo
- Mejores ataques por ID de indicadores de WAF
- Mejores filtros de tráfico activado
- Mejores 100 atacantes por IP de cliente, país y agente de usuario

![Análisis exhaustivo del panel de herramientas ELK](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![Análisis exhaustivo del panel de herramientas ELK](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Siguiente paso

Familiarícese con las [prácticas recomendadas](./best-practices.md) para reducir el riesgo de violaciones de seguridad.

## Recursos adicionales

[Sintaxis de reglas de filtro de tráfico](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=es#rules-syntax)

[Formato de registro de la CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=es#cdn-log-format)


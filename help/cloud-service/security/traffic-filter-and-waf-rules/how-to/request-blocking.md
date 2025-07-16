---
title: Restricción del acceso
description: Obtenga información sobre cómo restringir el acceso bloqueando solicitudes específicas mediante reglas de filtro de tráfico en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 22%

---

# Restricción del acceso

Obtenga información sobre cómo restringir el acceso bloqueando solicitudes específicas mediante reglas de filtro de tráfico en AEM as a Cloud Service.

Este tutorial muestra cómo **bloquear solicitudes a rutas internas desde IP públicas** en el servicio de publicación de AEM.

## Por qué y cuándo bloquear solicitudes

El bloqueo del tráfico ayuda a aplicar políticas de seguridad organizativas al impedir el acceso a recursos o direcciones URL confidenciales en determinadas condiciones. En comparación con el registro, el bloqueo es una acción más estricta y debe utilizarse cuando se esté seguro de que el tráfico de fuentes específicas no está autorizado o no es deseado.

Los escenarios comunes en los que el bloqueo es apropiado incluyen:

- Restringir el acceso a `internal` o `confidential` páginas solo a intervalos de IP internos (por ejemplo, detrás de una VPN corporativa).
- Bloqueo del tráfico de bots, analizadores automatizados o actores de amenazas identificados por IP o geolocalización.
- Impedir el acceso a puntos de conexión obsoletos o no seguros durante las migraciones organizadas.
- Limitar el acceso a las herramientas de creación o a las rutas de administración en los niveles de publicación.

## Requisitos previos

Antes de continuar, asegúrese de completar la configuración necesaria tal como se describe en el tutorial [Cómo configurar el filtro de tráfico y las reglas de WAF](../setup.md). Además, ha clonado e implementado el [proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd) en su entorno de AEM.

## Ejemplo: bloquear rutas internas de direcciones IP públicas

En este ejemplo, configure una regla para bloquear el acceso externo a una página WKND interna, como `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, desde direcciones IP públicas. Solo los usuarios dentro de un rango de IP de confianza (como una VPN corporativa) pueden acceder a esta página.

Puede crear su propia página interna (por ejemplo, `demo-page.html`) o usar el [paquete adjunto](../assets/how-to/demo-internal-pages-package.zip).

- Añada la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
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

- Implemente los cambios en el entorno de AEM mediante la canalización de configuración de Cloud Manager [creada anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Pruebe la regla accediendo a la página interna del sitio WKND, por ejemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` o utilizando el siguiente comando CURL:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Repita el paso anterior desde la dirección IP que utilizó en la regla y luego desde una dirección IP diferente (por ejemplo, usando su teléfono móvil).

## Análisis

Para analizar los resultados de la regla `block-internal-paths`, siga los mismos pasos descritos en el [tutorial de configuración](../setup.md#cdn-logs-ingestion)

Debería ver las **solicitudes bloqueadas** y los valores correspondientes en las columnas IP de cliente (cli_ip), host, URL, acción (waf_action) y nombre de regla (waf_match).

![Solicitud bloqueada del panel de herramientas ELK](../assets/how-to/elk-tool-dashboard-blocked.png)

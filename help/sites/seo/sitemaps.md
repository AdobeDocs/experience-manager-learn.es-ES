---
title: Mapas del sitio
description: Aprenda a impulsar su SEO creando mapas del sitio para AEM Sites.
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 937
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 5%

---

# Mapas del sitio

Aprenda a impulsar su SEO creando mapas del sitio para AEM Sites.

>[!WARNING]
>
>Este vídeo muestra el uso de direcciones URL relativas en el mapa del sitio. Los mapas del sitio [deben usar direcciones URL absolutas](https://sitemaps.org/protocol.html). Consulte [Configuraciones](#absolute-sitemap-urls) para ver cómo habilitar las direcciones URL absolutas, ya que esto no se trata en el siguiente vídeo.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configuraciones

### URL absolutas de mapa del sitio{#absolute-sitemap-urls}

El mapa del sitio de AEM admite direcciones URL absolutas mediante [asignación de Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Para ello, cree nodos de asignación en los servicios de AEM que generan mapas del sitio (normalmente, el servicio Publicación de AEM).

Un ejemplo de definición de nodo de asignación de Sling para `https://wknd.com` se puede definir en `/etc/map/https` de la siguiente manera:

| Ruta | Nombre de la propiedad | Tipo de propiedad | Valor de propiedad |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | Cadena | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | Cadena | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | Cadena | `wknd.com/$1` |

La captura de pantalla siguiente ilustra una configuración similar para `http://wknd.local` (una asignación de nombre de host local que se ejecuta en `http`).

![Configuración de direcciones URL absolutas de mapa del sitio](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Configuración de OSGi del programador de mapa del sitio

Define la [configuración de fábrica de OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) para la frecuencia (mediante [expresiones cron](https://cron.help/)) con la que los mapas del sitio se vuelven a generar y se almacenan en caché en AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.author`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Regla de filtro de permitidos para Dispatcher

Permitir solicitudes HTTP para los archivos de índice y mapa del sitio.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regla de reescritura del servidor web Apache

Asegúrese de que `.xml` solicitudes HTTP de mapa del sitio se enruten a la página de AEM subyacente correcta. Si no se utiliza el acortamiento de URL, o si se utilizan asignaciones de Sling para lograr el acortamiento de URL, esta configuración no es necesaria.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Recursos

+ [Documentación de mapa del sitio AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=es)
+ [Documentación de mapa del sitio Apache Sling](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentación de mapa del sitio de Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Documentación del archivo de índice de mapa de sitios Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Ayudante Cron](https://cron.help/)

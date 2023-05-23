---
title: Mapas del sitio
description: Aprenda a impulsar su SEO creando mapas del sitio para AEM Sites.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 5%

---

# Mapas del sitio

Aprenda a impulsar su SEO creando mapas del sitio para AEM Sites.

>[!WARNING]
>
>Este vídeo muestra el uso de direcciones URL relativas en el mapa del sitio. Mapas del sitio [debe utilizar direcciones URL absolutas](https://sitemaps.org/protocol.html). Consulte [Configuraciones](#absolute-sitemap-urls) para obtener información sobre cómo habilitar las direcciones URL absolutas, ya que esto no se trata en el vídeo siguiente.

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## Configuraciones

### URL absolutas de mapa del sitio{#absolute-sitemap-urls}

AEM El mapa del sitio admite direcciones URL absolutas usando [Asignación de Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). AEM Para ello, cree nodos de asignación en los servicios de que generan mapas del sitio (normalmente, el servicio de publicación de AEM).

Ejemplo de definición de nodo de asignación de Sling para `https://wknd.com` se puede definir en `/etc/map/https` como sigue:

| Ruta  | Nombre de la propiedad | Tipo de propiedad | Valor de propiedad |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | Cadena | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | Cadena | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | Cadena | `wknd.com/$1` |

La captura de pantalla siguiente ilustra una configuración similar, pero para `http://wknd.local` (una asignación de nombre de host local que se ejecuta en `http`).

![Configuración de direcciones URL absolutas de mapa](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Configuración de OSGi del programador de mapa del sitio

Define el [Configuración de fábrica de OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) para la frecuencia (usando [expresiones cron](http://www.cronmaker.com)AEM ) los mapas del sitio se vuelven a generar y se almacenan en caché en la caché de la.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Regla de filtro de permitidos de Dispatcher

Permitir solicitudes HTTP para los archivos de índice y mapa del sitio.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regla de reescritura del servidor web Apache

Asegurar `.xml` AEM Las solicitudes HTTP del mapa del sitio se dirigen a la página de datos subyacente correcta. Si no se utiliza el acortamiento de URL, o si se utilizan asignaciones de Sling para lograr el acortamiento de URL, esta configuración no es necesaria.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## Recursos

+ [AEM Documentación de mapa del sitio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentación de mapa del sitio Apache Sling](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentación de mapa del sitio Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Documentación del archivo de índice de mapa del sitio Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronista](http://www.cronmaker.com/)
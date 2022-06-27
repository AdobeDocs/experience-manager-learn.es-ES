---
title: Mapas del sitio
description: Aprenda a mejorar su SEO creando mapas del sitio para AEM Sites.
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 7cfc150989453eec776eb34eac9b4598c46b0d7c
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 6%

---

# Mapas del sitio

Aprenda a mejorar su SEO creando mapas del sitio para AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Medios

+ [Documentación AEM del mapa del sitio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentación del mapa del sitio de Apache Sling](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Documentación del mapa del sitio de Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Documentación del archivo de índice Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronista](http://www.cronmaker.com/)

## Configuraciones

### Configuración OSGi del programador del mapa del sitio

Define el [Configuración de fábrica de OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) para la frecuencia (utilizando [expresiones cron](http://www.cronmaker.com)) los mapas del sitio se regenerarán y almacenarán en la caché en AEM.

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Direcciones URL absolutas del mapa del sitio

AEM mapa del sitio admite direcciones URL absolutas mediante [Asignación de Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). Esto se realiza creando nodos de asignación en los servicios de AEM que generan mapas del sitio.

Un ejemplo de definición de nodo de asignación de Sling para `https://wknd.com` se puede definir en `/etc/map/https` de la siguiente manera:

| Ruta | Nombre de la propiedad | Tipo de propiedad | Valor de propiedad |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | Cadena | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | Cadena | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | Cadena | `wknd.com/$1` |

La captura de pantalla siguiente ilustra una configuración similar, pero para `http://wknd.local` (una asignación de nombre de host local que se ejecuta en `http`).

![Configuración de direcciones URL absolutas del mapa del sitio](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Regla de filtro de permiso de Dispatcher

Permitir solicitudes HTTP para el índice de mapa del sitio y los archivos de mapa del sitio.

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Regla de reescritura del servidor web Apache

Asegúrese `.xml` las solicitudes HTTP del mapa del sitio se dirigen a la página de AEM subyacente correcta. Si no se utiliza la abreviación de URL o las asignaciones de Sling se utilizan para acortar URL, esta configuración no es necesaria.

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

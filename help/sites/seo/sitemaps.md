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
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---


# Mapas del sitio

Aprenda a mejorar su SEO creando mapas del sitio para AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## Medios

+ [Documentación AEM del mapa del sitio](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Documentación del mapa del sitio de Apache Sling](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [AEM Componentes principales de WCM Github](https://github.com/adobe/aem-core-wcm-components)
   + Funcionalidad de mapa del sitio añadida en la versión 2.17.6
+ [Documentación del mapa del sitio de Sitemap.org](https://www.sitemaps.org/protocol.html)
+ [Documentación del archivo de índice Sitemap.org](https://www.sitemaps.org/protocol.html#index)
+ [Cronista](http://www.cronmaker.com/)

## Configuraciones

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

Define el [Configuración de fábrica de OSGi](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) para la frecuencia (utilizando [expresiones cron](http://www.cronmaker.com)) los mapas del sitio se regenerarán y almacenarán en la caché en AEM.

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

Permitir solicitudes HTTP para el índice de mapa del sitio y los archivos de mapa del sitio.

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

Asegúrese `.xml` las solicitudes HTTP del mapa del sitio se dirigen a la página de AEM subyacente correcta. Si no se utiliza la abreviación de URL o las asignaciones de Sling se utilizan para acortar URL, esta configuración no es necesaria.

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

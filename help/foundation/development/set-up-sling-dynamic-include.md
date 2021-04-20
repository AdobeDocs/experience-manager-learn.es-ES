---
title: Configuración de Sling Dynamic Include para AEM
description: Una guía de vídeo sobre la instalación y el uso de Apache Sling Dynamic Include con AEM Dispatcher que se ejecuta en el servidor web HTTP Apache.
version: 6.3, 6.4, 6.5
sub-product: fundaciones, sitios
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 6%

---


# Configurar [!DNL Sling Dynamic Include]

Un vídeo de introducción a la instalación y el uso de [!DNL Apache Sling Dynamic Include] con [AEM Dispatcher](https://docs.adobe.com/content/help/es-ES/experience-manager-dispatcher/using/dispatcher.html) que se ejecuta en [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Asegúrese de que la última versión de AEM Dispatcher esté instalada localmente.

1. Descargue e instale el [[!DNL Sling Dynamic Include] paquete](https://sling.apache.org/downloads.cgi).
1. Configure [!DNL Sling Dynamic Include] a través de [!DNL OSGi Configuration Factory] en **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   O bien, para añadir a una base de código de AEM, cree el nodo **sling:OsgiConfig** apropiado en:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (Opcional) Repita el último paso para permitir que los componentes del [contenido bloqueado (inicial) de las plantillas editables](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/page-templates-editable.html) también se proporcionen mediante [!DNL SDI]. El motivo de la configuración adicional es que el contenido bloqueado de las plantillas editables se suministra desde `/conf` en lugar de `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. Actualice el archivo [!DNL Apache HTTPD Web server] `httpd.conf` para habilitar el módulo [!DNL Include].

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Actualice el archivo [!DNL vhost] para respetar las directivas de inclusión.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. Actualice el archivo de configuración dispatcher.any para que admita (1) selectores `nocache` y (2) habilite la compatibilidad con TTL.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > Dejar el `*` final en la regla `*.nocache.html*` global anterior, puede causar [problemas en las solicitudes de subrecursos](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Reinicie siempre [!DNL Apache HTTP Web Server] después de realizar cambios en sus archivos de configuración o en el `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Si está utilizando [!DNL Sling Dynamic Includes] para servir las órdenes de inclusión del lado del perímetro (ESI), asegúrese de almacenar en caché los encabezados de respuesta relevantes [en la caché del despachante](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Los posibles encabezados son los siguientes:
>
>* &quot;Cache-Control&quot;
>* &quot;Disposición de contenido&quot;
>* &quot;Content-Type&quot;
>* &quot;Caduca&quot;
>* &quot;Última modificación&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Última modificación&quot;

>



## Materiales de apoyo

* [Descargar paquete de inclusión dinámica de Sling](https://sling.apache.org/downloads.cgi)
* [Documentación de Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)

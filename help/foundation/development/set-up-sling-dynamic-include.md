---
title: Configurar Sling Dynamic Include para AEM
description: Un vídeo explicativo de la instalación y el uso de Apache Sling Dynamic Include con AEM Dispatcher que se ejecuta en Apache HTTP Web Server.
version: 6.3, 6.4, 6.5
sub-product: fundación, sitios
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 3%

---


# Configurar [!DNL Sling Dynamic Include]

Un vídeo explicativo de la instalación y el uso de [!DNL Apache Sling Dynamic Include] con [AEM Dispatcher](https://docs.adobe.com/content/help/es-ES/experience-manager-dispatcher/using/dispatcher.html) que se ejecuta en [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Asegúrese de que la versión más reciente de AEM Dispatcher esté instalada localmente.

1. Descargue e instale el [[!DNL Sling Dynamic Include] paquete](https://sling.apache.org/downloads.cgi).
1. Configure [!DNL Sling Dynamic Include] mediante [!DNL OSGi Configuration Factory] en **http://&lt;host>:&lt;puerto>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   O bien, para agregar a una base de código AEM, cree el nodo **sling:OsgiConfig** correspondiente en:

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

1. (Opcional) Repita el último paso para permitir que los componentes en [contenido bloqueado (inicial) de plantillas editables](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) también se puedan servir mediante [!DNL SDI]. El motivo de la configuración adicional es que el contenido bloqueado de las plantillas editables se proporciona desde `/conf` en lugar de `/content`.

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

1. Actualice el archivo de configuración dispatcher.any para admitir (1) `nocache` selectores y (2) habilitar la compatibilidad con TTL.

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
   > Dejar el `*` final en la regla `*.nocache.html*` glob anterior, puede resultar en [problemas en las solicitudes de subrecursos](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

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
>Si está utilizando [!DNL Sling Dynamic Includes] para ofrecer elementos de Edge-side includes (ESI), asegúrese de almacenar en caché los encabezados de respuesta relevantes [en la caché del despachante](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Los posibles encabezados son los siguientes:
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;Caduca&quot;
>* &quot;Última modificación&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Última modificación&quot;

>



## Materiales de apoyo

* [Descargar paquete Sling Dynamic Include](https://sling.apache.org/downloads.cgi)
* [Documentación de Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)

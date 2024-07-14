---
title: AEM Configuración de la inclusión dinámica de Sling para la inclusión de
description: AEM Un tutorial en vídeo sobre la instalación y el uso de Apache Sling Dynamic Include con el Dispatcher de Apache que se ejecuta en el servidor web HTTP de Apache.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Configurar [!DNL Sling Dynamic Include]

AEM Un vídeo de introducción a la instalación y el uso de [!DNL Apache Sling Dynamic Include] con [Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es) en ejecución en [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> AEM Asegúrese de que la versión más reciente de Dispatcher esté instalada localmente.

1. Descargue e instale el [[!DNL Sling Dynamic Include] paquete](https://sling.apache.org/downloads.cgi).
1. Configure [!DNL Sling Dynamic Include] mediante [!DNL OSGi Configuration Factory] en **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   AEM O bien, para agregarlo a una base de código de, cree el nodo **sling:OsgiConfig** apropiado en:

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

1. (Opcional) Repita el último paso para permitir que los componentes del contenido [bloqueado (inicial) de las plantillas editables](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) se proporcionen también a través de [!DNL SDI]. El motivo de la configuración adicional es que el contenido bloqueado de las plantillas editables se proporciona desde `/conf` en lugar de desde `/content`.

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

1. Actualice el archivo `httpd.conf` de [!DNL Apache HTTPD Web server] para habilitar el módulo [!DNL Include].

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
   > Si se deja el `*` final en la regla glob `*.nocache.html*` anterior, pueden producirse [problemas en las solicitudes de subrecursos](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Reinicie siempre [!DNL Apache HTTP Web Server] después de realizar cambios en sus archivos de configuración o en `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Si usa [!DNL Sling Dynamic Includes] para servir inclusiones del lado del perímetro (ESI), asegúrese de almacenar en caché [los encabezados de respuesta relevantes en la caché de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Entre los posibles encabezados se incluyen los siguientes:
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

* [Descargar paquete de inclusión dinámica de Sling](https://sling.apache.org/downloads.cgi)
* [Documentación de inclusión dinámica de Apache Sling](https://github.com/Cognifide/Sling-Dynamic-Include)

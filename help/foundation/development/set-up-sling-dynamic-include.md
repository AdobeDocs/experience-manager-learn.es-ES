---
title: AEM Configuración de la inclusión dinámica de Sling para la inclusión de
description: AEM Un tutorial en vídeo sobre la instalación y el uso de Apache Sling Dynamic Include con Dispatcher de en ejecución en el servidor web HTTP de Apache.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 887
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Configuración de [!DNL Sling Dynamic Include]

Vídeo de introducción a la instalación y el uso de [!DNL Apache Sling Dynamic Include] con [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=es) ejecución en [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> AEM Asegúrese de que la última versión de Dispatcher esté instalada localmente.

1. Descargue e instale [[!DNL Sling Dynamic Include] paquete](https://sling.apache.org/downloads.cgi).
1. Configurar [!DNL Sling Dynamic Include] a través de [!DNL OSGi Configuration Factory] en **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   AEM O bien, para agregar a un código basado en un código de, cree el **sling:OsgiConfig** nodo en:

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

1. (Opcional) Repita el último paso para permitir los componentes en [contenido bloqueado (inicial) de plantillas editables](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) para servir mediante [!DNL SDI] y también. El motivo de la configuración adicional es que el contenido bloqueado de las plantillas editables se proporciona desde `/conf` en lugar de `/content`.

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

1. Actualizar [!DNL Apache HTTPD Web server]de `httpd.conf` para activar el [!DNL Include] módulo.

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Actualice el [!DNL vhost] archivo para respetar directivas include.

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

1. Actualice el archivo de configuración dispatcher.any para que sea compatible con (1) `nocache` selectores y (2) habilitan la compatibilidad con TTL.

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
   > Dejando el final `*` en el glob `*.nocache.html*` la regla anterior, puede dar como resultado [problemas en solicitudes de subrecursos](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Reiniciar siempre [!DNL Apache HTTP Web Server] después de realizar cambios en sus archivos de configuración o en el `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Si está utilizando [!DNL Sling Dynamic Includes] para servir inclusiones del lado del borde (ESI), asegúrese de almacenar en caché los datos relevantes [encabezados de respuesta en la caché de dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Entre los posibles encabezados se incluyen los siguientes:
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

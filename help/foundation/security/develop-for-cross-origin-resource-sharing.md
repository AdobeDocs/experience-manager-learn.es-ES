---
title: Desarrollar para uso compartido de recursos entre Orígenes (CORS) con AEM
description: Un ejemplo breve de cómo aprovechar CORS para acceder a contenido AEM desde una aplicación web externa a través de JavaScript del lado del cliente.
version: 6.3, 6,4, 6.5
sub-product: fundación, servicios de contenido, sitios
feature: null
topics: security, development, content-delivery
activity: develop
audience: developer
doc-type: tutorial
translation-type: tm+mt
source-git-commit: c657eefa69b383c1b1a9e2845276245d3db00e6f
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 0%

---


# Desarrollar para el uso compartido de recursos entre Orígenes (CORS)

Un ejemplo breve de cómo aprovechar [!DNL CORS] para acceder a contenido AEM desde una aplicación Web externa mediante JavaScript del lado del cliente.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

En este vídeo:

* **www.example.** comaps para localhost mediante  `/etc/hosts`
* **aem-publish.** localmaps a localhost mediante  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12) (un contenedor para SimpleHTTPServer [[!DNL Python] de ](https://docs.python.org/2/library/simplehttpserver.html)s) está sirviendo la página HTML a través del puerto 8000.
* [!DNL AEM Dispatcher] se ejecuta en  [!DNL Apache HTTP Web Server] 2.4 y se realiza una solicitud de proxy inverso  `aem-publish.local` en  `localhost:4503`.

Para obtener más información, consulte [Explicación del uso compartido de recursos entre Orígenes (CORS) en AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML y JavaScript

Esta página Web tiene lógica en que

1. Al hacer clic en el botón
1. Realiza una solicitud [!DNL AJAX GET] a `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera el `jcr:title` formulario de la respuesta JSON
1. Inyecta el `jcr:title` en el DOM

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## Configuración de fábrica OSGi

La fábrica de configuración OSGi para [!DNL Cross-Origin Resource Sharing] está disponible mediante:

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Configuración de Dispatcher {#dispatcher-configuration}

Para permitir el almacenamiento en caché y el servicio de encabezados CORS en contenido en caché, agregue la siguiente [/configuración de los encabezados de cliente](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) a todos los archivos de AEM Publish `dispatcher.any` compatibles.

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin",
      "Access-Control-Expose-Headers",
      "Access-Control-Max-Age",
      "Access-Control-Allow-Credentials",
      "Access-Control-Allow-Methods",
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Reinicie la** aplicación del servidor web después de realizar cambios en el  `dispatcher.any` archivo.

Es probable que borre la caché por completo para garantizar que los encabezados se almacenen correctamente en la caché en la siguiente solicitud después de una actualización de configuración `/clientheaders`.

## Materiales de apoyo {#supporting-materials}

* [AEM fábrica de configuración de OSGi para directivas de uso compartido de recursos entre Orígenes](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer para macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (compatible con Windows/macOS/Linux)

* [Explicación del uso compartido de recursos entre Orígenes (CORS) en AEM](./understand-cross-origin-resource-sharing.md)
* [Uso compartido de recursos entre Orígenes (W3C)](https://www.w3.org/TR/cors/)
* [CONTROL DE ACCESO HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)


---
title: Desarrollo para el intercambio de recursos de origen cruzado (CORS) con AEM
description: Un breve ejemplo de cómo aprovechar CORS para acceder a contenido AEM desde una aplicación web externa a través de JavaScript del lado del cliente.
version: 6.3, 6,4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# Desarrollo para el intercambio de recursos de origen cruzado (CORS)

Un breve ejemplo de cómo aprovechar [!DNL CORS] para acceder a contenido AEM desde una aplicación web externa a través de JavaScript del lado del cliente.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

En este vídeo:

* **www.example.** se compila en localhost mediante  `/etc/hosts`
* **aem-publish.** localmaps para localhost a través de  `/etc/hosts`
* SimpleHTTPServer (un envoltorio para SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) de [[!DNL Python]) está sirviendo la página HTML a través del puerto 8000.
   * _Ya no está disponible en Mac App Store. Usar similar, como [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] se ejecuta en  [!DNL Apache HTTP Web Server] 2.4 y la solicitud de proxy inverso  `aem-publish.local` a  `localhost:4503`.

Para obtener más información, consulte [Explicación del uso compartido de recursos de origen cruzado (CORS) en AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML y JavaScript

Esta página web tiene lógica que

1. Al hacer clic en el botón
1. Hace una [!DNL AJAX GET] solicitud a `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera el `jcr:title` de la respuesta JSON
1. Inserta el `jcr:title` en el DOM

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

## Configuración de fábrica de OSGi

La fábrica de configuración OSGi para [!DNL Cross-Origin Resource Sharing] está disponible a través de:

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

Para permitir el almacenamiento en caché y la entrega de encabezados CORS en contenido almacenado en caché, añada la siguiente [/configuración de encabezados de clientes](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) a todos los archivos `dispatcher.any` de AEM Publish compatibles.

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Reinicie la** aplicación del servidor web después de realizar cambios en el  `dispatcher.any` archivo.

Es probable que se borre la caché por completo para garantizar que los encabezados se almacenen correctamente en caché en la siguiente solicitud después de una actualización de configuración `/clientheaders`.

## Materiales de apoyo {#supporting-materials}

* [AEM fábrica de configuración OSGi para directivas de intercambio de recursos de origen cruzado](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Jeeves para macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)  (compatible con Windows/macOS/Linux)

* [Explicación del uso compartido de recursos de origen cruzado (CORS) en AEM](./understand-cross-origin-resource-sharing.md)
* [Uso compartido de recursos de origen cruzado (W3C)](https://www.w3.org/TR/cors/)
* [Control de acceso HTTP (MDN Mozilla)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)


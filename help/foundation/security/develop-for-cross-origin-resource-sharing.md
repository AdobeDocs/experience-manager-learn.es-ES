---
title: AEM Desarrollo para el Intercambio de Recursos de Origen Cruzado (CORS) con la ayuda de la comunidad de
description: AEM Un breve ejemplo de cómo aprovechar CORS para acceder a contenido de la desde una aplicación web externa a través de JavaScript del lado del cliente.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Desarrollo para el Intercambio de Recursos de Origen Cruzado (CORS)

AEM Un ejemplo breve de cómo aprovechar [!DNL CORS] para acceder a contenido de la desde una aplicación web externa a través de JavaScript del lado del cliente. AEM Este ejemplo utiliza la configuración OSGi de CORS para habilitar el acceso CORS en el modo de acceso a la. El enfoque de configuración OSGi es viable cuando:

* AEM Un solo origen es el acceso al contenido de Publish de la
* AEM Se requiere acceso CORS para el autor de la

AEM Si se requiere acceso de varios orígenes a Publish, consulte [esta documentación](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

En este vídeo:

* **www.example.com** se asigna a localhost mediante `/etc/hosts`
* **aem-publish.local** se asigna a localhost mediante `/etc/hosts`
* SimpleHTTPServer (un contenedor para SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) de [[!DNL Python]) está sirviendo la página del HTML a través del puerto 8000.
   * _Ya no está disponible en Mac App Store. Usar elementos similares como [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] se está ejecutando en [!DNL Apache HTTP Web Server] 2.4 y la solicitud de proxy inverso de `aem-publish.local` a `localhost:4503`.

AEM Para obtener más información, revisa [Comprender el Intercambio de Recursos de Origen Cruzado (CORS) en el caso de los recursos compartidos de origen cruzado (CORS) en el caso de los recursos de origen cruzado (](./understand-cross-origin-resource-sharing.md)).

## HTML de www.example.com y JavaScript

Esta página web tiene una lógica que

1. Al hacer clic en el botón
1. Realiza una solicitud de [!DNL AJAX GET] a `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Recupera `jcr:title` de la respuesta JSON
1. Inserta `jcr:title` en el DOM

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

El generador de configuración OSGi para [!DNL Cross-Origin Resource Sharing] está disponible a través de:

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

### Permitir encabezados de solicitud CORS

AEM Para permitir que los [encabezados de solicitud HTTP necesarios pasen a los elementos de la lista de distribución para su procesamiento](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), deben permitirse en la configuración `/clientheaders` de Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Almacenamiento en caché de encabezados de respuesta CORS

AEM Para permitir el almacenamiento en caché y el servicio de encabezados CORS en contenido almacenado en caché, agregue la siguiente configuración [/cache /headers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es#caching-http-response-headers) al archivo de Publish `dispatcher.any` de la.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

**Reinicie la aplicación del servidor web** después de realizar cambios en el archivo `dispatcher.any`.

Es probable que se necesite borrar la caché por completo para garantizar que los encabezados se almacenen correctamente en la caché en la siguiente solicitud después de una actualización de configuración de `/cache /headers`.

## Materiales de apoyo {#supporting-materials}

* [Jeeves para macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Servidor Python SimpleHTTPS](https://docs.python.o:qrg/2/library/simplehttpserver.html) (compatible con Windows/macOS/Linux)

* [AEM Comprender el Intercambio de Recursos de Origen Cruzado (CORS) en la](./understand-cross-origin-resource-sharing.md)
* [Uso Compartido De Recursos De Origen Cruzado (W3C)](https://www.w3.org/TR/cors/)
* [Control de acceso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

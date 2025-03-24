---
title: Comprensión del Intercambio de Recursos de Origen Cruzado (CORS) con AEM
description: El Intercambio de Recursos de Origen Cruzado (CORS) de Adobe Experience Manager facilita las propiedades web que no son de AEM para hacer llamadas del lado del cliente a AEM, tanto autenticadas como no autenticadas, para recuperar contenido o interactuar directamente con AEM.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1011'
ht-degree: 1%

---

# Comprender el intercambio de recursos de origen cruzado ([!DNL CORS])

El Intercambio de Recursos de Origen Cruzado ([!DNL CORS]) de Adobe Experience Manager facilita las propiedades web que no son de AEM para hacer llamadas del lado del cliente a AEM, tanto autenticadas como no autenticadas, para recuperar contenido o interactuar directamente con AEM.

La configuración OSGI descrita en este documento es suficiente para lo siguiente:

1. Uso compartido de recursos de un solo origen en AEM Publish
2. Acceso CORS a AEM Author

Si se requiere acceso CORS de varios orígenes en la publicación de AEM, consulte [esta documentación](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

## Configuración de OSGi de la política de uso compartido de recursos de origen cruzado de Adobe Granite

Las configuraciones de CORS se administran como fábricas de configuración OSGi en AEM, y cada directiva se representa como una instancia de la fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuración OSGi de la política de uso compartido de recursos de origen cruzado de Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selección de política

Se selecciona una política comparando las variables

* `Allowed Origin` con el encabezado de solicitud `Origin`
* y `Allowed Paths` con la ruta de solicitud.

Se utiliza la primera directiva que coincide con estos valores. Si no se encuentra ninguna, se denegará cualquier solicitud [!DNL CORS].

Si no se configura ninguna directiva, tampoco se responderá a las solicitudes de [!DNL CORS] porque el controlador está deshabilitado y, por lo tanto, se denegará de forma efectiva, siempre que ningún otro módulo del servidor responda a [!DNL CORS].

### Propiedades de directiva

#### [!UICONTROL Orígenes permitidos]

* `"alloworigin" <origin> | *`
* Lista de `origin` parámetros que especifican los URI que pueden acceder al recurso. Para solicitudes sin credenciales, el servidor puede especificar &#42; como comodín, permitiendo así que cualquier origen acceda al recurso. *No se recomienda usar `Allow-Origin: *` en producción, ya que permite que cada sitio web extranjero (es decir, atacante) realice solicitudes que los exploradores prohíben estrictamente sin CORS.*

#### [!UICONTROL Orígenes permitidos (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de `regexp` expresiones regulares que especifican los URI que pueden acceder al recurso. *Las expresiones regulares pueden provocar coincidencias no deseadas si no se crean con cuidado, lo que permite a un atacante utilizar un nombre de dominio personalizado que también coincida con la directiva.* En general, se recomienda tener directivas independientes para cada nombre de host de origen específico, utilizando `alloworigin`, aunque esto signifique una configuración repetida de las demás propiedades de directiva. Los distintos orígenes tienden a tener ciclos de vida y requisitos diferentes, por lo que se benefician de una separación clara.

#### [!UICONTROL Rutas permitidas]

* `"allowedpaths" <regexp>`
* Lista de `regexp` expresiones regulares que especifican las rutas de recursos para las que se aplica la directiva.

#### [!UICONTROL Encabezados expuestos]

* `"exposedheaders" <header>`
* Lista de parámetros de encabezado que indican los encabezados de respuesta a los que los exploradores pueden acceder. En el caso de las solicitudes CORS (no de comprobación preliminar), si no están vacías, estos valores se copian en el encabezado de respuesta `Access-Control-Expose-Headers`. A continuación, el explorador puede acceder a los valores de la lista (nombres de encabezado); sin ella, el explorador no puede leer esos encabezados.

#### [!UICONTROL Edad máxima]

* `"maxage" <seconds>`
* Un parámetro `seconds` que indica cuánto tiempo se pueden almacenar en caché los resultados de una solicitud de verificación previa al vuelo.

#### [!UICONTROL Encabezados admitidos]

* `"supportedheaders" <header>`
* Lista de `header` parámetros que indican qué encabezados de solicitud HTTP se pueden usar al realizar la solicitud real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parámetros de método que indica qué métodos HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Admite credenciales]

* `"supportscredentials" <boolean>`
* Un `boolean` que indica si la respuesta a la solicitud se puede exponer al explorador. Cuando se utiliza como parte de una respuesta a una solicitud de verificación previa al vuelo, esto indica si la solicitud real se puede realizar o no mediante credenciales.

### Configuraciones de ejemplo

El sitio 1 es un escenario básico, de acceso anónimo y de solo lectura en el que el contenido se consume a través de [!DNL GET] solicitudes:

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ]
}
```

El sitio 2 es más complejo y requiere solicitudes autorizadas y mutantes (POST, PUT, DELETE):

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Problemas y configuración del almacenamiento en caché de Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir de Dispatcher 4.1.1, los encabezados de respuesta se pueden almacenar en caché. Esto permite almacenar en caché [!DNL CORS] encabezados junto con los recursos solicitados por [!DNL CORS], siempre y cuando la solicitud sea anónima.

Por lo general, se pueden aplicar las mismas consideraciones para almacenar en caché el contenido en Dispatcher a los encabezados de respuesta CORS en caché en Dispatcher. La siguiente tabla define cuándo se pueden almacenar en caché los encabezados de [!DNL CORS] (y, por lo tanto, las solicitudes de [!DNL CORS]).

| Almacenable en caché | Entorno | Estado de autenticación | Explicación |
|-----------|-------------|-----------------------|-------------|
| No | Publicación de AEM | Autenticado | El almacenamiento en caché de Dispatcher en AEM Author está limitado a recursos estáticos no creados. Esto dificulta y hace poco práctico almacenar en caché la mayoría de los recursos en AEM Author, incluidos los encabezados de respuesta HTTP. |
| No | Publicación de AEM | Autenticado | Evite almacenar en caché los encabezados CORS en solicitudes autenticadas. Esto se ajusta a la guía común de no almacenar en caché las solicitudes autenticadas, ya que es difícil determinar cómo afectará el estado de autenticación/autorización del usuario solicitante al recurso enviado. |
| Sí | Publicación de AEM | Anónimo | Las solicitudes anónimas que se pueden almacenar en caché en Dispatcher también pueden tener sus encabezados de respuesta en caché, lo que garantiza que las futuras solicitudes CORS puedan acceder al contenido almacenado en caché. Cualquier cambio en la configuración CORS en la publicación de AEM **debe** ir seguido de una invalidación de los recursos en caché afectados. Las prácticas recomendadas dictan sobre las implementaciones de código o configuración cuando se purga la caché de Dispatcher, ya que es difícil determinar qué contenido en caché puede verse afectado. |

### Permitir encabezados de solicitud CORS

Para permitir que los [encabezados de solicitud HTTP necesarios pasen a AEM para el procesamiento](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), deben estar permitidos en la configuración `/clientheaders` de Dispatcher.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Almacenamiento en caché de encabezados de respuesta CORS

Para permitir el almacenamiento en caché y el servicio de encabezados CORS en contenido almacenado en caché, agregue la siguiente configuración [/cache /headers](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es#caching-http-response-headers) al archivo `dispatcher.any` de publicación de AEM.

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

Recuerde **reiniciar la aplicación del servidor web** después de realizar cambios en el archivo `dispatcher.any`.

Es probable que se necesite borrar la caché por completo para garantizar que los encabezados se almacenen correctamente en la caché en la siguiente solicitud después de una actualización de configuración de `/cache/headers`.

## Solución de problemas CORS

El registro está disponible en `com.adobe.granite.cors`:

* habilite `DEBUG` para ver los detalles de por qué se denegó una solicitud [!DNL CORS]
* habilitar a `TRACE` para ver los detalles de todas las solicitudes que pasan por el controlador CORS

### Sugerencias:

* Vuelva a crear manualmente las solicitudes XHR utilizando curl, pero asegúrese de copiar todos los encabezados y detalles, ya que cada uno puede marcar la diferencia; algunas consolas de explorador permiten copiar el comando curl
* Compruebe si el controlador CORS ha denegado la solicitud y no la autenticación, el filtro de token CSRF, los filtros de Dispatcher u otras capas de seguridad
   * Si el controlador CORS responde con 200, pero el encabezado `Access-Control-Allow-Origin` no aparece en la respuesta, revise los registros para ver si hay denegaciones en [!DNL DEBUG] en `com.adobe.granite.cors`
* Si el almacenamiento en caché de Dispatcher de [!DNL CORS] solicitudes está habilitado
   * Asegúrese de que la configuración de `/cache/headers` se aplique a `dispatcher.any` y de que el servidor web se reinicie correctamente
   * Asegúrese de que la caché se haya borrado correctamente después de cualquier cambio de configuración de OSGi o Dispatcher.any.
* si es necesario, compruebe la presencia de credenciales de autenticación en la solicitud.

## Materiales de apoyo

* [Fábrica de configuración de AEM OSGi para políticas de intercambio de recursos de origen cruzado](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Uso Compartido De Recursos De Origen Cruzado (W3C)](https://www.w3.org/TR/cors/)
* [Control de acceso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

---
title: AEM Comprender el Intercambio de Recursos de Origen Cruzado (CORS) con los usuarios de la red de
description: El Intercambio de Recursos de Origen Cruzado (CORS) de Adobe Experience Manager AEM AEM AEM facilita las propiedades web que no son de origen cruzado para hacer llamadas del lado del cliente a los usuarios, tanto a los que se autentican como a los que no se autentican, para recuperar contenido o interactuar directamente con los usuarios de la web de forma directa con los que no se puede acceder a la página de inicio de sesión de la página de inicio de la página de inicio de sesión.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
feature: Security, APIs
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: d2a9596ddadd897793a0fce8421aa8b246b45b12
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 2%

---

# Comprender el intercambio de recursos de origen cruzado ([!DNL CORS])

Intercambio de recursos de origen cruzado de Adobe Experience Manager ([!DNL CORS]AEM AEM AEM ) facilita que las propiedades web no realicen llamadas del lado del cliente a los usuarios, tanto a los que se han autenticado como a los que no se han autenticado, para recuperar contenido o interactuar directamente con los usuarios de la red de forma directa.

## Configuración de OSGi de la política de uso compartido de recursos de origen cruzado de Adobe Granite

AEM Las configuraciones de CORS se administran como fábricas de configuración OSGi en la práctica, y cada directiva se representa como una instancia de la fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuración de OSGi de la política de uso compartido de recursos de origen cruzado de Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selección de política

Se selecciona una política comparando las variables

* `Allowed Origin` con el `Origin` encabezado de solicitud
* y `Allowed Paths` con la ruta de solicitud.

Se utiliza la primera directiva que coincide con estos valores. Si no se encuentra ninguno, cualquier [!DNL CORS] se ha denegado la solicitud.

Si no se configura ninguna directiva, [!DNL CORS] Las solicitudes de no serán respondidas ya que el controlador está deshabilitado y, por lo tanto, denegado de forma efectiva, siempre y cuando ningún otro módulo del servidor responda a [!DNL CORS].

### Propiedades de directiva

#### [!UICONTROL Orígenes permitidos]

* `"alloworigin" <origin> | *`
* Lista de `origin` parámetros que especifican los URI que pueden acceder al recurso. Para solicitudes sin credenciales, el servidor puede especificar &#42; como comodín, permitiendo así que cualquier origen acceda al recurso. *No es absolutamente recomendable utilizar `Allow-Origin: *` en producción, ya que permite a cada sitio web extranjero (es decir, atacante) realizar solicitudes que sin CORS están estrictamente prohibidas por los navegadores.*

#### [!UICONTROL Orígenes permitidos (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de `regexp` expresiones regulares que especifican los URI que pueden acceder al recurso. *Las expresiones regulares pueden provocar coincidencias no deseadas si no se crean con cuidado, lo que permite a un atacante utilizar un nombre de dominio personalizado que también coincida con la directiva.* Por lo general, se recomienda tener políticas independientes para cada nombre de host de origen específico, utilizando `alloworigin`, incluso si esto implica una configuración repetida de las demás propiedades de directiva. Los distintos orígenes tienden a tener ciclos de vida y requisitos diferentes, por lo que se benefician de una separación clara.

#### [!UICONTROL Rutas permitidas]

* `"allowedpaths" <regexp>`
* Lista de `regexp` expresiones regulares que especifican rutas de recursos para las que se aplica la directiva.

#### [!UICONTROL Encabezados expuestos]

* `"exposedheaders" <header>`
* Lista de parámetros de encabezado que indican los encabezados de respuesta a los que los exploradores pueden acceder. Para las solicitudes CORS (no de verificación previa), si no están vacías, estos valores se copian en `Access-Control-Expose-Headers` encabezado de respuesta. A continuación, el explorador puede acceder a los valores de la lista (nombres de encabezado); sin ella, el explorador no puede leer esos encabezados.

#### [!UICONTROL Edad máxima]

* `"maxage" <seconds>`
* A `seconds` parámetro que indica cuánto tiempo se pueden almacenar en caché los resultados de una solicitud de verificación previa.

#### [!UICONTROL Encabezados admitidos]

* `"supportedheaders" <header>`
* Lista de `header` parámetros que indican qué encabezados de solicitud HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parámetros de método que indica qué métodos HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Admite credenciales]

* `"supportscredentials" <boolean>`
* A `boolean` que indica si la respuesta a la solicitud se puede exponer al explorador. Cuando se utiliza como parte de una respuesta a una solicitud de verificación previa al vuelo, esto indica si la solicitud real se puede realizar o no mediante credenciales.

### Configuraciones de ejemplo

El sitio 1 es un escenario básico, accesible de forma anónima y de solo lectura en el que el contenido se consume a través de [!DNL GET] solicita:

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
    "Access-Control-Request-Headers",
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

A partir de Dispatcher 4.1.1, los encabezados de respuesta se pueden almacenar en caché. Esto permite almacenar en caché [!DNL CORS] encabezados junto con el [!DNL CORS]Recursos no solicitados, siempre que la solicitud sea anónima.

Por lo general, las mismas consideraciones para almacenar en caché el contenido en Dispatcher se pueden aplicar al almacenamiento en caché de los encabezados de respuesta CORS en Dispatcher. La siguiente tabla define cuándo [!DNL CORS] encabezados (y, por lo tanto, [!DNL CORS] solicitudes) se pueden almacenar en caché.

| Almacenable en caché | Entorno | Estado de autenticación | Explicación |
|-----------|-------------|-----------------------|-------------|
| No | AEM Publish | Autenticado | El almacenamiento en caché de Dispatcher en AEM Author está limitado a recursos estáticos no creados. Esto hace que sea difícil y poco práctico almacenar en caché la mayoría de los recursos en AEM Author, incluidos los encabezados de respuesta HTTP. |
| No | AEM Publish | Autenticado | Evite almacenar en caché los encabezados CORS en solicitudes autenticadas. Esto se ajusta a la guía común de no almacenar en caché las solicitudes autenticadas, ya que es difícil determinar cómo afectará el estado de autenticación/autorización del usuario solicitante al recurso enviado. |
| Sí | AEM Publish | Anónimo | Las solicitudes anónimas que se pueden almacenar en caché en Dispatcher también pueden tener sus encabezados de respuesta en caché, lo que garantiza que las futuras solicitudes CORS puedan acceder al contenido almacenado en caché. Cualquier cambio en la configuración de CORS en AEM Publish **debe** ir seguido de una invalidación de los recursos en caché afectados. Las prácticas recomendadas dictan sobre las implementaciones de código o configuración cuando se purga la caché de Dispatcher, ya que es difícil determinar qué contenido en caché puede verse afectado. |

### Permitir encabezados de solicitud CORS

Para permitir el [AEM Encabezados de solicitud HTTP para pasarlos a la para su procesamiento](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), deben estar permitidos en el de Dispatcher `/clientheaders` configuración.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Almacenamiento en caché de encabezados de respuesta CORS

Para permitir el almacenamiento en caché y el servicio de encabezados CORS en contenido almacenado en caché, agregue lo siguiente [/cache /headers configuración](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=es#caching-http-response-headers) a AEM Publish `dispatcher.any` archivo.

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

Recuerde lo siguiente **reinicie la aplicación del servidor web** después de realizar cambios en el `dispatcher.any` archivo.

Es probable que se necesite borrar la caché por completo para garantizar que los encabezados se almacenen correctamente en la caché en la siguiente solicitud después de un `/cache/headers` actualización de configuración.

## Solución de problemas CORS

El registro está disponible en `com.adobe.granite.cors`:

* habilitar `DEBUG` para ver detalles sobre el motivo [!DNL CORS] se denegó la solicitud
* habilitar `TRACE` para ver detalles sobre todas las solicitudes que pasan por el controlador CORS

### Sugerencias:

* Vuelva a crear manualmente las solicitudes XHR utilizando curl, pero asegúrese de copiar todos los encabezados y detalles, ya que cada uno puede marcar la diferencia; algunas consolas de explorador permiten copiar el comando curl
* Compruebe si el controlador CORS ha denegado la solicitud y no la autenticación, el filtro de token CSRF, los filtros de Dispatcher u otras capas de seguridad
   * Si el controlador CORS responde con 200, pero `Access-Control-Allow-Origin` no haya ningún encabezado en la respuesta. Revise los registros para ver si hay denegaciones en [!DNL DEBUG] in `com.adobe.granite.cors`
* Si el almacenamiento en caché de Dispatcher [!DNL CORS] Las solicitudes de están habilitadas
   * Asegúrese de que `/cache/headers` La configuración de se aplica a `dispatcher.any` y el servidor web se reinicia correctamente
   * Asegúrese de que la caché se haya borrado correctamente después de cualquier cambio de configuración de OSGi o Dispatcher.any.
* si es necesario, compruebe la presencia de credenciales de autenticación en la solicitud.

## Materiales de apoyo

* [AEM Fábrica de configuración de OSGi para políticas de uso compartido de recursos de origen cruzado](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Intercambio de recursos de origen cruzado (W3C)](https://www.w3.org/TR/cors/)
* [Control de acceso HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

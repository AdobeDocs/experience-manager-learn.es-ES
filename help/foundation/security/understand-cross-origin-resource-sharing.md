---
title: Comprender el uso compartido de recursos de origen cruzado (CORS) con AEM
description: El uso compartido de recursos de origen cruzado (CORS) de Adobe Experience Manager facilita las propiedades web no AEM para realizar llamadas del lado del cliente a AEM, tanto autenticadas como no autenticadas, para recuperar contenido o interactuar directamente con AEM.
version: 6,4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 1%

---

# Comprender el uso compartido de recursos de origen cruzado ([!DNL CORS])

Uso compartido de recursos de origen cruzado de Adobe Experience Manager ([!DNL CORS]) facilita las propiedades web no AEM para realizar llamadas del lado del cliente a AEM, tanto autenticadas como no autenticadas, para recuperar contenido o interactuar directamente con AEM.

## Política de uso compartido de recursos de origen cruzado de Adobe Granite Configuración OSGi

Las configuraciones de CORS se administran como fábricas de configuración de OSGi en AEM, y cada directiva se representa como una instancia de fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Política de uso compartido de recursos de origen cruzado de Adobe Granite Configuración OSGi](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selección de políticas

Una directiva se selecciona comparando la variable

* `Allowed Origin` con la variable `Origin` encabezado de solicitud
* y `Allowed Paths` con la ruta de solicitud.

Se utilizará la primera directiva que coincida con estos valores. Si no se encuentra ninguno, se muestra [!DNL CORS] se denegará.

Si no hay ninguna directiva configurada, [!DNL CORS] tampoco se responderán las solicitudes, ya que el controlador se desactivará y, por lo tanto, se denegará de forma efectiva, siempre que ningún otro módulo del servidor responda [!DNL CORS].

### Propiedades de directiva

#### [!UICONTROL Orígenes permitidos]

* `"alloworigin" <origin> | *`
* Lista de `origin` parámetros que especifican URIs que pueden acceder al recurso. Para solicitudes sin credenciales, el servidor puede especificar &#42; como comodín, permitiendo así que cualquier origen acceda al recurso. *No se recomienda su uso `Allow-Origin: *` en producción, ya que permite a cada sitio web extranjero (es decir, atacante) realizar solicitudes de que sin CORS estén estrictamente prohibidas por los navegadores.*

#### [!UICONTROL Orígenes permitidos (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de `regexp` expresiones regulares que especifican los URI que pueden acceder al recurso. *Las expresiones regulares pueden provocar coincidencias no deseadas si no se crean cuidadosamente, lo que permite que un atacante utilice un nombre de dominio personalizado que también coincida con la política.* Por lo general, se recomienda tener directivas independientes para cada nombre de host de origen específico, utilizando `alloworigin`, incluso si eso significa una configuración repetida de las otras propiedades de directiva. Los diferentes orígenes tienden a tener diferentes ciclos de vida y requisitos, beneficiándose así de una clara separación.

#### [!UICONTROL Rutas permitidas]

* `"allowedpaths" <regexp>`
* Lista de `regexp` expresiones regulares que especifican rutas de recursos para las que se aplica la directiva.

#### [!UICONTROL Encabezados expuestos]

* `"exposedheaders" <header>`
* Lista de parámetros de encabezado que indican encabezados de solicitud a los que se permite acceder a los navegadores.

#### [!UICONTROL Edad máxima]

* `"maxage" <seconds>`
* A `seconds` que indica cuánto tiempo se pueden almacenar en caché los resultados de una solicitud de pre-vuelo.

#### [!UICONTROL Encabezados admitidos]

* `"supportedheaders" <header>`
* Lista de `header` parámetros que indican qué encabezados HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parámetros de método que indican qué métodos HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Admite credenciales]

* `"supportscredentials" <boolean>`
* A `boolean` indicando si la respuesta a la solicitud se puede exponer o no al explorador. Cuando se utiliza como parte de una respuesta a una solicitud de pre-vuelo, indica si la solicitud real se puede realizar o no con credenciales.

### Ejemplos de configuraciones

El sitio 1 es un escenario básico, accesible de forma anónima y de solo lectura en el que el contenido se consume mediante [!DNL GET] solicitudes:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

El sitio 2 es más complejo y requiere solicitudes autorizadas e inseguras:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Preocupaciones y configuración del almacenamiento en caché de Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir de Dispatcher 4.1.1+, los encabezados de respuesta se pueden almacenar en caché. Esto permite almacenar en caché [!DNL CORS] encabezados a lo largo de la sección [!DNL CORS]-recursos solicitados, siempre que la solicitud sea anónima.

Por lo general, las mismas consideraciones para almacenar en caché el contenido en Dispatcher se pueden aplicar al almacenamiento en caché de los encabezados de respuesta CORS en Dispatcher. La tabla siguiente define cuándo [!DNL CORS] encabezados (y [!DNL CORS] ) se pueden almacenar en caché.

| Cacheable | creación | Estado de autenticación | Explicación |
|-----------|-------------|-----------------------|-------------|
| No | AEM Publish | Autenticado | El almacenamiento en caché de Dispatcher en AEM Author se limita a recursos estáticos no creados. Esto hace que sea difícil y poco práctico almacenar en caché la mayoría de los recursos en AEM Author, incluidos los encabezados de respuesta HTTP. |
| No | AEM Publish | Autenticado | Evite almacenar en caché los encabezados CORS en solicitudes autenticadas. Esto se ajusta a la guía común de no almacenar en caché las solicitudes autenticadas, ya que es difícil determinar cómo afectará el estado de autenticación/autorización del usuario solicitante al recurso entregado. |
| Sí | AEM Publish | Anónimo | Las solicitudes anónimas que se pueden almacenar en caché en Dispatcher también pueden almacenar en caché sus encabezados de respuesta, lo que garantiza que las futuras solicitudes CORS puedan acceder al contenido almacenado en caché. Cualquier cambio en la configuración de CORS en AEM Publish **must** a continuación se invalida la parte de los recursos en caché afectados. Las prácticas recomendadas dictan en las implementaciones de código o configuración que se depura la caché de Dispatcher, ya que es difícil determinar qué contenido almacenado en caché se puede realizar. |

Para permitir el almacenamiento en caché de los encabezados CORS, agregue la siguiente configuración a todos los archivos compatibles con Dispatcher.any de AEM Publish.

```
/cache { 
  ...
  /headers {
      "Origin"
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

Recuerde **reiniciar la aplicación de servidor web** después de realizar cambios en `dispatcher.any` archivo.

Es probable que se borre la caché por completo para garantizar que los encabezados se almacenen correctamente en caché en la siguiente solicitud después de una `/cache/headers` actualización de configuración.

## Resolución de problemas de CORS

El registro está disponible en `com.adobe.granite.cors`:

* enable `DEBUG` para ver detalles sobre por qué un [!DNL CORS] solicitud denegada
* enable `TRACE` para ver detalles sobre todas las solicitudes que pasan por el controlador CORS

### Sugerencias:

* Volver a crear manualmente solicitudes XHR utilizando curl, pero asegúrese de copiar todos los encabezados y detalles, ya que cada uno puede marcar una diferencia; algunas consolas de navegador permiten copiar el comando curl
* Verifique si la solicitud fue denegada por el controlador CORS y no por la autenticación, el filtro de token CSRF, los filtros de Dispatcher u otras capas de seguridad
   * Si el controlador CORS responde con 200, pero `Access-Control-Allow-Origin` el encabezado está ausente en la respuesta, revise los registros para denegaciones en [!DNL DEBUG] en `com.adobe.granite.cors`
* Si el almacenamiento en caché del despachante de [!DNL CORS] solicitudes está activada
   * Asegúrese de que la variable `/cache/headers` la configuración se aplica a `dispatcher.any` y el servidor web se reinicia correctamente
   * Asegúrese de que la caché se borró correctamente después de cualquier cambio de configuración de OSGi o dispatcher.any.
* si es necesario, compruebe la presencia de credenciales de autenticación en la solicitud.

## Materiales de apoyo

* [AEM fábrica de configuración OSGi para directivas de intercambio de recursos de origen cruzado](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Uso compartido de recursos de origen cruzado (W3C)](https://www.w3.org/TR/cors/)
* [Control de acceso HTTP (MDN Mozilla)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

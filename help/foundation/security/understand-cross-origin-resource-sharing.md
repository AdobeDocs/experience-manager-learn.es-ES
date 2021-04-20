---
title: Comprender el uso compartido de recursos de origen cruzado (CORS) con AEM
description: El uso compartido de recursos de origen cruzado (CORS) de Adobe Experience Manager facilita las propiedades web que no son de AEM para realizar llamadas del lado del cliente a AEM, tanto autenticadas como no autenticadas, para recuperar contenido o interactuar directamente con AEM.
version: 6.3, 6,4, 6.5
sub-product: fundación, servicios de contenido, sitios
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 1%

---


# Comprender el uso compartido de recursos de origen cruzado ([!DNL CORS])

El uso compartido de recursos de origen cruzado ([!DNL CORS]) de Adobe Experience Manager facilita las propiedades web que no son de AEM para realizar llamadas del lado del cliente a AEM, tanto autenticadas como no autenticadas, para recuperar contenido o interactuar directamente con AEM.

## Configuración OSGi de la política de uso compartido de recursos de origen cruzado de Adobe Granite

Las configuraciones de CORS se administran como fábricas de configuración de OSGi en AEM, y cada directiva se representa como una instancia de fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuración OSGi de la política de uso compartido de recursos de origen cruzado de Adobe Granite](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selección de políticas

Una directiva se selecciona comparando la variable

* `Allowed Origin` con el encabezado de la  `Origin` solicitud
* y `Allowed Paths` con la ruta de solicitud.

Se utilizará la primera directiva que coincida con estos valores. Si no se encuentra ninguna, se denegará cualquier solicitud [!DNL CORS].

Si no hay ninguna directiva configurada, las solicitudes [!DNL CORS] tampoco se contestarán, ya que el controlador se desactivará y, por lo tanto, se denegará de forma efectiva, siempre que ningún otro módulo del servidor responda a [!DNL CORS].

### Propiedades de directiva

#### [!UICONTROL Orígenes permitidos]

* `"alloworigin" <origin> | *`
* Lista de parámetros `origin` que especifican los URI que pueden acceder al recurso. Para las solicitudes sin credenciales, el servidor puede especificar * como comodín, lo que permite que cualquier origen acceda al recurso. *No se recomienda utilizar  `Allow-Origin: *` en producción, ya que permite a todos los sitios web extranjeros (es decir, atacantes) realizar solicitudes de que los navegadores prohíban estrictamente el uso de CORS sin ningún tipo de CORS.*

#### [!UICONTROL Orígenes permitidos (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de expresiones regulares `regexp` que especifican los URI que pueden acceder al recurso. *Las expresiones regulares pueden provocar coincidencias no deseadas si no se crean cuidadosamente, lo que permite que un atacante utilice un nombre de dominio personalizado que también coincida con la política.* Por lo general, se recomienda tener directivas independientes para cada nombre de host de origen específico, utilizando  `alloworigin`, incluso si esto significa una configuración repetida de las otras propiedades de directiva. Los diferentes orígenes tienden a tener diferentes ciclos de vida y requisitos, beneficiándose así de una clara separación.

#### [!UICONTROL Rutas permitidas]

* `"allowedpaths" <regexp>`
* Lista de expresiones regulares `regexp` que especifican las rutas de recursos para las que se aplica la directiva.

#### [!UICONTROL Encabezados expuestos]

* `"exposedheaders" <header>`
* Lista de parámetros de encabezado que indican encabezados de solicitud a los que se permite acceder a los navegadores.

#### [!UICONTROL Edad máxima]

* `"maxage" <seconds>`
* Un parámetro `seconds` que indica cuánto tiempo se pueden almacenar en caché los resultados de una solicitud de pre-vuelo.

#### [!UICONTROL Encabezados admitidos]

* `"supportedheaders" <header>`
* Lista de parámetros `header` que indican qué encabezados HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parámetros de método que indican qué métodos HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Admite credenciales]

* `"supportscredentials" <boolean>`
* Un `boolean` que indica si la respuesta a la solicitud se puede exponer al explorador. Cuando se utiliza como parte de una respuesta a una solicitud de pre-vuelo, indica si la solicitud real se puede realizar o no con credenciales.

### Ejemplos de configuraciones

El sitio 1 es un escenario básico, accesible de forma anónima y de solo lectura donde el contenido se consume mediante solicitudes [!DNL GET]:

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

A partir de Dispatcher 4.1.1+, los encabezados de respuesta se pueden almacenar en caché. Esto permite almacenar en caché los encabezados [!DNL CORS] junto con los recursos [!DNL CORS] solicitados, siempre que la solicitud sea anónima.

Por lo general, las mismas consideraciones para almacenar en caché el contenido en Dispatcher se pueden aplicar al almacenamiento en caché de los encabezados de respuesta CORS en Dispatcher. La siguiente tabla define cuándo se pueden almacenar en caché los encabezados [!DNL CORS] (y por lo tanto [!DNL CORS] solicitudes).

| Cacheable | creación | Estado de autenticación | Explicación |
|-----------|-------------|-----------------------|-------------|
| No | AEM Publish | Autenticado | El almacenamiento en caché de Dispatcher en AEM Author se limita a recursos estáticos no creados. Esto hace que sea difícil y poco práctico almacenar en caché la mayoría de los recursos en AEM Author, incluidos los encabezados de respuesta HTTP. |
| No | AEM Publish | Autenticado | Evite almacenar en caché los encabezados CORS en solicitudes autenticadas. Esto se ajusta a la guía común de no almacenar en caché las solicitudes autenticadas, ya que es difícil determinar cómo afectará el estado de autenticación/autorización del usuario solicitante al recurso entregado. |
| Sí | AEM Publish | Anónimo | Las solicitudes anónimas que se pueden almacenar en caché en Dispatcher también pueden almacenar en caché sus encabezados de respuesta, lo que garantiza que las futuras solicitudes CORS puedan acceder al contenido almacenado en caché. Cualquier cambio en la configuración de CORS en AEM Publish **debe** ir seguido de una invalidación de los recursos en caché afectados. Las prácticas recomendadas dictan en las implementaciones de código o configuración que se depura la caché de Dispatcher, ya que es difícil determinar qué contenido almacenado en caché se puede realizar. |

Para permitir el almacenamiento en caché de los encabezados CORS, agregue la siguiente configuración a todos los archivos compatibles con Dispatcher.any de AEM Publish.

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

Recuerde **reiniciar la aplicación del servidor web** después de realizar cambios en el archivo `dispatcher.any`.

Es probable que se borre la caché por completo para garantizar que los encabezados se almacenen correctamente en caché en la siguiente solicitud después de una actualización de configuración `/clientheaders`.

## Resolución de problemas de CORS

El registro está disponible en `com.adobe.granite.cors`:

* habilitar `DEBUG` para ver detalles sobre por qué se denegó una solicitud [!DNL CORS]
* habilitar `TRACE` para ver detalles sobre todas las solicitudes que pasan por el controlador CORS

### Sugerencias:

* Volver a crear manualmente solicitudes XHR utilizando curl, pero asegúrese de copiar todos los encabezados y detalles, ya que cada uno puede marcar una diferencia; algunas consolas de navegador permiten copiar el comando curl
* Verifique si la solicitud fue denegada por el controlador CORS y no por la autenticación, el filtro de token CSRF, los filtros de Dispatcher u otras capas de seguridad
   * Si el controlador CORS responde con 200, pero el encabezado `Access-Control-Allow-Origin` está ausente en la respuesta, revise los registros para las denegaciones en [!DNL DEBUG] en `com.adobe.granite.cors`
* Si el almacenamiento en caché de Dispatcher de [!DNL CORS] solicitudes está habilitado
   * Asegúrese de que la configuración `/clientheaders` se aplica a `dispatcher.any` y que el servidor web se reinicia correctamente
   * Asegúrese de que la caché se borró correctamente después de cualquier cambio de configuración de OSGi o dispatcher.any.
* si es necesario, compruebe la presencia de credenciales de autenticación en la solicitud.

## Materiales de apoyo

* [Fábrica de configuración de AEM OSGi para políticas de intercambio de recursos de origen cruzado](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Uso compartido de recursos de origen cruzado (W3C)](https://www.w3.org/TR/cors/)
* [Control de acceso HTTP (MDN Mozilla)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

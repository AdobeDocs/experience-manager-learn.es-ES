---
title: Comprender el uso compartido de recursos entre Orígenes (CORS) con AEM
description: El uso compartido de recursos entre Orígenes (CORS, Cross- Resource Sharing ) de Adobe Experience Manager facilita las propiedades web no AEM para realizar llamadas del cliente a AEM, tanto autenticadas como no autenticadas, para recuperar contenido o interactuar directamente con AEM.
version: 6.3, 6,4, 6.5
sub-product: fundación, servicios de contenido, sitios
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# Comprender el uso compartido de recursos entre Orígenes ([!DNL CORS])

El uso compartido de recursos entre Orígenes ([!DNL CORS]) de Adobe Experience Manager facilita las propiedades web no AEM para realizar llamadas del cliente a AEM, autenticadas y no autenticadas, para recuperar contenido o interactuar directamente con AEM.

## Configuración de OSGi de la directiva de uso compartido de recursos entre Orígenes de granito Adobe

Las configuraciones de CORS se administran como fábricas de configuración de OSGi en AEM, y cada directiva se representa como una instancia de fábrica.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Configuración de OSGi de la directiva de uso compartido de recursos entre Orígenes de granito Adobe](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Selección de directivas

Una directiva se selecciona comparando la variable

* `Allowed Origin` con el encabezado de la `Origin` solicitud
* y `Allowed Paths` con la ruta de solicitud.

Se utilizará la primera directiva que coincida con estos valores. Si no se encuentra ninguna, se denegará cualquier [!DNL CORS] solicitud.

Si no hay ninguna directiva configurada, tampoco se responderá a las solicitudes, ya que el controlador se desactivará y, por lo tanto, se denegará de forma efectiva, siempre que ningún otro módulo del servidor responda a [!DNL CORS] [!DNL CORS].

### Propiedades de directiva

#### [!UICONTROL Orígenes permitidos]

* `"alloworigin" <origin> | *`
* Lista de `origin` parámetros que especifican URI que pueden acceder al recurso. Para las solicitudes sin credenciales, el servidor puede especificar * como comodín, permitiendo así que cualquier origen pueda acceder al recurso. *No se recomienda usar`Allow-Origin: *`en producción, ya que permite a todos los sitios web extranjeros (es decir, atacantes) realizar solicitudes que los navegadores prohíban estrictamente sin CORS.*

#### [!UICONTROL Orígenes permitidos (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lista de expresiones `regexp` regulares que especifican URI que pueden acceder al recurso. *Las expresiones regulares pueden llevar a coincidencias no deseadas si no se crean cuidadosamente, lo que permite que un atacante utilice un nombre de dominio personalizado que también coincida con la política.* Por lo general, se recomienda tener directivas independientes para cada nombre de host de origen específico, utilizando `alloworigin`, incluso si esto significa una configuración repetida de las demás propiedades de directiva. Los diferentes orígenes tienden a tener diferentes ciclos de vida y requisitos, por lo que se benefician de una clara separación.

#### [!UICONTROL Rutas permitidas]

* `"allowedpaths" <regexp>`
* Lista de expresiones `regexp` regulares que especifican rutas de recursos para las que se aplica la política.

#### [!UICONTROL Encabezados expuestos]

* `"exposedheaders" <header>`
* Lista de parámetros de encabezado que indican encabezados de solicitud a los que los navegadores pueden acceder.

#### [!UICONTROL Edad máxima]

* `"maxage" <seconds>`
* Parámetro `seconds` que indica cuánto tiempo se pueden almacenar en caché los resultados de una solicitud de verificación previa.

#### [!UICONTROL Encabezados admitidos]

* `"supportedheaders" <header>`
* Lista de `header` parámetros que indican qué encabezados HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Métodos permitidos]

* `"supportedmethods"`
* Lista de parámetros de método que indican qué métodos HTTP se pueden utilizar al realizar la solicitud real.

#### [!UICONTROL Admite credenciales]

* `"supportscredentials" <boolean>`
* Un `boolean` indicador que indica si la respuesta a la solicitud se puede exponer al explorador. Cuando se utiliza como parte de una respuesta a una solicitud de verificación previa, indica si la solicitud real se puede realizar o no con credenciales.

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

El sitio 2 es más complejo y requiere solicitudes autorizadas y no seguras:

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

## Problemas y configuración de almacenamiento en caché de Dispatcher {#dispatcher-caching-concerns-and-configuration}

A partir de Dispatcher 4.1.1+, los encabezados de respuesta se pueden almacenar en caché. Esto permite almacenar en caché [!DNL CORS] los encabezados junto con los recursos [!DNL CORS]solicitados, siempre que la solicitud sea anónima.

Generalmente, las mismas consideraciones para almacenar contenido en caché en Dispatcher se pueden aplicar a los encabezados de respuesta CORS en caché en el despachante. La siguiente tabla define cuándo se pueden almacenar en caché [!DNL CORS] los encabezados (y, por lo tanto, [!DNL CORS] las solicitudes).

| En caché | creación | Estado de autenticación | Explicación |
|-----------|-------------|-----------------------|-------------|
| No | AEM Publish | Autenticado | El almacenamiento en caché del despachante en AEM Author está limitado a recursos estáticos y no creados. Esto dificulta y dificulta la caché de la mayoría de los recursos en AEM Author, incluidos los encabezados de respuesta HTTP. |
| No | AEM Publish | Autenticado | Evite almacenar en caché los encabezados CORS en solicitudes autenticadas. Esto se ajusta a la guía común de no almacenar en caché las solicitudes autenticadas, ya que es difícil determinar cómo afectará el estado de autenticación/autorización del usuario solicitante al recurso entregado. |
| Sí | AEM Publish | Anónimo | Las solicitudes anónimas que se pueden almacenar en caché en el despachante también pueden tener sus encabezados de respuesta en caché, lo que garantiza que las futuras solicitudes CORS puedan acceder al contenido almacenado en caché. Cualquier cambio de configuración de CORS en AEM Publish **debe** ir seguido de una invalidación de los recursos en caché afectados. Las optimizaciones dictan en implementaciones de código o configuración que se depura la caché del despachante, ya que es difícil determinar qué contenido en caché se puede realizar. |

Para permitir el almacenamiento en caché de encabezados CORS, agregue la siguiente configuración a todos los archivos compatibles con Dispatcher.any de AEM Publish.

```
/cache { 
  ...
  /headers {
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

Recuerde **reiniciar la aplicación** de servidor web después de realizar cambios en el `dispatcher.any` archivo.

Es probable que se borre la caché por completo para garantizar que los encabezados se almacenen correctamente en la caché en la siguiente solicitud después de una actualización de la `/headers` configuración.

## Resolución de problemas de CORS

El registro está disponible en `com.adobe.granite.cors`:

* habilitar `DEBUG` para ver los detalles de por qué se denegó una [!DNL CORS] solicitud
* habilitar `TRACE` para ver detalles sobre todas las solicitudes que pasan por el controlador CORS

### Sugerencias:

* Volver a crear manualmente solicitudes XHR con curl, pero asegúrese de copiar todos los encabezados y detalles, ya que cada uno puede marcar una diferencia; algunas consolas de navegador permiten copiar el comando curl
* Compruebe si el controlador CORS denegó la solicitud y no la autenticación, el filtro de token CSRF, los filtros de distribuidor u otros niveles de seguridad
   * Si el controlador CORS responde con 200 pero `Access-Control-Allow-Origin` el encabezado no está presente en la respuesta, revise los registros para ver si hay denegaciones [!DNL DEBUG] en `com.adobe.granite.cors`
* Si el despachante almacena en caché [!DNL CORS] las solicitudes está habilitado
   * Asegúrese de que la `/headers` configuración se aplica a `dispatcher.any` y de que el servidor web se reinicia correctamente
   * Asegúrese de que la caché se borró correctamente después de cualquier cambio de configuración de OSGi o dispatcher.
* si es necesario, compruebe la presencia de credenciales de autenticación en la solicitud.

## Materiales de apoyo

* [AEM fábrica de configuración de OSGi para directivas de uso compartido de recursos entre Orígenes](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Uso compartido de recursos entre Orígenes (W3C)](https://www.w3.org/TR/cors/)
* [CONTROL DE ACCESO HTTP (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

---
title: AEM Configuración de CORS para el uso de GraphQL en
description: AEM Obtenga información sobre cómo configurar el uso compartido de recursos de origen cruzado (CORS) para su uso con GraphQL de la.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
source-git-commit: 36b4217a899b462296d4389bc96a644da97d5da4
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 1%

---

# Uso compartido de recursos de origen cruzado (CORS)

El Intercambio de Recursos de Origen Cruzado (CORS) de Adobe Experience Manager as a Cloud Service AEM AEM facilita que las propiedades web que no son de origen cruzado realicen llamadas del lado del cliente basadas en el explorador a las API de GraphQL de la.

El siguiente artículo describe cómo configurar _de un solo origen_ AEM Acceso a un conjunto específico de extremos sin encabezado mediante CORS. AEM AEM Un solo origen significa que solo se accede a un dominio no único, por ejemplo, a través de la conexión de https://app.example.com con https://www.example.com. Es posible que el acceso de varios orígenes no funcione con este método debido a problemas de almacenamiento en caché.

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para que se ajusten a los requisitos del proyecto.

## Requisito CORS

AEM CORS es necesario para las conexiones basadas en el explorador a las API de GraphQL AEM AEM, cuando el cliente que se conecta a la NO se proporciona desde el mismo origen (también conocido como host o dominio) que el que se proporciona a las API de.

| Tipo de cliente | [SPA Aplicación de una sola página ()](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [De servidor a servidor](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requiere la configuración CORS | ✔ | ✔ | ✘ | ✘ |

## Configuración de OSGi

AEM El generador de configuración OSGi de CORS de define los criterios de permiso para aceptar solicitudes HTTP CORS.

| El cliente se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración OSGi de CORS | ✔ | ✔ | ✔ |


El ejemplo siguiente define una configuración OSGi para AEM Publish (`../config.publish/..`), pero se puede agregar a [cualquier carpeta de modo de ejecución compatible](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Las propiedades de configuración clave son:

+ `alloworigin` y/o `alloworiginregexp` AEM especifica los orígenes en los que se conecta el cliente a las ejecuciones web de la.
+ `allowedpaths` especifica los patrones de ruta de URL permitidos a partir de los orígenes especificados.
   + AEM Para admitir consultas persistentes de GraphQL, agregue el siguiente patrón: `/graphql/execute.json.*`
   + Para admitir fragmentos de experiencias, agregue el siguiente patrón: `/content/experience-fragments/.*`
+ `supportedmethods` especifica los métodos HTTP permitidos para las solicitudes CORS. Añadir `GET`AEM , para admitir consultas persistentes (y Fragmentos de experiencias) de GraphQL de la.

[Obtenga más información sobre la configuración OSGi de CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

AEM Esta configuración de ejemplo admite el uso de consultas persistentes de GraphQL de. Para utilizar consultas de GraphQL definidas por el cliente, agregue una URL de extremo de GraphQL en `allowedpaths` y `POST` hasta `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### AEM Solicitudes autorizadas de API de GraphQL

AEM Al acceder a las API de GraphQL que requieren autorización (normalmente, AEM Author o contenido protegido en AEM Publish), asegúrese de que la configuración de CORS OSGi tenga los valores adicionales:

+ `supportedheaders` también enumera `"Authorization"`
+ `supportscredentials` se establece en `true`

AEM Las solicitudes autorizadas a las API de GraphQL que requieren la configuración de CORS son inusuales, ya que generalmente se producen en el contexto de [aplicaciones de servidor a servidor](../server-to-server.md) y, por lo tanto, no requieren la configuración de CORS. Aplicaciones basadas en explorador que requieren configuraciones CORS, como [aplicaciones de una sola página](../spa.md) o [Componentes web](../web-component.md), normalmente utiliza la autorización, ya que es difícil asegurar las credenciales

Por ejemplo, estas dos configuraciones se establecen de la siguiente manera en una `CORSPolicyImpl` Configuración de fábrica de OSGi:

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### Ejemplo de configuración de OSGi

+ [Se puede encontrar un ejemplo de la configuración OSGi en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Configuración de Dispatcher

Dispatcher del servicio de publicación (y previsualización) de AEM debe configurarse para admitir CORS.

| El cliente se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración CORS de Dispatcher | ✘ | ✔ | ✔ |

### Permitir encabezados CORS en solicitudes HTTP

Actualice el `clientheaders.any` para permitir los encabezados de solicitud HTTP `Origin`,  `Access-Control-Request-Method`, y `Access-Control-Request-Headers` AEM para pasarse a los usuarios de, lo que permite que la petición HTTP sea procesada por el usuario de [AEM Configuración de CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Ejemplo de configuración de Dispatcher

+ [Un ejemplo de Dispatcher _encabezados de cliente_ La configuración de se puede encontrar en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Envío de encabezados de respuesta HTTP CORS

Configurar la granja de Dispatcher para almacenar en caché **Encabezados de respuesta HTTP CORS** AEM para asegurarse de que se incluyen cuando las consultas persistentes de GraphQL se proporcionan desde la caché de Dispatcher agregando la variable `Access-Control-...` a la lista de encabezados de caché.

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

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

#### Ejemplo de configuración de Dispatcher

+ [Un ejemplo de Dispatcher _Encabezados de respuesta HTTP CORS_ La configuración de se puede encontrar en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)

---
title: Configuración CORS para AEM GraphQL
description: Aprenda a configurar el uso compartido de recursos de origen cruzado (CORS) para utilizarlo con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 1%

---


# Uso compartido de recursos de origen diverso (CORS)

El uso compartido de recursos de origen cruzado (CORS) de Adobe Experience Manager as a Cloud Service facilita las propiedades web no AEM para realizar llamadas de cliente basadas en explorador a las API de GraphQL de AEM.

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para adaptarlos a los requisitos del proyecto.

## Requisito de CORS

El CORS es necesario para conexiones basadas en navegador a API de AEM GraphQL, cuando el cliente que se conecta a AEM NO se suministra desde el mismo origen (también conocido como host o dominio) que AEM.

| Tipo de cliente | [Aplicación de una sola página (SPA)](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requiere configuración CORS | š | š | ü | ü |

## Configuración de OSGi

La fábrica de configuración AEM CORS OSGi define los criterios de permitidos para aceptar solicitudes HTTP CORS.

| El cliente se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración CORS OSGi | š | š | š |


El ejemplo siguiente define una configuración OSGi para AEM Publish (`../config.publish/..`), pero se puede agregar a [cualquier carpeta de modo de ejecución compatible](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

Las propiedades de configuración clave son:

+ `alloworigin` y/o `alloworiginregexp` especifica los orígenes en los que se ejecuta el cliente que se conecta a AEM web.
+ `allowedpaths` especifica los patrones de ruta de URL permitidos desde los orígenes especificados. Para admitir AEM consultas persistentes de GraphQL, utilice el siguiente patrón: `"/graphql/execute.json.*"`
+ `supportedmethods` especifica los métodos HTTP permitidos para las solicitudes CORS. Agregar `GET`, para admitir AEM consultas persistentes de GraphQL.

[Obtenga más información sobre la configuración de OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)


Esta configuración de ejemplo admite el uso de AEM consultas persistentes de GraphQL. Para utilizar consultas de GraphQL definidas por el cliente, agregue una URL de extremo de GraphQL en `allowedpaths` y `POST` a `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
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
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Solicitudes de API autorizadas de AEM GraphQL

Cuando acceda a AEM API de GraphQL que requieren autorización (normalmente Autor de AEM o contenido protegido en AEM Publish), asegúrese de que la configuración de CORS OSGi tenga los valores adicionales:

+ `supportedheaders` listas también `"Authorization"`
+ `supportscredentials` está configurado como `true`

Las solicitudes autorizadas a AEM API de GraphQL que requieren la configuración de CORS son inusuales, ya que normalmente se producen en el contexto de [aplicaciones de servidor a servidor](../server-to-server.md) y, por lo tanto, no requieren la configuración de CORS. Aplicaciones basadas en navegadores que requieren configuraciones CORS, como [aplicaciones de una sola página](../spa.md) o [Componentes web](../web-component.md), normalmente utilizan autorización porque es difícil proteger las credenciales .

Por ejemplo, estas dos configuraciones se establecen de la siguiente manera en un `CORSPolicyImpl` Configuración de fábrica de OSGi:

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

+ [Puede encontrar un ejemplo de la configuración OSGi en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Configuración de Dispatcher

Dispatcher del servicio AEM Publish (y Preview) debe estar configurado para admitir CORS.

| El cliente se conecta a | Autor de AEM | AEM Publish | Vista previa de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración de Dispatcher CORS | ü | š | š |

### Permitir encabezados CORS en solicitudes HTTP

Actualice el `clientheaders.any` para permitir los encabezados de solicitud HTTP `Origin`,  `Access-Control-Request-Method`y `Access-Control-Request-Headers` para pasarse a AEM, lo que permite que la solicitud HTTP la procese el [Configuración de AEM CORS](#osgi-configuration).

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

+ [Ejemplo de Dispatcher _encabezados de cliente_ se puede encontrar en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### Envío de encabezados de respuesta HTTP CORS

Configurar la granja de Dispatcher en caché **Encabezados de respuesta HTTP CORS** para asegurarse de que se incluyen cuando AEM consultas persistentes de GraphQL se proporcionan desde la caché de Dispatcher añadiendo la variable `Access-Control-...` encabezados a la lista de encabezados de caché.

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

+ [Ejemplo de Dispatcher _Encabezados de respuesta HTTP CORS_ se puede encontrar en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)

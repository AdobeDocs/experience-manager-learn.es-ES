---
title: Configuración CORS para AEM GraphQL
description: Aprenda a configurar el uso compartido de recursos de origen cruzado (CORS) para utilizarlo con AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 2%

---


# Uso compartido de recursos de origen diverso (CORS)

El uso compartido de recursos de origen cruzado (CORS) de Adobe Experience Manager as a Cloud Service facilita las propiedades web no AEM para realizar llamadas de cliente basadas en explorador a las API de GraphQL de AEM.

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para adaptarlos a los requisitos del proyecto.



## Requisito de CORS

El CORS es necesario para conexiones basadas en navegador a API de AEM GraphQL, cuando el cliente que se conecta a AEM NO se suministra desde el mismo origen (también conocido como host o dominio) que AEM.

| Tipo de cliente | Aplicación de una sola página (SPA) | Componente web/JS | Móvil | Servidor a servidor |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requiere configuración CORS | š | š | ü | ü |

## Configuración de OSGi

La fábrica de configuración AEM CORS OSGi define los criterios de permitidos para aceptar solicitudes HTTP CORS.

| El cliente se conecta a | AEM Author | AEM Publish | Vista previa de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración CORS OSGi | š | š | š |


El ejemplo siguiente define una configuración OSGi para AEM Publish (`../config.publish/..`), pero se puede agregar a [cualquier carpeta runmode admitida](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

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


## Configuración de Dispatcher

Dispatcher del servicio AEM Publish (y Preview) debe estar configurado para admitir CORS.

| El cliente se conecta a | Autor de AEM | AEM Publish | Vista previa de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración de Dispatcher CORS | ü | š | š |

### Permitir encabezados CORS en solicitudes HTTP

Actualice el `clientheaders.any` para permitir los encabezados de solicitud HTTP `Origin`,  `Access-Control-Request-Method` y `Access-Control-Request-Headers` para pasarse a AEM, lo que permite que la solicitud HTTP la procese el [Configuración de AEM CORS](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

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

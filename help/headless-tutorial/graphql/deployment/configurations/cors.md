---
title: AEM Configuración de CORS para el uso de GraphQL en
description: AEM Obtenga información sobre cómo configurar el uso compartido de recursos de origen cruzado (CORS) para su uso con GraphQL de la.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2023-08-08T00:00:00Z
duration: 240
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 2%

---

# Uso compartido de recursos de origen cruzado (CORS)

El Intercambio de Recursos de Origen Cruzado (CORS) de Adobe Experience Manager as a Cloud Service AEM AEM facilita que las propiedades web que no son de origen cruzado realicen llamadas del lado del cliente basadas en el explorador a las API de GraphQL AEM de la y a otros recursos sin encabezado.

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para que se ajusten a los requisitos del proyecto.

## Requisito CORS

AEM CORS es necesario para las conexiones basadas en el explorador a las API de GraphQL AEM AEM, cuando el cliente que se conecta a la NO se proporciona desde el mismo origen (también conocido como host o dominio) que el que se proporciona a las API de.

| Tipo de cliente | [SPA Aplicación de una sola página ()](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [De servidor a servidor](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requiere la configuración CORS | ✔ | ✔ | ✘ | ✘ |

## AEM Author

AEM AEM AEM La activación de CORS en el servicio de autor de es diferente de la activación de los servicios de publicación y previsualización de. AEM AEM El servicio de autor de la requiere que se agregue una configuración OSGi a la carpeta de modo de ejecución del servicio de autor de la aplicación y no utiliza una configuración de Dispatcher.

### Configuración de OSGi

AEM El generador de configuración OSGi de CORS de define los criterios de permiso para aceptar solicitudes HTTP CORS.

| El cliente se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración OSGi de CORS | ✔ | ✘ | ✘ |


AEM El ejemplo siguiente define una configuración OSGi para el autor de la (`../config.author/..`AEM ), por lo que solo está activo en el servicio de autor de.

Las propiedades de configuración clave son:

+ `alloworigin` y/o `alloworiginregexp` AEM especifica los orígenes en los que se conecta el cliente a las ejecuciones web de la.
+ `allowedpaths` especifica los patrones de ruta de URL permitidos a partir de los orígenes especificados.
   + AEM Para admitir consultas persistentes de GraphQL, agregue el siguiente patrón: `/graphql/execute.json.*`
   + Para admitir fragmentos de experiencias, agregue el siguiente patrón: `/content/experience-fragments/.*`
+ `supportedmethods` especifica los métodos HTTP permitidos para las solicitudes CORS. AEM Para admitir consultas persistentes (y fragmentos de experiencias) de GraphQL de, agregue `GET` .
+ `supportedheaders` incluye `"Authorization"` AEM como solicitudes a Autor de la deben estar autorizadas.
+ `supportscredentials` se establece en `true` AEM como solicitud a Autor de la debe estar autorizado.

[Obtenga más información sobre la configuración OSGi de CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

AEM El siguiente ejemplo admite el uso de consultas persistentes de GraphQL AEM en el uso de la de autor. Para utilizar consultas de GraphQL definidas por el cliente, agregue una URL de extremo de GraphQL en `allowedpaths` y `POST` hasta `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

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
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### Ejemplo de configuración de OSGi

+ [Se puede encontrar un ejemplo de la configuración OSGi en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## AEM Publish

AEM AEM La activación de CORS en los servicios de publicación (y previsualización) de es diferente al servicio de creación de páginas. AEM AEM AEM El servicio de publicación requiere que se añada una configuración de Dispatcher de a la configuración de Dispatcher de publicación de la aplicación. AEM La publicación no utiliza un [Configuración de OSGi](#osgi-configuration).

AEM Al configurar CORS en la publicación de la, asegúrese de lo siguiente:

+ El `Origin` AEM El encabezado de solicitud HTTP no se puede enviar al servicio de publicación de eliminando `Origin` AEM encabezado (si se ha añadido anteriormente) del proyecto de Dispatcher de `clientheaders.any` archivo. Cualquiera `Access-Control-` Los encabezados de deben eliminarse del `clientheaders.any` AEM y Dispatcher los administra, no el servicio de publicación de la.
+ Si tiene alguna [Configuraciones de CORS OSGi](#osgi-configuration) AEM activado en el servicio Publicación de la, debe eliminarlos y migrar sus configuraciones a [Configuración de vhost de Dispatcher](#set-cors-headers-in-vhost) se describe a continuación.

### Configuración de Dispatcher

AEM El Dispatcher del servicio de publicación (y previsualización) debe configurarse para admitir CORS.

| El cliente se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración CORS de Dispatcher | ✘ | ✔ | ✔ |

#### Definición de encabezados CORS en vhost

1. AEM Abra el archivo de configuración vhost para el servicio Publicación de la, en el proyecto de configuración de Dispatcher, normalmente en `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copie el contenido del `<IfDefine ENABLE_CORS>...</IfDefine>` bloque siguiente, en el archivo de configuración vhost habilitado.

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch '%{REQUEST_SCHEME}://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. AEM Haga coincidir los orígenes deseados accediendo a su servicio de publicación de actualizando la expresión regular en la línea siguiente. Si se necesitan varios orígenes, duplique esta línea y actualice para cada origen/patrón de origen.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + Por ejemplo, para habilitar el acceso CORS desde orígenes:

      + Cualquier subdominio de `https://example.com`
      + Cualquier puerto en `http://localhost`

     Sustituya la línea por las dos líneas siguientes:

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Ejemplo de configuración de Dispatcher

+ [Se puede encontrar un ejemplo de la configuración de Dispatcher en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)

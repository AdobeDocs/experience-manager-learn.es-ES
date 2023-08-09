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
last-substantial-update: 2023-08-08T00:00:00Z
source-git-commit: f8fd13d3f315aa0bd9f268b9fe81b9d9c17b243c
workflow-type: tm+mt
source-wordcount: '647'
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

AEM La activación de CORS en el servicio de AEM Author es diferente de los servicios de AEM Publish y Vista previa de la. El servicio de creación de AEM requiere que se agregue una configuración OSGi a la carpeta de modo de ejecución del servicio de creación de AEM y no utiliza una configuración de Dispatcher.

### Configuración de OSGi

AEM El generador de configuración OSGi de CORS de define los criterios de permiso para aceptar solicitudes HTTP CORS.

| El cliente se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración OSGi de CORS | ✔ | ✘ | ✘ |


El ejemplo siguiente define una configuración OSGi para AEM Author (`../config.author/..`), por lo que solo está activo en el servicio de AEM Author.

Las propiedades de configuración clave son:

+ `alloworigin` y/o `alloworiginregexp` AEM especifica los orígenes en los que se conecta el cliente a las ejecuciones web de la.
+ `allowedpaths` especifica los patrones de ruta de URL permitidos a partir de los orígenes especificados.
   + AEM Para admitir consultas persistentes de GraphQL, agregue el siguiente patrón: `/graphql/execute.json.*`
   + Para admitir fragmentos de experiencias, agregue el siguiente patrón: `/content/experience-fragments/.*`
+ `supportedmethods` especifica los métodos HTTP permitidos para las solicitudes CORS. AEM Para admitir consultas persistentes (y fragmentos de experiencias) de GraphQL de, agregue `GET` .
+ `supportedheaders` incluye `"Authorization"` como solicitudes al Autor de AEM deben estar autorizadas.
+ `supportscredentials` se establece en `true` como solicitud al Autor de AEM debe estar autorizado.

[Obtenga más información sobre la configuración OSGi de CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

AEM El siguiente ejemplo admite el uso de consultas persistentes de GraphQL en AEM Author de la aplicación para la creación de consultas de la aplicación de datos de. Para utilizar consultas de GraphQL definidas por el cliente, agregue una URL de extremo de GraphQL en `allowedpaths` y `POST` hasta `supportedmethods`.

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

La activación de CORS en los servicios de publicación (y previsualización) de AEM es diferente al servicio de creación de AEM. AEM El servicio de publicación de AEM requiere que se añada una configuración de Dispatcher de a la configuración de Dispatcher de AEM Publish. AEM Publish no utiliza un [Configuración de OSGi](#osgi-configuration).

Al configurar CORS en AEM Publish, asegúrese de lo siguiente:

+ El `Origin` El encabezado de solicitud HTTP no se puede enviar al servicio de publicación de AEM eliminando `Origin` AEM encabezado (si se ha añadido anteriormente) del proyecto de Dispatcher de `clientheaders.any` archivo. Cualquiera `Access-Control-` Los encabezados de deben eliminarse del `clientheaders.any` y Dispatcher los administra, no el servicio de publicación de AEM.
+ Si tiene alguna [Configuraciones de CORS OSGi](#osgi-configuration) Habilitado en el servicio de publicación de AEM, debe eliminarlos y migrar sus configuraciones al [Configuración de vhost de Dispatcher](#set-cors-headers-in-vhost) se describe a continuación.

### Configuración de Dispatcher

Dispatcher del servicio de publicación (y previsualización) de AEM debe configurarse para admitir CORS.

| El cliente se conecta a | AEM Author | AEM Publish | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración CORS de Dispatcher | ✘ | ✔ | ✔ |

#### Establecer la variable de entorno de Dispatcher

1. AEM Abra el archivo global.vars para acceder a la configuración de Dispatcher de forma general, en la dirección `dispatcher/src/conf.d/variables/global.vars`.
2. Añada lo siguiente al archivo:

   ```
   # Enable CORS handling in the dispatcher
   #
   # By default, CORS is handled by the AEM publish server.
   # If you uncomment and define the ENABLE_CORS variable, then CORS will be handled in the dispatcher.
   # See the default.vhost file for a suggested dispatcher configuration. Note that:
   #   a. You will need to adapt the regex from default.vhost to match your CORS domains
   #   b. Remove the "Origin" header (if it exists) from the clientheaders.any file
   #   c. If you have any CORS domains configured in your AEM publish server origin, you have to move those to the dispatcher
   #       (i.e. accordingly update regex in default.vhost to match those domains)
   #
   Define ENABLE_CORS
   ```

#### Definición de encabezados CORS en vhost

1. Abra el archivo de configuración vhost para el servicio de publicación de AEM, en el proyecto de configuración de Dispatcher, normalmente en `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copie el contenido del `<IfDefine ENABLE_CORS>...</IfDefine>` bloque siguiente, en el archivo de configuración vhost habilitado.

   ```{ highlight="19"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
       ...
       <IfDefine ENABLE_CORS>
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
   
         # Remove Origin before sending to AEM Publish
         RequestHeader unset Origin
   
         ################## End of CORS configuration ##################
       </IfDefine>
       ...
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Haga coincidir los orígenes deseados para acceder al servicio de publicación de AEM actualizando la expresión regular en la línea siguiente. Si se necesitan varios orígenes, duplique esta línea y actualice para cada origen/patrón de origen.

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

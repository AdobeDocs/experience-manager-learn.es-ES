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
last-substantial-update: 2024-03-22T00:00:00Z
duration: 184
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 2%

---

# Uso compartido de recursos de origen cruzado (CORS)

El Intercambio de Recursos de Origen Cruzado (CORS) de Adobe Experience Manager as a Cloud Service AEM AEM facilita que las propiedades web que no son de origen realicen llamadas del lado del cliente basadas en el explorador a las API de GraphQL AEM que se utilizan para el y a otros recursos sin encabezado.

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para que se ajusten a los requisitos del proyecto.

## Requisito CORS

AEM CORS es necesario para las conexiones basadas en el explorador a las API de GraphQL AEM AEM, cuando el cliente que se conecta a la NO se proporciona desde el mismo origen (también conocido como host o dominio) que el que se proporciona a las API de.

| Tipo de cliente | SPA [Aplicación de una sola página ()](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requiere la configuración CORS | ✔ | ✔ | ✘ | ✘ |

## AEM Author

AEM AEM La activación de CORS en el servicio de autor de es diferente de la activación de los servicios de previsualización de Publish AEM y de. AEM AEM El servicio de creación de requiere que se agregue una configuración OSGi a la carpeta de modo de ejecución del servicio de creación de OSGi y no utiliza una configuración de Dispatcher.

### Configuración de OSGi

AEM El generador de configuración OSGi de CORS de define los criterios de permiso para aceptar solicitudes HTTP CORS.

| El cliente se conecta a | AEM Author | Publicación de AEM | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración OSGi de CORS | ✔ | ✘ | ✘ |


AEM AEM El ejemplo siguiente define una configuración OSGi para el autor de la (`../config.author/..`), de modo que solo está activa en el servicio de autor de la.

Las propiedades de configuración clave son:

+ AEM `alloworigin` y/o `alloworiginregexp` especifican los orígenes en los que se ejecuta la conexión del cliente a la web de la.
+ `allowedpaths` especifica los patrones de ruta de acceso de dirección URL permitidos a partir de los orígenes especificados.
   + AEM Para admitir consultas persistentes de GraphQL, agregue el siguiente patrón: `/graphql/execute.json.*`
   + Para admitir fragmentos de experiencias, agregue el siguiente patrón: `/content/experience-fragments/.*`
+ `supportedmethods` especifica los métodos HTTP permitidos para las solicitudes CORS. AEM Para admitir consultas persistentes (y fragmentos de experiencias) de GraphQL de, agregue `GET`
+ AEM `supportedheaders` incluye `"Authorization"` como solicitudes a Autor de la que deben ser autorizadas.
+ AEM `supportscredentials` se ha establecido en `true`, ya que la solicitud a Autor de la debe estar autorizada.

[Más información sobre la configuración OSGi de CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

AEM El siguiente ejemplo admite el uso de consultas persistentes de GraphQL AEM en el uso de la de autor. Para usar consultas GraphQL definidas por el cliente, agregue una dirección URL de extremo GraphQL en `allowedpaths` y `POST` a `supportedmethods`.

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

## Publicación de AEM

AEM La activación de CORS en los servicios de Publish AEM (y Vista previa) de la es diferente al servicio de creación de formularios. AEM El servicio de Publish AEM requiere que se agregue una configuración de Dispatcher AEM de a la configuración de Dispatcher de Publish. AEM Publish no usa una [configuración OSGi](#osgi-configuration).

AEM Al configurar CORS en Publish, asegúrese de lo siguiente:

+ AEM El encabezado de solicitud HTTP `Origin` no se puede enviar al servicio de Publish AEM; para ello, elimine el encabezado `Origin` (si se agregó anteriormente) del archivo `clientheaders.any` del proyecto de Dispatcher de la. Los encabezados `Access-Control-` se deben eliminar del archivo `clientheaders.any` y Dispatcher AEM los administra, no el servicio de Publish, en lugar de los que se utilizan para administrar el servicio de identidad de los usuarios.
+ AEM Si tiene alguna [configuración OSGi de CORS](#osgi-configuration) habilitada en su servicio de Publish en la red de servicio, debe eliminarla y migrar sus configuraciones a la [configuración de vhost de Dispatcher](#set-cors-headers-in-vhost) que se describe a continuación.

### Configuración de Dispatcher

AEM El Dispatcher del servicio Publish (y previsualización) debe configurarse para admitir CORS.

| El cliente se conecta a | AEM Author | Publicación de AEM | AEM Previsualización de |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración CORS de Dispatcher | ✘ | ✔ | ✔ |

#### Definición de encabezados CORS en vhost

1. AEM Abra el archivo de configuración de vhost para el servicio de Publish de, en el proyecto de configuración de Dispatcher, normalmente en `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copie el contenido del bloque `<IfDefine ENABLE_CORS>...</IfDefine>` siguiente en el archivo de configuración vhost habilitado.

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'http://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'https://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
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

3. AEM Haga coincidir los orígenes deseados para acceder a su servicio de Publish de la manera que desee actualizando la expresión regular en la línea de abajo. Si se necesitan varios orígenes, duplique esta línea y actualice para cada origen/patrón de origen.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + Por ejemplo, para habilitar el acceso CORS desde orígenes:

      + Cualquier subdominio de `https://example.com`
      + Cualquier puerto de `http://localhost`

     Sustituya la línea por las dos líneas siguientes:

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Ejemplo de configuración de Dispatcher

+ [Se puede encontrar un ejemplo de la configuración de Dispatcher en el proyecto WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)

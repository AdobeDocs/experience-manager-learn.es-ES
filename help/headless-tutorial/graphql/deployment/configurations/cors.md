---
title: Configuración de CORS para AEM GraphQL
description: Obtenga información sobre cómo configurar el uso compartido de recursos de origen cruzado (CORS) para su uso con AEM GraphQL.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2024-03-22T00:00:00Z
duration: 185
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 2%

---

# Uso compartido de recursos de origen cruzado (CORS)

El Intercambio de Recursos de Origen Cruzado (CORS) de Adobe Experience Manager as a Cloud Service facilita las propiedades web que no son de AEM para hacer llamadas del lado del cliente basadas en el explorador a las API de GraphQL de AEM y a otros recursos sin encabezado de AEM.

>[!TIP]
>
> Las siguientes configuraciones son ejemplos. Asegúrese de ajustarlos para que se ajusten a los requisitos del proyecto.

## Requisito CORS

CORS es necesario para conexiones basadas en el explorador a las API de GraphQL de AEM, cuando el cliente que se conecta a AEM NO se proporciona desde el mismo origen (también conocido como host o dominio) que AEM.

| Tipo de cliente | [Aplicación de una sola página (SPA)](../spa.md) | [Componente web/JS](../web-component.md) | [Móvil](../mobile.md) | [Servidor a servidor](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Requiere la configuración CORS | ✔ | ✔ | ✘ | ✘ |

## AEM Author

La activación del servicio CORS en AEM Author es diferente de los servicios de publicación de AEM y de vista previa de AEM. El servicio de AEM Author requiere que se agregue una configuración OSGi a la carpeta de modo de ejecución del servicio de AEM Author y no utiliza una configuración de Dispatcher.

### Configuración de OSGi

El generador de configuración de AEM CORS OSGi define los criterios de permiso para aceptar solicitudes HTTP CORS.

| El cliente se conecta a | AEM Author | AEM Publish | Previsualización de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración OSGi de CORS | ✔ | ✘ | ✘ |


El ejemplo siguiente define una configuración OSGi para AEM Author (`../config.author/..`), de modo que solo está activa en el servicio AEM Author.

Las propiedades de configuración clave son:

+ `alloworigin` o `alloworiginregexp` especifican los orígenes en los que se ejecuta el cliente que se conecta a la web de AEM.
+ `allowedpaths` especifica los patrones de ruta de acceso de dirección URL permitidos a partir de los orígenes especificados.
   + Para admitir consultas persistentes de AEM GraphQL, agregue el siguiente patrón: `/graphql/execute.json.*`
   + Para admitir fragmentos de experiencias, agregue el siguiente patrón: `/content/experience-fragments/.*`
+ `supportedmethods` especifica los métodos HTTP permitidos para las solicitudes CORS. Para admitir consultas persistentes (y fragmentos de experiencias) de AEM GraphQL, agregue `GET`
+ `supportedheaders` incluye `"Authorization"`, ya que las solicitudes al autor de AEM deben autorizarse.
+ `supportscredentials` se ha establecido en `true`, ya que la solicitud al autor de AEM debe estar autorizada.

[Más información sobre la configuración OSGi de CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

El siguiente ejemplo admite el uso de consultas persistentes de AEM GraphQL en AEM Author. Para usar consultas GraphQL definidas por el cliente, agregue una dirección URL de extremo GraphQL en `allowedpaths` y `POST` a `supportedmethods`.

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

La activación de CORS en los servicios de publicación (y previsualización) de AEM es diferente al servicio de AEM Author. El servicio Publicación de AEM requiere que se agregue una configuración de AEM Dispatcher a la configuración de Dispatcher de AEM Publish. AEM Publish no usa una [configuración OSGi](#osgi-configuration).

Al configurar CORS en AEM Publish, asegúrese de lo siguiente:

+ El encabezado de solicitud HTTP `Origin` no se puede enviar al servicio de publicación de AEM; para ello, quite el encabezado `Origin` (si se agregó anteriormente) del archivo `clientheaders.any` del proyecto de AEM Dispatcher. Cualquier encabezado `Access-Control-` debe quitarse del archivo `clientheaders.any` y Dispatcher lo administra, no el servicio de publicación de AEM.
+ Si tiene alguna [configuración OSGi de CORS](#osgi-configuration) habilitada en su servicio de publicación de AEM, debe eliminarla y migrar dicha configuración a la [configuración vhost de Dispatcher](#set-cors-headers-in-vhost) que se describe a continuación.

### Configuración de Dispatcher

El Dispatcher del servicio de publicación (y previsualización) de AEM debe configurarse para admitir CORS.

| El cliente se conecta a | AEM Author | AEM Publish | Previsualización de AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Requiere la configuración CORS de Dispatcher | ✘ | ✔ | ✔ |

#### Definición de encabezados CORS en vhost

1. Abra el archivo de configuración vhost para el servicio Publicación de AEM, en el proyecto de configuración de Dispatcher, normalmente en `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
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

3. Haga coincidir los orígenes deseados para acceder al servicio de publicación de AEM actualizando la expresión regular en la línea siguiente. Si se necesitan varios orígenes, duplique esta línea y actualice para cada origen/patrón de origen.

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

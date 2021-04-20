---
title: Credenciales de servicio
description: Las credenciales de servicio de AEM se utilizan para facilitar las aplicaciones, sistemas y servicios externos con el fin de interactuar mediante programación con los servicios de AEM Author o Publish a través de HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# Credenciales de servicio

Las integraciones con AEM as a Cloud Service deben poder autenticarse de forma segura en AEM. La consola de desarrollador de AEM concede acceso a las credenciales de servicio, que se utilizan para facilitar las aplicaciones, sistemas y servicios externos con el fin de interactuar mediante programación con los servicios de AEM Author o Publish a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Las credenciales de servicio pueden aparecer de forma similar [Local Development Access Tokens](./local-development-access-token.md), pero son diferentes de varias formas clave:

+ Las credenciales de servicio son _no_ tokens de acceso, sino que son credenciales que se utilizan para _obtener_ tokens de acceso.
+ Las credenciales de servicio son más permanentes (caducan cada 365 días) y no cambian a menos que se revoquen, mientras que los tokens de acceso de desarrollo local caducan a diario.
+ Las credenciales de servicio para un entorno de AEM as a Cloud Service se asignan a un único usuario de cuenta técnica de AEM, mientras que los tokens de acceso de desarrollo local se autentican como el usuario de AEM que generó el token de acceso.

Tanto las credenciales de servicio como los tokens de acceso que generan, así como los tokens de acceso de desarrollo local, deben mantenerse en secreto, ya que los tres pueden utilizarse para obtener acceso a sus entornos respectivos de AEM as a Cloud Service

## Generar credenciales de servicio

La generación de credenciales de servicio se divide en dos pasos:

1. Inicialización de credenciales de servicio única por un administrador de organización de Adobe IMS
1. Descarga y uso del JSON de credenciales de servicio

### Inicialización de Credenciales de Servicio

Las credenciales de servicio, a diferencia de los tokens de acceso de desarrollo local, requieren una _inicialización única_ por parte del administrador de IMS de Adobe Org antes de poder descargarlas.

![Inicializar credenciales de servicio](assets/service-credentials/initialize-service-credentials.png)

__Esta es una inicialización única por entorno de AEM as a Cloud Service__

1. Asegúrese de haber iniciado sesión como administrador de su organización de Adobe IMS
1. Inicie sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra el Programa que contiene el entorno de AEM as a Cloud Service para integrar y configurar las credenciales de servicio para
1. Pulse los puntos suspensivos junto al entorno en la sección __Environments__ y seleccione __Developer Console__
1. Pulse en la pestaña __Integrations__
1. Pulse el botón __Get Service Credentials__
1. Las credenciales del servicio se inicializarán y se mostrarán como JSON

![AEM Developer Console: integraciones: obtener credenciales de servicio](./assets/service-credentials/developer-console.png)

Una vez inicializadas las credenciales de servicio del entorno de AEM as Cloud Service, otros desarrolladores de AEM de su organización de Adobe IMS pueden descargarlas.

### Descargar credenciales del servicio

![Descargar credenciales del servicio](assets/service-credentials/download-service-credentials.png)

La descarga de las credenciales de servicio sigue los mismos pasos que la inicialización. Si la inicialización aún no se ha producido, se mostrará al usuario un error al pulsar el botón __Get Service Credentials__.

1. Asegúrese de que es miembro del perfil de producto de IMS de __Cloud Manager - Developer__ (que otorga acceso a AEM Developer Console)
   + Los entornos de Sandbox AEM as a Cloud Service solo requieren la pertenencia al __Perfil de producto de los__ administradores de AEM o __usuarios de AEM__
1. Inicie sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra el programa que contiene el entorno de AEM as a Cloud Service con el que se integrará
1. Pulse los puntos suspensivos junto al entorno en la sección __Environments__ y seleccione __Developer Console__
1. Pulse en la pestaña __Integrations__
1. Pulse el botón __Get Service Credentials__
1. Pulse el botón de descarga en la esquina superior izquierda para descargar el archivo JSON que contiene el valor Credenciales de servicio y guarde el archivo en una ubicación segura.
   + _Si las credenciales del servicio están comprometidas, póngase en contacto inmediatamente con el servicio de asistencia de Adobe para que se les revoque_

## Instalación de las credenciales del servicio

Las credenciales de servicio proporcionan los detalles necesarios para generar un JWT, que se intercambia por un token de acceso utilizado para autenticarse con AEM as a Cloud Service. Las credenciales de servicio deben almacenarse en una ubicación segura a la que puedan acceder las aplicaciones, sistemas o servicios externos que la utilicen para acceder a AEM. Cómo y dónde se administran las credenciales de servicio serán únicas para cada cliente.

Para simplificar, este tutorial pasa las credenciales de servicio a través de la línea de comandos, sin embargo, trabaje con su equipo de seguridad de TI para comprender cómo almacenar y acceder a estas credenciales de acuerdo con las directrices de seguridad de su organización.

1. Copie las [credenciales de servicio descargadas JSON](#download-service-credentials) en un archivo denominado `service_token.json` en la raíz del proyecto
   + Pero recuerden, ¡nunca comprometan ninguna credencial a Git!

## Usar credenciales de servicio

Las credenciales de servicio, un objeto JSON completamente formado, no son lo mismo que el JWT ni el token de acceso. En su lugar, las credenciales de servicio (que contienen una clave privada) se utilizan para generar un JWT, que se intercambia con las API de Adobe IMS por un token de acceso.

![Credenciales de Servicio: Aplicación Externa](assets/service-credentials/service-credentials-external-application.png)

1. Descargue las credenciales de servicio de AEM Developer Console en una ubicación segura
1. Una aplicación externa debe interactuar mediante programación con los entornos de AEM as a Cloud Service
1. La aplicación externa lee las credenciales del servicio desde una ubicación segura
1. La Aplicación Externa utiliza la información de las Credenciales de Servicio para construir un Token de JWT
1. El token de JWT se envía a Adobe IMS para intercambiar por un token de acceso
1. Adobe IMS devuelve un token de acceso que se puede utilizar para acceder a AEM as a Cloud Service
   + Los tokens de acceso pueden tener una caducidad solicitada. Es mejor mantener la vida corta del token de acceso y actualizarlo cuando sea necesario.
1. La aplicación externa realiza solicitudes HTTP a AEM as a Cloud Service, agregando el token de acceso como token de portador al encabezado de autorización de las solicitudes HTTP
1. AEM as a Cloud Service recibe la solicitud HTTP, la autentica y realiza el trabajo solicitado por la solicitud HTTP y devuelve una respuesta HTTP a la aplicación externa

### Actualizaciones en la aplicación externa

Para acceder a AEM as a Cloud Service mediante las credenciales de servicio, nuestra aplicación externa debe actualizarse de 3 formas:

1. Leer en las credenciales del servicio
   + Para simplificar, lo leeremos del archivo JSON descargado, pero en situaciones de uso real, las credenciales de servicio deben almacenarse de forma segura de acuerdo con las directrices de seguridad de su organización
1. Generar un JWT a partir de las credenciales del servicio
1. Intercambiar el JWT por un token de acceso
   + Cuando existen credenciales de servicio, nuestra aplicación externa utiliza este token de acceso en lugar del token de acceso de desarrollo local al acceder a AEM as a Cloud Service

En este tutorial, el módulo npm `@adobe/jwt-auth` de Adobe se utiliza para ambos, (1) genera el JWT a partir de las credenciales del servicio y (2) lo cambia por un token de acceso, en una sola llamada a la función . Si la aplicación no está basada en JavaScript, revise el [código de muestra en otros idiomas](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) para saber cómo crear un JWT a partir de las credenciales del servicio y cámbielo por un token de acceso con Adobe IMS.

## Leer las credenciales del servicio

Revise el `getCommandLineParams()` y vea que podemos leer en los archivos JSON de credenciales de servicio utilizando el mismo código utilizado para leer en el JSON de token de acceso de desarrollo local.

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## Crear un JWT e intercambiar un token de acceso

Una vez leídas las credenciales de servicio, se utilizan para generar un JWT que luego se intercambia con las API de Adobe IMS por un token de acceso, que luego se puede utilizar para acceder a AEM as a Cloud Service.

Esta aplicación de ejemplo está basada en Node.js, por lo que es mejor utilizar el módulo npm [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) para facilitar la (1) generación de JWT y (20 exchange con Adobe IMS). Si la aplicación se desarrolla con otro idioma, revise [los ejemplos de código correspondientes](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) sobre cómo construir la solicitud HTTP a Adobe IMS con otros lenguajes de programación.

1. Actualice `getAccessToken(..)` para inspeccionar el contenido del archivo JSON y determinar si representa un token de acceso de desarrollo local o credenciales de servicio. Esto se puede lograr fácilmente comprobando la existencia de la propiedad `.accessToken` , que solo existe para el token de acceso de desarrollo local JSON.

   Si se proporcionan credenciales de servicio, la aplicación genera un JWT y lo intercambia con Adobe IMS para obtener un token de acceso. Utilizaremos la función [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) de `auth(...)` que genera un JWT y lo intercambia por un token de acceso en una sola llamada a la función.  Los parámetros de `auth(..)` son un objeto [JSON compuesto por información específica](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponible en el JSON de credenciales de servicio, como se describe a continuación en el código.

   ```javascript
    async function getAccessToken(developerConsoleCredentials) {
   
        if (developerConsoleCredentials.accessToken) {
            // This is a Local Development access token
            return developerConsoleCredentials.accessToken;
        } else {
            // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
            let serviceCredentials = developerConsoleCredentials.integration;
   
            // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
            // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
            let { access_token } = await auth({
                clientId: serviceCredentials.technicalAccount.clientId, // Client Id
                technicalAccountId: serviceCredentials.id,              // Technical Account Id
                orgId: serviceCredentials.org,                          // Adobe IMS Org Id
                clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
                privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
                metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
                ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
            });
   
            return access_token;
        }
    }
   ```

   Ahora, según el archivo JSON (el token de acceso de desarrollo local JSON o el JSON de credenciales de servicio) que se pase a través del parámetro de línea de comandos `file`, la aplicación obtendrá un token de acceso.

   Recuerde que aunque las credenciales de servicio no caducan, el JWT y el token de acceso correspondiente sí lo hacen y deben actualizarse antes de que caduquen. Esto se puede hacer utilizando un `refresh_token` [proporcionado por Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Con estos cambios en su lugar y las credenciales de servicio JSON descargadas de AEM Developer Console (y para mayor simplicidad, guardadas como `service_token.json` la misma carpeta que esta `index.js`), ejecute la aplicación reemplazando el parámetro de línea de comandos `file` por `service_token.json` y actualice la `propertyValue` a un nuevo valor para que los efectos se vean en AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   La salida al terminal tendrá el siguiente aspecto:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Las líneas __403 - Prohibido__ indican errores en las llamadas de API HTTP a AEM as a Cloud Service. Estos 403 errores prohibidos se producen al intentar actualizar los metadatos de los recursos.

   El motivo de esto es que el token de acceso derivado de Credenciales de servicio autentica la solicitud a AEM mediante una cuenta técnica creada automáticamente para el usuario de AEM que, de forma predeterminada, solo tiene acceso de lectura. Para proporcionar a la aplicación acceso de escritura a AEM, se debe conceder permiso en AEM a la cuenta técnica de usuario de AEM asociada al token de acceso.

## Configuración del acceso en AEM

El token de acceso derivado de Credenciales de servicio utiliza una cuenta técnica de usuario de AEM que pertenece al grupo de usuarios de AEM Colaboradores.

![Credenciales de servicio: cuenta técnica: usuario de AEM](./assets/service-credentials/technical-account-user.png)

Una vez que la cuenta técnica del usuario de AEM existe en AEM (después de la primera solicitud HTTP con el token de acceso), los permisos de este usuario de AEM se pueden administrar del mismo modo que los de otros usuarios de AEM.

1. En primer lugar, busque el nombre de inicio de sesión de AEM de la cuenta técnica abriendo el JSON de credenciales de servicio descargado de AEM Developer Console y busque el valor `integration.email`, que debería ser similar a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Inicie sesión en el servicio Autor del entorno de AEM correspondiente como administrador de AEM
1. Vaya a __Herramientas__ > __Seguridad__ > __Usuarios__
1. Busque el usuario de AEM con el __Nombre de inicio de sesión__ identificado en el paso 1 y abra sus __Propiedades__
1. Vaya a la pestaña __Groups__ y agregue el grupo __DAM Users__ (que como acceso de escritura a los recursos)
1. Toque __Guardar y cerrar__

Con la cuenta técnica autorizada en AEM para tener permisos de escritura en los recursos, vuelva a ejecutar la aplicación:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

La salida al terminal tendrá el siguiente aspecto:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verificar los cambios

1. Inicie sesión en el entorno de AEM as a Cloud Service que se ha actualizado (con el mismo nombre de host proporcionado en el parámetro de línea de comandos `aem`)
1. Vaya a __Assets__ > __Files__
1. Desplácese por la carpeta de recursos especificada por el parámetro de línea de comandos `folder`, por ejemplo __WKND__ > __English__ > __Adventures__ > __Ayunar el vino de Napa__
1. Abra las __Propiedades__ de cualquier recurso de la carpeta
1. Vaya a la pestaña __Advanced__
1. Revise el valor de la propiedad actualizada, por ejemplo __Copyright__ que está asignado a la propiedad `metadata/dc:rights` JCR actualizada, que ahora refleja el valor proporcionado en el parámetro `propertyValue`, por ejemplo __WKND Restricted Use__

![Actualización de metadatos de uso restringido de WKND](./assets/service-credentials/asset-metadata.png)

## Felicitaciones!

Ahora que hemos accedido mediante programación a AEM as a Cloud Service utilizando un token de acceso de desarrollo local, así como un token de acceso de servicio a servicio listo para la producción.


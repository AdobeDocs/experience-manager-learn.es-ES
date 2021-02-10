---
title: Credenciales de servicio
description: AEM Credenciales de servicio se utilizan para facilitar la interacción de aplicaciones, sistemas y servicios externos con los servicios de AEM Author o Publish a través de HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
translation-type: tm+mt
source-git-commit: 0b1150cd7ca32382cfaa880f9f956b55bfb65a33
workflow-type: tm+mt
source-wordcount: '1824'
ht-degree: 0%

---


# Credenciales de servicio

Las integraciones con AEM como Cloud Service deben poder autenticarse de forma segura en AEM. AEM Developer Console permite acceder a las credenciales de servicio, que se utilizan para facilitar la interacción programática de aplicaciones, sistemas y servicios externos con los servicios de AEM Author o Publish a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Las Credenciales de Servicio pueden aparecer como [Tokenes de acceso de desarrollo local](./local-development-access-token.md) similares, pero son diferentes en algunos aspectos clave:

+ Las credenciales de servicio son _no_ tokenes de acceso, sino más bien credenciales que se utilizan para _obtener_ tokenes de acceso.
+ Las credenciales de servicio son más permanentes (caducan cada 365 días) y no cambian a menos que se revoquen, mientras que los Tokenes de acceso de desarrollo local caducan diariamente.
+ Las credenciales de servicio de un AEM como entorno de Cloud Service se asignan a un único usuario AEM de cuenta técnica, mientras que los Tokenes de acceso de desarrollo local se autentican como el AEM usuario que generó el token de acceso.

Tanto las credenciales de servicio como los tokenes de acceso que generan, así como los Tokenes de acceso de desarrollo local, deben mantenerse en secreto, ya que los tres pueden utilizarse para obtener acceso a sus respectivos AEM como entornos Cloud Service

## Generar credenciales de servicio

La generación de credenciales de servicio se divide en dos pasos:

1. Inicialización de Credenciales de servicio única por parte de un administrador de organización de IMS de Adobe
1. La descarga y el uso del JSON de credenciales de servicio

### Inicialización de credenciales de servicio

Las credenciales de servicio, a diferencia de los Tokenes de acceso de desarrollo local, requieren una _inicialización única_ por parte del administrador de IMS de la organización de Adobe para poder descargarlas.

![Inicializar credenciales de servicio](assets/service-credentials/initialize-service-credentials.png)

__Se trata de una inicialización única por AEM como entorno de Cloud Service__

1. Asegúrese de haber iniciado sesión como administrador de la organización de Adobe IMS
1. Inicie sesión en [Administrador de Adobe Cloud](https://my.cloudmanager.adobe.com)
1. Abra el Programa que contiene el AEM como entorno de Cloud Service para integrar la configuración de las credenciales de servicio para
1. Toque las elipsis junto al entorno en la sección __Entornos__ y seleccione __Consola de programador__
1. Puntee en la ficha __Integraciones__
1. Toque el botón __Obtener credenciales de servicio__
1. Las credenciales de servicio se inicializarán y se mostrarán como JSON

![AEM Developer Console - Integraciones - Obtener credenciales de servicio](./assets/service-credentials/developer-console.png)

Una vez inicializadas las credenciales de servicio del AEM como entorno Cloud Service, otros desarrolladores AEM de su organización IMS Adobe pueden descargarlas.

### Descargar credenciales del servicio

![Descargar credenciales del servicio](assets/service-credentials/download-service-credentials.png)

La descarga de las credenciales de servicio sigue los mismos pasos que la inicialización. Si la inicialización aún no se ha realizado, se mostrará al usuario un error al tocar el botón __Obtener credenciales de servicio__.

1. Asegúrese de que es miembro del __Perfil del producto de Cloud Manager - Developer__ IMS (que otorga acceso a AEM consola de desarrollador)
   + Los AEM de Simulador para pruebas como entornos de Cloud Service solo requieren la membresía en el Perfil de producto __AEM Administradores__ o __Usuarios de AEM__
1. Inicie sesión en [Administrador de Adobe Cloud](https://my.cloudmanager.adobe.com)
1. Abra el Programa que contiene el AEM como entorno de Cloud Service con el que se integrará
1. Toque las elipsis junto al entorno en la sección __Entornos__ y seleccione __Consola de programador__
1. Puntee en la ficha __Integraciones__
1. Toque el botón __Obtener credenciales de servicio__
1. Toque el botón de descarga en la esquina superior izquierda para descargar el archivo JSON que contiene el valor de Credenciales de servicio y guarde el archivo en una ubicación segura.
   + _Si las credenciales de servicio están comprometidas, póngase en contacto inmediatamente con el servicio de soporte de Adobe para que se les revoque_

## Instalar las credenciales de servicio

Las credenciales de servicio proporcionan los detalles necesarios para generar un JWT, que se intercambia por un token de acceso utilizado para autenticarse con AEM como Cloud Service. Las credenciales de servicio deben almacenarse en una ubicación segura a la que puedan acceder las aplicaciones, los sistemas o los servicios externos que la utilizan para acceder a AEM. El modo y el lugar en que se administran las credenciales de servicio serán únicos por cliente.

Para simplificar, este tutorial pasa las credenciales de servicio a través de la línea de comandos. Sin embargo, trabaje con su equipo de seguridad de TI para comprender cómo almacenar y acceder a estas credenciales de acuerdo con las directrices de seguridad de su organización.

1. Copie el [descargó las credenciales de servicio JSON](#download-service-credentials) en un archivo denominado `service_token.json` en la raíz del proyecto
   + Pero recuerden, ¡nunca envíen credenciales a Git!

## Usar credenciales de servicio

Las credenciales de servicio, un objeto JSON completamente formado, no son iguales a JWT ni al token de acceso. En su lugar, las credenciales de servicio (que contienen una clave privada) se utilizan para generar un JWT, que se intercambia con las API de IMS de Adobe para un token de acceso.

![Credenciales de servicio - Aplicación externa](assets/service-credentials/service-credentials-external-application.png)

1. Descargue las credenciales de servicio de AEM consola de desarrollador en una ubicación segura
1. Una aplicación externa debe interactuar mediante programación con AEM como entornos de Cloud Service
1. La aplicación externa lee en las credenciales de servicio desde una ubicación segura
1. La aplicación externa utiliza información de las credenciales de servicio para construir un token de JWT
1. El testigo JWT se envía a Adobe IMS para intercambiar por un token de acceso
1. Adobe IMS devuelve un token de acceso que se puede utilizar para acceder a AEM como Cloud Service
   + Los tokenes de acceso pueden solicitar una caducidad. Lo mejor es mantener la vida corta del token de acceso y actualizarla cuando sea necesario.
1. La aplicación externa realiza solicitudes HTTP para AEM como Cloud Service, agregando el token de acceso como token de portador al encabezado de autorización de las solicitudes HTTP
1. AEM como Cloud Service recibe la solicitud HTTP, la autentica y realiza el trabajo solicitado por la solicitud HTTP y devuelve una respuesta HTTP a la aplicación externa

### Actualizaciones de la aplicación externa

Para acceder a AEM como Cloud Service mediante las credenciales de servicio, nuestra aplicación externa debe actualizarse de tres maneras:

1. Leer en las credenciales de servicio
   + Para simplificar, leeremos estos datos del archivo JSON descargado, pero en situaciones de uso real, las credenciales de servicio deben almacenarse de forma segura de acuerdo con las directrices de seguridad de su organización
1. Generar un JWT a partir de las credenciales de servicio
1. Intercambiar JWT por un token de acceso
   + Cuando las credenciales de servicio están presentes, nuestra aplicación externa utiliza este token de acceso en lugar del Token de acceso de desarrollo local, al acceder a AEM como Cloud Service

En este tutorial, el módulo npm `@adobe/jwt-auth` de Adobe se utiliza para ambos, (1) genera el JWT a partir de las credenciales de servicio y (2) lo cambia por un token de acceso, en una sola llamada de función. Si su aplicación no está basada en JavaScript, revise el [código de muestra en otros idiomas](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) para ver cómo crear un JWT a partir de las credenciales de servicio y canjéalo por un token de acceso con IMS de Adobe.

## Leer las credenciales del servicio

Revise `getCommandLineParams()` y observe que podemos leer en los archivos JSON de Credenciales de Servicio utilizando el mismo código utilizado para leer en el JSON de Token de acceso de Desarrollo Local.

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

## Crear un JWT e intercambiar un Token de acceso

Una vez leídas las credenciales de servicio, se utilizan para generar un JWT que luego se intercambia con las API de IMS de Adobe para un token de acceso, que luego se puede utilizar para acceder a AEM como Cloud Service.

Esta aplicación de ejemplo está basada en Node.js, por lo que es mejor utilizar el módulo npm [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) para facilitar la (1) generación JWT y (20 Exchange con IMS Adobe. Si su aplicación se desarrolla con otro idioma, consulte [las muestras de código correspondientes](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) sobre cómo construir la solicitud HTTP para Adobe IMS usando otros lenguajes de programación.

1. Actualice `getAccessToken(..)` para inspeccionar el contenido del archivo JSON y determinar si representa un Token de acceso de desarrollo local o credenciales de servicio. Esto se puede lograr fácilmente comprobando la existencia de la propiedad `.accessToken`, que sólo existe para el Token de acceso de Desarrollo Local JSON.

   Si se proporcionan credenciales de servicio, la aplicación genera un JWT y lo intercambia con IMS de Adobe para un token de acceso. Utilizaremos la función [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) `auth(...)` que genera un JWT y lo intercambia por un token de acceso en una sola llamada de función.  Los parámetros de `auth(..)` es un objeto [JSON compuesto de información específica](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponible desde el JSON de credenciales de servicio, como se describe a continuación en el código.

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

   Ahora, en función del archivo JSON (el JSON de Token de acceso de desarrollo local o el JSON de credenciales de servicio) que se pase a través del parámetro de línea de comandos `file`, la aplicación obtendrá un token de acceso.

   Recuerde que mientras las credenciales de servicio no caducan, JWT y el token de acceso correspondiente lo hacen y deben actualizarse antes de que caduquen. Esto se puede hacer utilizando un `refresh_token` [proporcionado por Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Con estos cambios en su lugar, y con las credenciales de servicio JSON descargadas de la consola de desarrollo de AEM (y para mayor simplicidad, guardadas como `service_token.json` la misma carpeta que ésta `index.js`), ejecute la aplicación reemplazando el parámetro de línea de comandos `file` por `service_token.json` y actualice el `propertyValue` a un nuevo valor para que los efectos se vean en AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   La salida a la terminal tendrá el siguiente aspecto:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Las líneas __403 - Prohibido__ indican errores en las llamadas de API HTTP a AEM como Cloud Service. Estos 403 errores prohibidos se producen al intentar actualizar los metadatos de los recursos.

   El motivo de esto es que el token de acceso derivado de Credenciales de Servicio autentica la solicitud de AEM mediante una cuenta técnica creada automáticamente AEM usuario que, de forma predeterminada, solo tiene acceso de lectura. Para proporcionar a la aplicación acceso de escritura a AEM, se debe conceder permiso en AEM a la cuenta técnica AEM usuario asociada con el token de acceso.

## Configurar el acceso en AEM

El token de acceso derivado de Credenciales de Servicio utiliza una cuenta técnica AEM Usuario que está afiliado al grupo de usuarios Colaboradores AEM.

![Credenciales de servicio: cuenta técnica AEM usuario](./assets/service-credentials/technical-account-user.png)

Una vez que la cuenta técnica AEM usuario existe en AEM (después de la primera solicitud HTTP con el token de acceso), los permisos de este AEM usuario se pueden administrar del mismo modo que otros usuarios AEM.

1. En primer lugar, localice el nombre de inicio de sesión AEM de la cuenta técnica abriendo el JSON de credenciales de servicio descargado desde AEM consola de desarrollador y localice el valor `integration.email`, que debería ser similar a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Inicie sesión en el servicio Autor del entorno de AEM correspondiente como administrador AEM
1. Vaya a __Herramientas__ > __Seguridad__ > __Usuarios__
1. Busque el usuario AEM con el __Nombre de inicio de sesión__ identificado en el paso 1 y abra sus __Propiedades__
1. Vaya a la ficha __Grupos__ y agregue el grupo __Usuarios de DAM__ (que como acceso de escritura a los recursos)
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

La salida a la terminal tendrá el siguiente aspecto:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verificar los cambios

1. Inicie sesión en el AEM como entorno de Cloud Service que se ha actualizado (utilizando el mismo nombre de host proporcionado en el parámetro de línea de comandos `aem`)
1. Vaya a __Assets__ > __Files__
1. Desplácese hasta la carpeta de recursos especificada por el parámetro de línea de comandos `folder`, por ejemplo __WKND__ > __Inglés__ > __Aventuras__ > __Dasado de vino de Napa__
1. Abra las __Propiedades__ de cualquier recurso de la carpeta
1. Vaya a la ficha __Avanzado__
1. Revise el valor de la propiedad actualizada, por ejemplo __Copyright__, que se asigna a la propiedad `metadata/dc:rights` JCR actualizada, que ahora refleja el valor proporcionado en el parámetro `propertyValue`, por ejemplo __Uso restringido de WKND__

![Actualización de metadatos de uso restringido WKND](./assets/service-credentials/asset-metadata.png)

## Felicitaciones!

Ahora que hemos accedido programáticamente a AEM como Cloud Service usando un token de acceso de desarrollo local, así como un token de acceso de servicio a servicio listo para la producción.


---
title: Credenciales del servicio
description: Aprenda a utilizar las credenciales de servicio que se utilizan para facilitar las aplicaciones, sistemas y servicios externos con el fin de interactuar mediante programación con los servicios de Autor o Publicación a través de HTTP.
version: Cloud Service
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
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1937'
ht-degree: 0%

---

# Credenciales del servicio

Las integraciones con Adobe Experience Manager (AEM) as a Cloud Service deben poder autenticarse de forma segura en AEM servicio. AEM Developer Console otorga acceso a las credenciales de servicio, que se utilizan para facilitar la interacción mediante programación de aplicaciones, sistemas y servicios externos con AEM Author o Publish Services a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Las credenciales del servicio pueden aparecer de forma similar [Tokens de acceso de desarrollo local](./local-development-access-token.md) pero son diferentes de algunas maneras clave:

+ Las credenciales de servicio están asociadas con cuentas técnicas. Se pueden activar varias credenciales de servicio para una cuenta técnica.
+ Las credenciales del servicio son _not_ acceder a tokens, no son credenciales que se usan para _obtener_ acceder a tokens.
+ Las credenciales de servicio son más permanentes (el certificado caduca cada 365 días) y no cambian a menos que se revoquen, mientras que los tokens de acceso de desarrollo local caducan a diario.
+ Credenciales de servicio para un entorno as a Cloud Service AEM se asignan a un único usuario de cuenta técnica AEM, mientras que los tokens de acceso de desarrollo local se autentican como el usuario AEM que generó el token de acceso.
+ Un entorno as a Cloud Service AEM puede tener hasta diez cuentas técnicas, cada una con sus propias credenciales de servicio, cada una con una asignación a una cuenta técnica discreta AEM usuario.

Tanto las credenciales de servicio como los tokens de acceso que generan y los tokens de acceso de desarrollo local deben mantenerse en secreto. Como los tres pueden ser utilizados para obtener, acceso a su entorno as a Cloud Service AEM respectivo.

## Generar credenciales de servicio

La generación de credenciales de servicio se divide en dos pasos:

1. Creación de una cuenta técnica única por parte de un administrador de organización de Adobe IMS
1. La descarga y el uso de las credenciales de servicio de la cuenta técnica JSON

### Crear una cuenta técnica

Las credenciales de servicio, a diferencia de los tokens de acceso de desarrollo local, requieren que un administrador de IMS de organización de Adobe cree una cuenta técnica antes de poder descargarlas. Se deben crear cuentas técnicas discretas para cada cliente que requiera acceso programático a AEM.

![Crear una cuenta técnica](assets/service-credentials/initialize-service-credentials.png)

Las cuentas técnicas se crean una vez, pero las claves privadas utilizan para administrar las credenciales de servicio asociadas con la cuenta técnica y se pueden administrar a lo largo del tiempo. Por ejemplo, se deben generar nuevas credenciales de clave privada/servicio antes de la caducidad de la clave privada actual para permitir el acceso ininterrumpido de un usuario de las credenciales de servicio.

1. Asegúrese de haber iniciado sesión como:
   + __Administrador de la organización IMS de Adobe__
   + Miembro de __Administradores de AEM__ Perfil de producto IMS en __Autor de AEM__
1. Iniciar sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra el Programa que contiene el entorno as a Cloud Service AEM para integrar y configurar las credenciales de servicio para
1. Pulse los puntos suspensivos junto al entorno en la __Entornos__ y seleccione __Developer Console__
1. Toque en la __Integraciones__ ficha
1. Toque . __Cuentas técnicas__ ficha
1. Toque __Crear nueva cuenta técnica__ botón
1. Las credenciales de servicio de la cuenta técnica se inicializan y se muestran como JSON

![AEM Developer Console - Integraciones - Obtención de credenciales de servicio](./assets/service-credentials/developer-console.png)

Una vez inicializadas las credenciales de servicio del AEM como entorno de Cloud Service, otros desarrolladores de AEM de su organización IMS de Adobe pueden descargarlas.

### Descargar credenciales del servicio

![Descargar credenciales del servicio](assets/service-credentials/download-service-credentials.png)

La descarga de las credenciales de servicio sigue los pasos similares a los de la inicialización.

1. Asegúrese de haber iniciado sesión como:
   + __Administrador de la organización IMS de Adobe__
   + Miembro de __Administradores de AEM__ Perfil de producto IMS en __Autor de AEM__
1. Iniciar sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Abra el Programa que contiene el entorno as a Cloud Service AEM para integrar con
1. Pulse los puntos suspensivos junto al entorno en la __Entornos__ y seleccione __Developer Console__
1. Toque en la __Integraciones__ ficha
1. Toque . __Cuentas técnicas__ ficha
1. Expanda el __Cuenta técnica__ para usar
1. Expanda el __Clave privada__ cuyas credenciales de servicio se descargarán y verifique que el estado sea __Activo__
1. Toque en la __...__ > __Ver__ asociado con la variable __Clave privada__, que muestra el JSON de credenciales de servicio
1. Pulse el botón de descarga en la esquina superior izquierda para descargar el archivo JSON que contiene el valor Credenciales de servicio y guarde el archivo en una ubicación segura

## Instalación de las credenciales del servicio

Las credenciales del servicio proporcionan los detalles necesarios para generar un JWT, que se intercambia por un token de acceso utilizado para autenticarse con AEM as a Cloud Service. Las credenciales de servicio deben almacenarse en una ubicación segura accesible a través de las aplicaciones, sistemas o servicios externos que la utilizan para acceder a AEM. Cómo y dónde se administran las credenciales de servicio son únicas por cliente.

Para simplificar, este tutorial pasa las credenciales de servicio a través de la línea de comandos. Sin embargo, trabaje con su equipo de seguridad de TI para comprender cómo almacenar y acceder a estas credenciales de acuerdo con las directrices de seguridad de su organización.

1. Copie el [se ha descargado el JSON de credenciales de servicio](#download-service-credentials) a un archivo denominado `service_token.json` en la raíz del proyecto
   + Recuerde, nunca comprometa _cualquier credencial_ ¡a Git!

## Usar credenciales de servicio

Las credenciales de servicio, un objeto JSON completamente formado, no son lo mismo que el JWT ni el token de acceso. En su lugar, las credenciales de servicio (que contienen una clave privada) se utilizan para generar un JWT, que se intercambia con las API de Adobe IMS por un token de acceso.

![Credenciales de Servicio: Aplicación Externa](assets/service-credentials/service-credentials-external-application.png)

1. Descargue las credenciales de servicio de AEM Developer Console en una ubicación segura
1. La aplicación externa necesita interactuar mediante programación con AEM entorno as a Cloud Service
1. La aplicación externa lee las credenciales del servicio desde una ubicación segura
1. La Aplicación Externa utiliza la información de las Credenciales de Servicio para construir un Token de JWT
1. El token de JWT se envía a Adobe IMS para intercambiar por un token de acceso
1. Adobe IMS devuelve un token de acceso que se puede usar para acceder a AEM as a Cloud Service
   + Los tokens de acceso pueden tener una caducidad solicitada. Es mejor mantener la vida corta del token de acceso y actualizarlo cuando sea necesario.
1. La aplicación externa hace que las solicitudes HTTP AEM as a Cloud Service, agregando el token de acceso como token de portador al encabezado de autorización de las solicitudes HTTP
1. AEM as a Cloud Service recibe la solicitud HTTP, la autentica y realiza el trabajo solicitado por la solicitud HTTP y devuelve una respuesta HTTP a la aplicación externa

### Actualizaciones en la aplicación externa

Para acceder a AEM as a Cloud Service mediante las credenciales de servicio, la aplicación externa debe actualizarse de tres maneras:

1. Leer en las credenciales del servicio

+ Para simplificar, las credenciales de servicio se leen desde el archivo JSON descargado, pero en situaciones de uso real, las credenciales de servicio deben almacenarse de forma segura de acuerdo con las directrices de seguridad de su organización

1. Generar un JWT a partir de las credenciales del servicio
1. Intercambiar el JWT por un token de acceso

+ Cuando existen credenciales de servicio, la aplicación externa utiliza este token de acceso en lugar del token de acceso de desarrollo local al acceder a AEM as a Cloud Service

En este tutorial, el Adobe `@adobe/jwt-auth` El módulo npm se utiliza para ambos, (1) genera el JWT a partir de las credenciales del servicio y (2) lo cambia por un token de acceso, en una sola llamada a la función. Si la aplicación no está basada en JavaScript, revise la [código de muestra en otros idiomas](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) para obtener información sobre cómo crear un JWT a partir de las credenciales del servicio y cómo intercambiarlo por un token de acceso con Adobe IMS.

## Leer las credenciales del servicio

Consulte la `getCommandLineParams()` así que vea cómo se lee el archivo JSON de credenciales de servicio utilizando el mismo código utilizado para leer en el JSON de token de acceso de desarrollo local.

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

Una vez leídas las credenciales de servicio, se utilizan para generar un JWT que luego se intercambia con las API de Adobe IMS por un token de acceso. Este token de acceso se puede utilizar para acceder a AEM as a Cloud Service.

Esta aplicación de ejemplo está basada en Node.js, por lo que es mejor utilizar [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) módulo npm para facilitar la (1) generación de JWT y (20 intercambio con Adobe IMS. Si su aplicación se desarrolla con otro idioma, consulte [las muestras de código correspondientes](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) sobre cómo construir la solicitud HTTP a Adobe IMS utilizando otros lenguajes de programación.

1. Actualice el `getAccessToken(..)` para inspeccionar el contenido del archivo JSON y determinar si representa un token de acceso de desarrollo local o credenciales de servicio. Esto se puede lograr fácilmente comprobando la existencia de la variable `.accessToken` , que solo existe para el token de acceso de desarrollo local JSON.

   Si se proporcionan credenciales de servicio, la aplicación genera un JWT y lo intercambia con Adobe IMS para obtener un token de acceso. Utilice la variable [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)&#39;s `auth(...)` que genera un JWT y lo intercambia por un token de acceso en una sola llamada de función. Los parámetros para `auth(..)` el método es [Objeto JSON formado por información específica](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponible desde el JSON de credenciales de servicio, como se describe a continuación en el código.

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

    Ahora, según el archivo JSON (el token de acceso de desarrollo local JSON o el JSON de credenciales de servicio) que se pase a través del parámetro de línea de comandos &quot;file&quot;, la aplicación obtendrá un token de acceso.
    
    Recuerde que mientras que las credenciales de servicio caducan cada 365 días, el JWT y el token de acceso correspondiente caducan con frecuencia y deben actualizarse antes de que caduquen. Esto se puede hacer utilizando un &quot;token_de_actualización&quot; [proporcionado por Adobe IMS] (https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Con estos cambios en su lugar, el JSON de credenciales de servicio se descargó de AEM Developer Console y, para mayor simplicidad, se guardó como `service_token.json` en la misma carpeta que esta `index.js`. Ahora, vamos a ejecutar la aplicación reemplazando el parámetro de línea de comandos `file` con `service_token.json`y actualizando el `propertyValue` a un nuevo valor para que los efectos se muestren en AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   La salida al terminal tiene el siguiente aspecto:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   La variable __403 - Prohibido__ indica errores en las llamadas de API HTTP a AEM as a Cloud Service. Estos 403 errores prohibidos se producen al intentar actualizar los metadatos de los recursos.

   El motivo de esto es que el token de acceso derivado de Credenciales de Servicio autentica la solicitud para AEM con una cuenta técnica creada automáticamente AEM usuario que, de forma predeterminada, solo tiene acceso de lectura. Para que la aplicación tenga acceso de escritura a AEM, se debe conceder permiso en AEM a la cuenta técnica AEM usuario asociado con el token de acceso.

## Configuración del acceso en AEM

El token de acceso derivado de Credenciales de Servicio utiliza una cuenta técnica AEM Usuario que pertenece al grupo __Colaboradores__ AEM grupo de usuarios.

![Credenciales de Servicio: Cuenta Técnica AEM Usuario](./assets/service-credentials/technical-account-user.png)

Una vez que la cuenta técnica AEM usuario existe en AEM (después de la primera solicitud HTTP con el token de acceso), los permisos de este AEM usuario se pueden administrar del mismo modo que otros usuarios AEM.

1. En primer lugar, busque el nombre de inicio de sesión AEM de la cuenta técnica abriendo el JSON de credenciales de servicio descargado de AEM Developer Console y busque el `integration.email` , que debería tener un aspecto similar a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Inicie sesión en el servicio Autor del entorno de AEM correspondiente como administrador de AEM
1. Navegue hasta __Herramientas__ > __Seguridad__ > __Usuarios__
1. Busque el usuario AEM con la variable __Nombre de inicio de sesión__ identificados en el paso 1 y abra su __Propiedades__
1. Vaya a la __Grupos__ y añada __Usuarios de DAM__ grupo (que como acceso de escritura a los recursos)
1. Toque __Guardar y cerrar__

Con la cuenta técnica permitida en AEM para tener permisos de escritura en los recursos, vuelva a ejecutar la aplicación:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

La salida al terminal tiene el siguiente aspecto:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Verificar los cambios

1. Inicie sesión en el entorno as a Cloud Service AEM que se ha actualizado (con el mismo nombre de host proporcionado en la variable `aem` parámetro de línea de comandos)
1. Vaya a la __Recursos__ > __Archivos__
1. Desplácese por la carpeta de recursos especificada por la variable `folder` parámetro de línea de comandos, por ejemplo __WKND__ > __Inglés__ > __Aventuras__ > __Ayuno de vino de Napa__
1. Abra el __Propiedades__ para cualquier recurso de la carpeta
1. Vaya a la __Avanzadas__ ficha
1. Revise el valor de la propiedad actualizada, por ejemplo __Copyright__ que se asigna a la actualización `metadata/dc:rights` La propiedad JCR, que ahora refleja el valor proporcionado en la variable `propertyValue` parámetro, por ejemplo __Uso restringido de WKND__

![Actualización de metadatos de uso restringido de WKND](./assets/service-credentials/asset-metadata.png)

## Enhorabuena.

Ahora que hemos accedido mediante programación a AEM as a Cloud Service mediante un token de acceso de desarrollo local y un token de acceso de servicio a servicio listo para la producción.

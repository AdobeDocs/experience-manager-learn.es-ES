---
title: Credenciales de servicio
description: Aprenda a utilizar las credenciales de servicio que se utilizan para facilitar las aplicaciones, sistemas y servicios externos e interactuar mediante programación con los servicios de autor o publicación a través de HTTP.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
duration: 957
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1930'
ht-degree: 0%

---

# Credenciales de servicio

Las integraciones con Adobe Experience Manager AEM () as a Cloud Service AEM deben poder autenticarse de forma segura en el servicio de autenticación a través de la autenticación de la autenticación de. AEM AEM Developer Console otorga acceso a las credenciales del servicio, que se utilizan para facilitar que las aplicaciones, los sistemas y los servicios externos interactúen mediante programación con los servicios de autor o publicación de a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Las credenciales del servicio pueden ser similares [Tokens de acceso de desarrollo local](./local-development-access-token.md) pero son diferentes en algunas formas clave:

+ Las credenciales del servicio están asociadas a cuentas técnicas. Se pueden activar varias credenciales de servicio para una cuenta técnica.
+ Las credenciales del servicio son _no_ tokens de acceso, sino que son credenciales utilizadas para lo siguiente _conseguir_ token de acceso.
+ Las credenciales del servicio son más permanentes (sus certificados caducan cada 365 días) y no cambian a menos que se revoquen, mientras que los tokens de acceso de desarrollo local caducan a diario.
+ AEM Las credenciales de servicio de un entorno as a Cloud Service AEM AEM de se asignan a un único usuario de cuenta técnica, mientras que los tokens de acceso de desarrollo local se autentican como el usuario que generó el token de acceso.
+ AEM Un entorno as a Cloud Service AEM puede tener hasta diez cuentas técnicas, cada una con sus propias credenciales de servicio, cada una de las cuales se asigna a un usuario de cuenta técnica independiente de la cuenta de usuario de la cuenta de servicio de la cuenta de usuario de la cuenta.

Tanto las credenciales del servicio como los tokens de acceso que generan, así como los tokens de acceso de desarrollo local, deben mantenerse en secreto. AEM Como los tres se pueden utilizar para obtener, acceso a su respectivo entorno as a Cloud Service de la.

## Generar credenciales de servicio

La generación de credenciales del servicio se divide en dos pasos:

1. Una creación de cuenta técnica única por parte de un administrador de organización de IMS de Adobe
1. La descarga y el uso de las credenciales de servicio JSON de la cuenta técnica

### Crear una cuenta técnica

Las credenciales del servicio, a diferencia de los tokens de acceso de desarrollo local, requieren que un administrador de IMS de organización de Adobe cree una cuenta técnica para poder descargarlas. AEM Deben crearse cuentas técnicas discretas para cada cliente que requiera acceso programático a las cuentas de los clientes de la aplicación de la manera de obtener acceso a las cuentas de la aplicación de la manera de.

![Crear una cuenta técnica](assets/service-credentials/initialize-service-credentials.png)

Las cuentas técnicas se crean una vez, pero las claves privadas que se utilizan para administrar las credenciales de servicio asociadas con la cuenta técnica se pueden administrar a lo largo del tiempo. Por ejemplo, se deben generar nuevas credenciales de clave privada/servicio antes de la caducidad de la clave privada actual para permitir el acceso ininterrumpido de un usuario de las credenciales de servicio.

1. Asegúrese de haber iniciado sesión como:
   + __Administrador del sistema de la organización IMS de Adobe__
   + Miembro de __AEM Administradores de__ Perfil de producto de IMS en __AEM Autor de__
1. Iniciar sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. AEM Abra el programa que contiene el entorno as a Cloud Service de la para integrar y configurar las credenciales de servicio de
1. Pulse los puntos suspensivos junto al entorno en la __Entornos__ y seleccione. __Developer Console__
1. Pulse en __Integraciones__ pestaña
1. Pulse el botón __Cuentas técnicas__ pestaña
1. Tocar __Crear nueva cuenta técnica__ botón
1. Las credenciales de servicio de la cuenta técnica se inicializan y se muestran como JSON

![AEM Consola de desarrollador de: integraciones: obtener credenciales de servicio](./assets/service-credentials/developer-console.png)

AEM Una vez que se han inicializado las credenciales de servicio del entorno de Cloud Service AEM, otros desarrolladores de la organización de IMS de Adobe pueden descargarlas.

### Descargar credenciales del servicio

![Descargar credenciales del servicio](assets/service-credentials/download-service-credentials.png)

La descarga de las credenciales del servicio sigue los pasos similares a los de la inicialización.

1. Asegúrese de haber iniciado sesión como:
   + __Administrador de organización de IMS de Adobe__
   + Miembro de __AEM Administradores de__ Perfil de producto de IMS en __AEM Autor de__
1. Iniciar sesión en [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. AEM Abra el Programa que contiene el entorno as a Cloud Service de la con el que desea integrarse
1. Pulse los puntos suspensivos junto al entorno en la __Entornos__ y seleccione. __Developer Console__
1. Pulse en __Integraciones__ pestaña
1. Pulse el botón __Cuentas técnicas__ pestaña
1. Expanda el __Cuenta técnica__ para utilizar
1. Expanda el __Clave privada__ cuyas credenciales de servicio se descargarán y compruebe que el estado es __Activo__
1. Pulse en __...__ > __Ver__ asociado con el __Clave privada__, que muestra el JSON de credenciales de servicio
1. Pulse el botón de descarga en la esquina superior izquierda para descargar el archivo JSON que contiene el valor de credenciales de servicio y guarde el archivo en una ubicación segura

## Instalar las credenciales del servicio

AEM Las credenciales del servicio proporcionan los detalles necesarios para generar un JWT, que se intercambia por un token de acceso utilizado para autenticarse con el as a Cloud Service de la. AEM Las credenciales del servicio deben almacenarse en una ubicación segura a la que puedan acceder las aplicaciones, los sistemas o los servicios externos que la utilizan para acceder a las credenciales de acceso de los usuarios de la red de distribución de correo electrónico (VECoS) de la red de distribución de correo electrónico. El modo y el lugar en que se administran las credenciales del servicio son únicos para cada cliente.

Para simplificar, este tutorial pasa las credenciales de servicio en a través de la línea de comandos. Sin embargo, trabaje con su equipo de seguridad de TI para comprender cómo almacenar y acceder a estas credenciales de acuerdo con las directrices de seguridad de su organización.

1. Copie el [descargó el JSON de credenciales de servicio](#download-service-credentials) a un archivo denominado `service_token.json` en la raíz del proyecto
   + Recuerda, nunca te comprometas _cualquier credencial_ a Git!

## Usar credenciales de servicio

Las credenciales del servicio, un objeto JSON completamente formado, no son las mismas que el JWT ni el token de acceso. En su lugar, las credenciales del servicio (que contienen una clave privada) se utilizan para generar un JWT, que se intercambia con las API de IMS de Adobe por un token de acceso.

![Credenciales de servicio: aplicación externa](assets/service-credentials/service-credentials-external-application.png)

1. AEM Descargue las credenciales del servicio de la consola de desarrollador de en una ubicación segura
1. AEM La aplicación externa debe interactuar mediante programación con el entorno as a Cloud Service de la aplicación de la aplicación
1. La aplicación externa lee las credenciales del servicio desde una ubicación segura
1. La aplicación externa utiliza la información de las credenciales del servicio para construir un token JWT
1. El token JWT se envía a Adobe IMS a cambio de un token de acceso
1. AEM Adobe IMS devuelve un token de acceso que se puede utilizar para acceder a los datos as a Cloud Service de la
   + Los tokens de acceso no pueden cambiar una hora de caducidad.
1. AEM La aplicación externa realiza solicitudes HTTP a las solicitudes as a Cloud Service, agregando el token de acceso como token de portador al encabezado de autorización de las solicitudes HTTP
1. AEM as a Cloud Service recibe la solicitud HTTP, autentica la solicitud, realiza el trabajo solicitado por la solicitud HTTP y devuelve una respuesta HTTP de nuevo a la aplicación externa

### Actualizaciones de la aplicación externa

AEM Para acceder a las credenciales as a Cloud Service mediante el uso de las credenciales del servicio, la aplicación externa debe actualizarse de tres formas:

1. Leer en las credenciales del servicio

+ Para simplificar, las credenciales del servicio se leen desde el archivo JSON descargado; sin embargo, en situaciones de uso real, las credenciales del servicio deben almacenarse de forma segura de acuerdo con las directrices de seguridad de su organización

1. Generar un JWT a partir de las credenciales de servicio
1. Intercambio del JWT por un token de acceso

+ AEM Cuando hay credenciales de servicio presentes, la aplicación externa utiliza este token de acceso en lugar del token de acceso de desarrollo local, al acceder a las credenciales as a Cloud Service de la

En este tutorial, Adobe `@adobe/jwt-auth` El módulo npm se utiliza para (1) generar el JWT a partir de las credenciales del servicio y (2) cambiarlo por un token de acceso, en una sola llamada de función. Si su aplicación no está basada en JavaScript, consulte la [código de muestra en otros idiomas](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) para obtener información sobre cómo crear un JWT a partir de las credenciales de servicio e intercambiarlo por un token de acceso con Adobe IMS.

## Leer las credenciales del servicio

Revise la `getCommandLineParams()` Por lo tanto, consulte cómo se lee el archivo JSON de credenciales de servicio con el mismo código utilizado para leer en el JSON del token de acceso de desarrollo local.

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

## Creación de un JWT e intercambio de un token de acceso

Una vez leídas las credenciales del servicio, se utilizan para generar un JWT que luego se intercambia con las API de Adobe IMS por un token de acceso. AEM A continuación, este token de acceso se puede utilizar para acceder a los as a Cloud Service de la.

Esta aplicación de ejemplo está basada en Node.js, por lo que es mejor utilizar [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm para facilitar la (1) generación de JWT y el intercambio de (20) con Adobe IMS. Si la aplicación se ha desarrollado en otro idioma, consulte [los ejemplos de código adecuados](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) explica cómo construir la solicitud HTTP a Adobe IMS utilizando otros lenguajes de programación.

1. Actualice el `getAccessToken(..)` para inspeccionar el contenido del archivo JSON y determinar si representa un token de acceso de desarrollo local o credenciales de servicio. Esto se puede lograr fácilmente comprobando la existencia del `.accessToken` , que solo existe para el token de acceso de desarrollo local JSON.

   Si se proporcionan credenciales de servicio, la aplicación genera un JWT e lo intercambia con Adobe IMS por un token de acceso. Utilice el [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)de `auth(...)` que genera un JWT e lo intercambia por un token de acceso en una sola llamada de función. Los parámetros para `auth(..)` El método es un [Objeto JSON compuesto de información específica](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponible desde el JSON de credenciales de servicio, como se describe a continuación en el código.

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

    Ahora, según el archivo JSON (el token de acceso de desarrollo local JSON o el JSON de credenciales de servicio) que se pase a través de ese parámetro de línea de comandos &quot;file&quot;, la aplicación derivará un token de acceso.
    
    Recuerde que, mientras que las credenciales de servicio caducan cada 365 días, el JWT y el token de acceso correspondiente caducan con frecuencia y deben actualizarse antes de que caduquen. Esto se puede hacer con un refresh_token [proporcionado por Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. AEM Con estos cambios en su lugar, el JSON de credenciales de servicio se descargó de la consola de desarrollador de y, para simplificar, se guardó como `service_token.json` en la misma carpeta que esta `index.js`. Ahora, vamos a ejecutar la aplicación reemplazando el parámetro de línea de comandos `file` con `service_token.json`y actualizando el `propertyValue` AEM a un nuevo valor, de modo que los efectos sean evidentes en el caso de los.

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

   El __403 - Prohibido__ AEM , indican errores en las llamadas a la API HTTP para as a Cloud Service. Estos errores 403 prohibidos se producen al intentar actualizar los metadatos de los recursos.

   AEM AEM El motivo es que el token de acceso derivado de las credenciales de servicio autentica la solicitud a los usuarios de la cuenta técnica creada automáticamente, que de forma predeterminada solo tiene acceso de lectura, y que se crea de forma automática con un usuario de la cuenta de usuario de la cuenta técnica de la que se crea la solicitud. AEM AEM AEM Para proporcionar a la aplicación acceso de escritura a los usuarios, se debe conceder permiso de escritura a la cuenta técnica del usuario asociado con el token de acceso en la.

## AEM Configuración del acceso en la

AEM El token de acceso derivado de las credenciales del servicio utiliza una cuenta técnica de usuario que es miembro de la cuenta de usuario de. __Colaboradores__ AEM grupo de usuarios de.

![AEM Credenciales del servicio: usuario de cuenta técnica de](./assets/service-credentials/technical-account-user.png)

AEM AEM AEM AEM Una vez que la cuenta técnica de usuario de la cuenta de usuario existe en el entorno de trabajo (después de la primera solicitud HTTP con el token de acceso), los permisos de este usuario de la cuenta de usuario se pueden administrar de la misma manera que los demás usuarios de la cuenta de trabajo de la cuenta de usuario de la cuenta de usuario de la cuenta de.

1. AEM AEM En primer lugar, busque el nombre de inicio de sesión de la cuenta técnica de abriendo el archivo JSON de credenciales de servicio descargado de la consola de desarrollador de y busque el `integration.email` , que debería ser similar a: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. AEM AEM Inicie sesión en el servicio de creación del entorno de la correspondiente como administrador de la aplicación
1. Vaya a __Herramientas__ > __Seguridad__ > __Usuarios__
1. AEM Busque el usuario de la con __Nombre de inicio__ identificado en el paso 1 y abra su __Propiedades__
1. Vaya a __Grupos__ y agregue la pestaña __Usuarios de DAM__ grupo (que tiene acceso de escritura a los recursos)
   + [AEM Consulte la lista de grupos de usuarios proporcionados por el](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html#built-in-users-and-groups) para agregar el usuario del servicio a para obtener los permisos óptimos. AEM Si ningún grupo de usuarios proporcionado es suficiente, cree el suyo propio y agregue los permisos adecuados.
1. Tocar __Guardar y cerrar__

AEM Con la cuenta técnica permitida en los recursos para tener permisos de escritura en los recursos, vuelva a ejecutar la aplicación:

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

## Verifique los cambios

1. AEM Inicie sesión en el entorno as a Cloud Service de que se ha actualizado (utilizando el mismo nombre de host proporcionado en la variable `aem` parámetro de línea de comandos)
1. Vaya a __Assets__ > __Archivos__
1. Navegue hasta la carpeta de recursos especificada por la variable `folder` parámetro de línea de comandos, por ejemplo __WKND__ > __Inglés__ > __Aventuras__ > __Cata de vinos de Napa__
1. Abra el __Propiedades__ para cualquier recurso de la carpeta
1. Vaya a __Avanzadas__ pestaña
1. Revise el valor de la propiedad actualizada, por ejemplo __Copyright__ que se asigna al actualizado `metadata/dc:rights` La propiedad JCR, que ahora refleja el valor proporcionado en la variable `propertyValue` parámetro, por ejemplo __Uso restringido de WKND__

![Actualización de metadatos de uso restringido de WKND](./assets/service-credentials/asset-metadata.png)

## Enhorabuena.

AEM Ahora que hemos accedido mediante programación a los as a Cloud Service utilizando un token de acceso de desarrollo local y un token de acceso de servicio a servicio listo para la producción,

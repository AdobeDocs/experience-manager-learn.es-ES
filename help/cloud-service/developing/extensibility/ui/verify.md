---
title: AEM Verificar una extensión de IU de
description: AEM Obtenga información sobre cómo obtener una vista previa, probar y comprobar una extensión de la interfaz de usuario de la aplicación antes de implementarla en producción.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
duration: 600
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '739'
ht-degree: 0%

---

# Verificar una extensión

AEM AEM Las extensiones de interfaz de usuario se pueden comprobar con cualquier entorno as a Cloud Service de la organización de Adobe a la que pertenezca la extensión.

AEM La prueba de una extensión se realiza a través de una dirección URL especialmente diseñada que indica a los que carguen la extensión, solo para esa solicitud.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> El vídeo anterior muestra el uso de una extensión de la consola Fragmento de contenido para ilustrar la previsualización y verificación de la aplicación de la extensión App Builder. AEM Sin embargo, es importante tener en cuenta que los conceptos cubiertos pueden aplicarse a todas las extensiones de la interfaz de usuario de la.

## AEM URL de interfaz de usuario

![AEM URL de consola de fragmento de contenido](./assets/verify/content-fragment-console-url.png){align="center"}

AEM AEM Para crear una dirección URL que monte la extensión que no es de producción en la interfaz de usuario de, debe obtenerse la dirección URL de la interfaz de usuario de la interfaz de usuario en la que se inserta la extensión. AEM Vaya al entorno as a Cloud Service de la extensión en el que se va a comprobar la extensión y abra la interfaz de usuario en la que se va a obtener una vista previa de la extensión.

Por ejemplo, para previsualizar una extensión para la consola Fragmento de contenido:

1. AEM Inicie sesión en el entorno as a Cloud Service de deseado.
1. Seleccione el __Fragmentos de contenido__ icono.
1. AEM Espere a que se cargue la consola Fragmento de contenido de en el explorador.
1. AEM Copie la URL de la consola Fragmento de contenido de la barra de direcciones del explorador, debe tener un aspecto similar al siguiente:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

A continuación, se utiliza esta URL al crear las URL para el desarrollo y la verificación por fases. AEM Si verifica la extensión con otras IU de, consiga esas URL y aplique los mismos pasos a continuación.

## Verificar compilaciones de desarrollo local

1. Abra una línea de comandos en la raíz del proyecto de extensión.
1. AEM Ejecute la extensión de la interfaz de usuario de la aplicación como aplicación local del generador de aplicaciones

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Tome nota de la URL de la aplicación local, que se muestra arriba como `-> https://localhost:9080`

1. Inicialmente (y siempre que vea un error de conexión) abierto `https://localhost:9080` (o la URL de la aplicación local que sea) en el explorador web y acepte manualmente [el certificado HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users).
1. Agregue los dos parámetros de consulta siguientes al [AEM URL de la interfaz de usuario](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, normalmente `&ext=https://localhost:9080`.

   Agregue los dos parámetros de consulta anteriores (`devMode` y `ext`) como __primero__ parámetros de consulta en la dirección URL. AEM Las IU ampliables de la interfaz de usuario de utilizan rutas hash (`#/@wknd/aem/...`), por lo que corrige incorrectamente los parámetros después de `#` no funciona.

   La URL de vista previa debe tener este aspecto:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie y pegue la dirección URL de vista previa en el explorador.

   + Es posible que tenga que inicialmente, y luego periódicamente, [aceptar el certificado HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) para el host de la aplicación local (`https://localhost:9080`).

1. AEM La interfaz de usuario de se carga con la versión local de la extensión insertada en ella para la verificación.

>[!IMPORTANT]
>
>AEM Recuerde, al utilizar este método, la extensión en desarrollo solo afecta a su experiencia de y todos los demás usuarios de la interfaz de usuario de la interfaz de usuario de la experiencia de la interfaz de usuario de sin la extensión insertada.

## Verificar compilaciones de fase

1. Abra una línea de comandos en la raíz del proyecto de extensión.
1. Asegúrese de que el espacio de trabajo de fase esté activo (o el espacio de trabajo que se utilice para la verificación).

   ```shell
   $ aio app use -w Stage
   ```

   Combinar cualquier cambio en `.env` y `.aio`.

1. Implemente la extensión actualizada App Builder. Si no ha iniciado sesión, ejecute `aio login` primero.

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. Agregue los dos parámetros de consulta siguientes al [AEM URL de la interfaz de usuario](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Agregue los dos parámetros de consulta anteriores (`devMode` y `ext`) como __primero__ AEM parámetros de consulta en la dirección URL, ya que las interfaces de usuario ampliables utilizan una ruta hash (`#/@wknd/aem/...`), por lo que corrige incorrectamente los parámetros después de `#` no funciona.

   La URL de vista previa debe tener este aspecto:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie y pegue la dirección URL de vista previa en el explorador.
1. AEM La consola Fragmento de contenido de la aplicación inserta la versión de la extensión implementada en el espacio de trabajo de fase en. Esta URL de fase se puede compartir con los usuarios de control de calidad o empresariales para su verificación.

AEM Recuerde, al utilizar este método, la extensión Ensayada solo se inserta en la consola Fragmento de contenido de la consola cuando se accede a con la URL de ensayo artesanal.

1. Las extensiones implementadas se pueden actualizar ejecutando `aio app deploy` de nuevo, y estos cambios se reflejan automáticamente al utilizar la dirección URL de vista previa.
1. Para quitar una extensión para su verificación, ejecute `aio app undeploy`.

## Previsualizar bookmarklet

Para facilitar la creación de las direcciones URL de vista previa y de vista previa descritas anteriormente, se puede crear un bookmarklet JavaScript que cargue la extensión.

El bookmarklet siguiente muestra una vista previa del [compilaciones de desarrollo local](#verify-local-development-builds) de la extensión el `https://localhost:9080`. Para previsualizar [compilaciones de fase](#verify-stage-builds), cree un bookmarklet con `previewApp` se establece en la URL de la aplicación App Builder implementada.

1. Cree un marcador en el explorador.
1. Edite el marcador.
1. Asigne a un marcador un nombre significativo, como `AEM UI Extension Preview (localhost:9080)`.
1. Establezca la dirección URL del marcador en el siguiente código:

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

1. AEM Vaya a una interfaz de usuario de ampliable en la que cargar la extensión de vista previa y, a continuación, haga clic en el bookmarklet.

>[!TIP]
>
> Si la extensión del Generador de aplicaciones no se carga, al utilizar, `&ext=https://localhost:9080`, abra ese host y puerto directamente en una pestaña del explorador y acepte el certificado firmado automáticamente. A continuación, vuelva a intentar el bookmarklet.

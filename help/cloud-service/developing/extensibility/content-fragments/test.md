---
title: Prueba de una extensión de la consola de fragmentos de contenido de AEM
description: Obtenga información sobre cómo probar una extensión de la consola de fragmentos de contenido AEM antes de implementarla en producción.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---


# Probar una extensión

AEM las extensiones de la consola de fragmentos de contenido se pueden probar con cualquier entorno as a Cloud Service AEM de la organización de Adobe a la que pertenezca la extensión.

La prueba de una extensión se realiza mediante una URL especialmente diseñada que indica a la consola Fragmento de contenido de AEM que cargue la extensión.

## URL de la consola de fragmento de contenido de AEM

![URL de la consola de fragmento de contenido de AEM](./assets/test/content-fragment-console-url.png){align="center"}

Para crear una URL que monte la extensión que no es de producción en una consola de fragmento de contenido AEM, se debe recopilar la URL de la consola de fragmento de contenido de AEM deseada. Vaya al entorno as a Cloud Service de AEM para probar la extensión en y copie la URL de su consola de fragmentos de contenido AEM.

1. Inicie sesión en el AEM as a Cloud Service env deseado.

   + Usar un entorno de desarrollo de AEM para [probar compilaciones de desarrollo](#testing-development-builds)
   + Utilice el entorno de desarrollo de fase de AEM o control de calidad para [probar compilaciones de etapa](#testing-stage-builds)

1. Seleccione el __Fragmentos de contenido__ icono.
1. Espere a que la Consola de fragmentos de contenido de AEM se cargue en el explorador.
1. Copie la URL de la Consola de fragmento de contenido de AEM desde la barra de direcciones del explorador; debe parecerse a:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Esta dirección URL se utiliza a continuación al crear las direcciones URL para desarrollo y pruebas de fase.

## Probar compilaciones de desarrollo local

1. Abra una línea de comandos en la raíz del proyecto de extensión.
1. Ejecute la extensión de la consola AEM fragmento de contenido como una aplicación local de App Builder

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Tenga en cuenta la URL de la aplicación local que se muestra arriba como `-> https://localhost:9080`

1. Agregue los dos parámetros de consulta siguientes al [URL de la consola de fragmento de contenido de AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, normalmente `&ext=https://localhost:9080`.

   La dirección URL de prueba debería tener el siguiente aspecto:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://localhost:9080
   ```

1. Copie y pegue la dirección URL de prueba en el explorador.

   + Es posible que tenga que empezar, y luego periódicamente, debe [aceptar el certificado HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) para el host de la aplicación local (`https://localhost:9080`).

1. La consola de fragmento de contenido de AEM se carga con la versión local de la extensión insertada para probarla y se vuelven a cargar los cambios durante todo el tiempo que la aplicación local de App Builder se esté ejecutando.

>[!IMPORTANT]
>
>Recuerde que, al utilizar este método, la extensión en desarrollo solo afecta a su experiencia y todos los demás usuarios de la consola Fragmento de contenido de AEM acceden a ella sin la extensión insertada.


## Probar compilaciones de etapas

1. Abra una línea de comandos en la raíz del proyecto de extensión.
1. Asegúrese de que el espacio de trabajo del escenario esté activo (o que sea el espacio de trabajo que se utilice para las pruebas).

   ```shell
   $ aio app use -w Stage
   ```
   Combinar cualquier cambio en `.env` y `.aio`.
1. Implemente la extensión actualizada de la aplicación App Builder. Si no ha iniciado sesión, ejecute `aio login` primero.

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

1. Agregue los dos parámetros de consulta siguientes al [URL de la consola de fragmento de contenido de AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   La dirección URL de prueba debería tener el siguiente aspecto:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin&devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html
   ```

1. Copie y pegue la dirección URL de prueba en el explorador.
1. La consola Fragmento de contenido de AEM inserta la versión de la extensión implementada en el espacio de trabajo de ensayo en. Esta URL de fase se puede compartir con usuarios de control de calidad o empresariales para realizar pruebas.

Recuerde que al utilizar este método, la extensión de ensayo solo se inserta en AEM consola de fragmento de contenido cuando se accede a con la URL de ensayo de la versión de diseño.

1. Las extensiones implementadas se pueden actualizar ejecutando `aio app deploy` y estos cambios se reflejan automáticamente al usar la dirección URL de prueba.
1. Para quitar una extensión para pruebas, ejecute `aio app undeploy`.




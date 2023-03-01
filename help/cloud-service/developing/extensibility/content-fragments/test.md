---
title: AEM Prueba de una extensión de la consola Fragmentos de contenido de
description: AEM Obtenga información sobre cómo probar una extensión de la consola Fragmento de contenido de antes de implementarla en producción.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: 1a4ee470a650aacc5412fbd27062ca14ccdb1967
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Prueba de una extensión

AEM AEM Las extensiones de la consola Fragmento de contenido se pueden probar con cualquier entorno as a Cloud Service de la organización de Adobe a la que pertenezca la extensión.

AEM La prueba de una extensión se realiza mediante una dirección URL especialmente diseñada que indica a la consola de fragmentos de contenido de la que cargue la extensión.

>[!VIDEO](https://video.tv.adobe.com/v/3412877/?quality=12&learn=on)

## AEM URL de consola de fragmento de contenido

![AEM URL de consola de fragmento de contenido](./assets/test/content-fragment-console-url.png){align="center"}

AEM AEM Para crear una dirección URL que monte la extensión que no sea de producción en una consola de fragmento de contenido de, se debe recopilar la dirección URL de la consola de fragmento de contenido de la aplicación que desee. AEM Vaya al entorno as a Cloud Service AEM de la en el que probar la extensión y copie la URL de su Consola de fragmento de contenido de la misma.

1. AEM Inicie sesión en el entorno as a Cloud Service de deseado.

   + AEM Uso de un entorno de desarrollo de para [prueba de compilaciones de desarrollo](#testing-development-builds)
   + AEM Uso del entorno de desarrollo de fase o control de calidad de para [pruebas de compilaciones de fase](#testing-stage-builds)

1. Seleccione el __Fragmentos de contenido__ icono.
1. AEM Espere a que se cargue la consola Fragmento de contenido de en el explorador.
1. AEM Copie la URL de la consola Fragmento de contenido de la barra de direcciones del explorador, debe tener un aspecto similar al siguiente:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

A continuación, se utiliza esta URL al crear las URL para el desarrollo y la prueba de fase.

## Prueba de compilaciones de desarrollo local

1. Abra una línea de comandos en la raíz del proyecto de extensión.
1. AEM Ejecute la extensión de la consola Fragmento de contenido de la aplicación como aplicación local del App Builder

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

1. Agregue los dos parámetros de consulta siguientes al [AEM URL de la consola de fragmento de contenido de la](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, normalmente `&ext=https://localhost:9080`.

   Agregue los dos parámetros de consulta anteriores (`devMode` y `ext`) como __primero__ parámetros de consulta en la dirección URL, ya que la consola Fragmento de contenido utiliza una ruta hash (`#/@wknd/aem/...`), por lo que corrige incorrectamente los parámetros después de `#` no funcionará.

   La URL de prueba debe tener un aspecto similar al siguiente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie y pegue la dirección URL de prueba en el explorador.

   + Es posible que tenga que hacerlo inicialmente, y luego periódicamente, debe [aceptar el certificado HTTPS](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) para el host de la aplicación local (`https://localhost:9080`).

1. AEM La consola Fragmento de contenido se carga con la versión local de la extensión insertada en ella para realizar pruebas y cambios de recarga en caliente durante todo el tiempo en que se esté ejecutando la aplicación local App Builder.

>[!IMPORTANT]
>
>AEM Recuerde, al utilizar este enfoque, la extensión en desarrollo solo afecta a su experiencia y todos los demás usuarios de la consola Fragmento de contenido de la aplicación acceden a ella sin la extensión insertada.


## Compilaciones de fase de prueba

1. Abra una línea de comandos en la raíz del proyecto de extensión.
1. Asegúrese de que el espacio de trabajo de fase esté activo (o el espacio de trabajo que se utilice para realizar pruebas).

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

1. Agregue los dos parámetros de consulta siguientes al [AEM URL de la consola de fragmento de contenido de la](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Agregue los dos parámetros de consulta anteriores (`devMode` y `ext`) como __primero__ parámetros de consulta en la dirección URL, ya que la consola Fragmento de contenido utiliza una ruta hash (`#/@wknd/aem/...`), por lo que corrige incorrectamente los parámetros después de `#` no funcionará.

   La URL de prueba debe tener un aspecto similar al siguiente:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copie y pegue la dirección URL de prueba en el explorador.
1. AEM La consola Fragmento de contenido de la aplicación inserta la versión de la extensión implementada en el espacio de trabajo de fase en. Esta URL de fase se puede compartir con los usuarios de control de calidad o empresariales para realizar pruebas.

AEM Recuerde, al utilizar este método, la extensión Ensayada solo se inserta en la consola Fragmento de contenido de la consola cuando se accede a con la URL de ensayo artesanal.

1. Las extensiones implementadas se pueden actualizar ejecutando `aio app deploy` de nuevo, y estos cambios se reflejan automáticamente al utilizar la dirección URL de prueba.
1. Para quitar una extensión con fines de prueba, ejecute lo siguiente `aio app undeploy`.




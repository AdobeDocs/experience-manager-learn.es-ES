---
title: AEM Implementación de una extensión de IU de
description: AEM Obtenga información sobre cómo implementar una extensión de IU de.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 1%

---

# Implementación de una extensión

AEM Para su uso en entornos as a Cloud Service de, la aplicación App Builder de extensión debe implementarse y aprobarse.

Al implementar aplicaciones de App Builder de extensión, hay que tener en cuenta varias consideraciones:

+ Las extensiones se implementan en el espacio de trabajo del proyecto de la consola Adobe Developer. Los espacios de trabajo predeterminados son:
   + __Producción__ AEM workspace contiene implementaciones de extensión disponibles en todas las áreas de trabajo as a Cloud Service.
   + __Fase__ workspace actúa como un espacio de trabajo de desarrollador. AEM Las extensiones implementadas en el espacio de trabajo de fase no están disponibles en el as a Cloud Service de la.
Los espacios de trabajo de la consola de Adobe Developer AEM no tienen ninguna correlación directa con los tipos de entorno as a Cloud Service de la.
+ AEM Una extensión implementada en el espacio de trabajo de producción se muestra en todos los entornos as a Cloud Service de la organización de Adobe en los que se encuentra la extensión, en todos los entornos de producción.
Una extensión no se puede limitar a los entornos en los que está registrada añadiendo [AEM lógica condicional que comprueba el nombre de host as a Cloud Service de la](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ AEM Se pueden utilizar varias extensiones en las as a Cloud Service. El Adobe recomienda que cada aplicación de App Builder de extensión se utilice para solucionar un único objetivo empresarial. Dicho esto, una aplicación App Builder de extensión única puede implementar varios puntos de extensión que apoyen un objetivo comercial común.

## Despliegue inicial

AEM Para que una extensión esté disponible en entornos as a Cloud Service, debe implementarse en la consola de Adobe Developer.

El proceso de implementación se divide en dos pasos lógicos:

1. Implementación de la aplicación App Builder de extensión en la consola de Adobe Developer por un desarrollador.
1. Aprobación de la extensión por un administrador de implementación o un propietario empresarial.

### Implementación de la extensión

Implemente la extensión en el espacio de trabajo de producción. AEM Las extensiones implementadas en el espacio de trabajo de producción se añaden automáticamente a todos los servicios de autor as a Cloud Service de la organización de Adobe en los que se implementa la extensión de.

1. Abra una línea de comandos en la raíz de la extensión actualizada de la aplicación App Builder.
1. Asegúrese de que el espacio de trabajo Producción esté activo

   ```shell
   $ aio app use -w Production
   ```

   Combinar cualquier cambio en `.env` y `.aio`.

1. Implemente la extensión actualizada App Builder.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprobación de implementación

![Enviar extensión para su aprobación](./assets/deploy/submit-for-approval.png){align="center"}

1. Iniciar sesión en [Consola de Adobe Developer](https://developer.adobe.com)
1. Seleccionar __Consola__
1. Vaya a __Proyectos__
1. Seleccione el proyecto asociado a la extensión
1. Seleccione el __Producción__ workspace
1. Seleccionar __Enviar para aprobación__
1. Complete y envíe el formulario, actualizando los campos según sea necesario.

### Aprobación de implementación

![Aprobación de extensión](./assets/deploy/adobe-exchange.png){align="center"}

1. Iniciar sesión en [Intercambio de Adobe](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones pendientes de revisión__
1. __Revisar__ la extensión App Builder
1. Si los cambios de extensión son aceptables __Aceptar__ la revisión. AEM Esto inserta inmediatamente la extensión en todos los servicios de autor as a Cloud Service dentro de la organización de Adobe.

AEM Una vez aprobada la solicitud de extensión, la extensión se activa inmediatamente en los servicios de autor as a Cloud Service de la.

## Actualización de una extensión

La actualización y la extensión de la aplicación App Builder siguen el mismo proceso que la [implementación inicial](#initial-deployment), con la desviación de que la implementación de extensión existente primero debe revocarse.

### Revocar la extensión

Para implementar una nueva versión de una extensión, primero debe revocarse (o eliminarse). AEM Mientras que la extensión es Revocada, no está disponible en consolas de.

1. Iniciar sesión en [Intercambio de Adobe](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones del Generador de aplicaciones__
1. __Revocar__ la extensión para actualizar

### Implementación de la extensión

Implemente la extensión en el espacio de trabajo de producción. AEM Las extensiones implementadas en el espacio de trabajo de producción se añaden automáticamente a todos los servicios de autor as a Cloud Service de la organización de Adobe en los que se implementa la extensión de.

1. Abra una línea de comandos en la raíz de la extensión actualizada de la aplicación App Builder.
1. Asegúrese de que el espacio de trabajo Producción esté activo

   ```shell
   $ aio app use -w Production
   ```

   Combinar cualquier cambio en `.env` y `.aio`.

1. Implemente la extensión actualizada App Builder.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprobación de implementación

![Enviar extensión para su aprobación](./assets/deploy/submit-for-approval.png){align="center"}

1. Iniciar sesión en [Consola de Adobe Developer](https://developer.adobe.com)
1. Seleccionar __Consola__
1. Vaya a __Proyectos__
1. Seleccione el proyecto asociado a la extensión
1. Seleccione el __Producción__ workspace
1. Seleccionar __Enviar para aprobación__
1. Complete y envíe el formulario, actualizando los campos según sea necesario.

#### Aprobación de la solicitud de implementación

![Aprobación de extensión](./assets/deploy/adobe-exchange.png){align="center"}

1. Iniciar sesión en [Intercambio de Adobe](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones pendientes de revisión__
1. __Revisar__ la extensión App Builder
1. Si los cambios de extensión son aceptables __Aceptar__ la revisión. AEM Esto inserta inmediatamente la extensión en todos los servicios de autor as a Cloud Service dentro de la organización de Adobe.

AEM Una vez aprobada la solicitud de extensión, la extensión se activa inmediatamente en los servicios de autor as a Cloud Service de la.

## Eliminación de una extensión

![Eliminación de una extensión](./assets/deploy/revoke.png)

Para eliminar una extensión, revoque (o elimine) su acceso a Adobe Exchange. AEM Cuando se revoca la extensión, se elimina de todos los servicios de autor as a Cloud Service de la.

1. Iniciar sesión en [Intercambio de Adobe](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones del Generador de aplicaciones__
1. __Revocar__ la extensión que se va a eliminar

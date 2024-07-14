---
title: AEM Implementación de una extensión de IU de
description: AEM Obtenga información sobre cómo implementar una extensión de IU de.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
duration: 166
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 0%

---

# Implementación de una extensión

Para su uso en entornos AEM as a Cloud Service, la aplicación de extensión de App Builder debe implementarse y aprobarse.

Hay que tener en cuenta varias consideraciones al implementar aplicaciones de App Builder de extensión:

+ Las extensiones se implementan en el espacio de trabajo del proyecto de Adobe Developer Console. Los espacios de trabajo predeterminados son:
   + El espacio de trabajo __Production__ contiene implementaciones de extensión disponibles en todos los AEM as a Cloud Service.
   + El espacio de trabajo __Stage__ actúa como un espacio de trabajo para desarrolladores. Las extensiones implementadas en el espacio de trabajo de fase no están disponibles en AEM as a Cloud Service.
Los espacios de trabajo de Adobe Developer Console no tienen ninguna correlación directa con los tipos de entorno de AEM as a Cloud Service.
+ Una extensión implementada en el espacio de trabajo de producción se muestra en todos los entornos de AEM as a Cloud Service de la organización de Adobe en los que existe la extensión.
Una extensión no se puede limitar a los entornos con los que está registrada agregando [lógica condicional que comprueba el nombre de host de AEM as a Cloud Service](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ Se pueden utilizar varias extensiones en AEM as a Cloud Service. El Adobe recomienda que cada aplicación de App Builder de extensión se utilice para solucionar un único objetivo empresarial. Dicho esto, una sola aplicación de App Builder de extensión puede implementar varios puntos de extensión que apoyen un objetivo comercial común.

## Despliegue inicial

Para que una extensión esté disponible en entornos AEM as a Cloud Service, debe implementarse en Adobe Developer Console.

El proceso de implementación se divide en dos pasos lógicos:

1. Implementación de la aplicación de extensión de App Builder en Adobe Developer Console por un desarrollador.
1. Aprobación de la extensión por un administrador de implementación o un propietario empresarial.

### Implementación de la extensión

Implemente la extensión en el espacio de trabajo de producción. Las extensiones implementadas en el espacio de trabajo de producción se añaden automáticamente a todos los servicios de AEM as a Cloud Service Author en la organización de Adobe en la que se implementa la extensión.

1. Abra una línea de comandos en la raíz de la aplicación App Builder de extensión actualizada.
1. Asegúrese de que el espacio de trabajo Producción esté activo

   ```shell
   $ aio app use -w Production
   ```

   Combine cualquier cambio realizado en `.env` y `.aio`.

1. Implemente la aplicación de App Builder de extensión actualizada.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprobación de implementación

![Enviar extensión para su aprobación](./assets/deploy/submit-for-approval.png){align="center"}

1. Iniciar sesión en [Adobe Developer Console](https://developer.adobe.com)
1. Seleccionar __consola__
1. Vaya a __Proyectos__
1. Seleccione el proyecto asociado a la extensión
1. Seleccione el área de trabajo __Producción__
1. Seleccionar __Enviar para aprobación__
1. Complete y envíe el formulario, actualizando los campos según sea necesario.

### Aprobación de implementación

![Aprobación de extensión](./assets/deploy/adobe-exchange.png){align="center"}

1. Iniciar sesión en [Adobe Exchange](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones pendientes de revisión__
1. __Revisar__ la aplicación App Builder de extensión
1. Si los cambios de extensión son aceptables __Accept__, realice la revisión. Esto inserta inmediatamente la extensión en todos los servicios de AEM as a Cloud Service Author dentro de la organización de Adobe.

Una vez aprobada la solicitud de extensión, la extensión se activa inmediatamente en los servicios de AEM as a Cloud Service Author.

## Actualización de una extensión

La actualización y la extensión de la aplicación App Builder siguen el mismo proceso que la [implementación inicial](#initial-deployment), con la desviación de que la implementación de extensión existente debe revocarse primero.

### Revocar la extensión

Para implementar una nueva versión de una extensión, primero debe revocarse (o eliminarse). AEM Mientras que la extensión es Revocada, no está disponible en consolas de.

1. Iniciar sesión en [Adobe Exchange](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones App Builder__
1. __Revocar__ la extensión para actualizar

### Implementación de la extensión

Implemente la extensión en el espacio de trabajo de producción. Las extensiones implementadas en el espacio de trabajo de producción se añaden automáticamente a todos los servicios de AEM as a Cloud Service Author en la organización de Adobe en la que se implementa la extensión.

1. Abra una línea de comandos en la raíz de la aplicación App Builder de extensión actualizada.
1. Asegúrese de que el espacio de trabajo Producción esté activo

   ```shell
   $ aio app use -w Production
   ```

   Combine cualquier cambio realizado en `.env` y `.aio`.

1. Implemente la aplicación de App Builder de extensión actualizada.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprobación de implementación

![Enviar extensión para su aprobación](./assets/deploy/submit-for-approval.png){align="center"}

1. Iniciar sesión en [Adobe Developer Console](https://developer.adobe.com)
1. Seleccionar __consola__
1. Vaya a __Proyectos__
1. Seleccione el proyecto asociado a la extensión
1. Seleccione el área de trabajo __Producción__
1. Seleccionar __Enviar para aprobación__
1. Complete y envíe el formulario, actualizando los campos según sea necesario.

#### Aprobación de la solicitud de implementación

![Aprobación de extensión](./assets/deploy/adobe-exchange.png){align="center"}

1. Iniciar sesión en [Adobe Exchange](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones pendientes de revisión__
1. __Revisar__ la aplicación App Builder de extensión
1. Si los cambios de extensión son aceptables __Accept__, realice la revisión. Esto inserta inmediatamente la extensión en todos los servicios de AEM as a Cloud Service Author dentro de la organización de Adobe.

Una vez aprobada la solicitud de extensión, la extensión se activa inmediatamente en los servicios de AEM as a Cloud Service Author.

## Eliminación de una extensión

![Quitar una extensión](./assets/deploy/revoke.png)

Para eliminar una extensión, revoque (o elimine) su Adobe Exchange. Cuando se revoca la extensión, se elimina de todos los servicios de AEM as a Cloud Service Author.

1. Iniciar sesión en [Adobe Exchange](https://exchange.adobe.com/)
1. Vaya a __Administrar__ > __Aplicaciones App Builder__
1. __Revocar__ la extensión que se va a quitar

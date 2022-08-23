---
title: Desarrollo de estados de recursos en AEM Sites
description: 'La API del estado de los recursos de Adobe Experience Manager es un marco conectable para exponer los mensajes de estado en AEM varias IU web de editor. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Desarrollo de estados de recursos {#developing-resource-statuses-in-aem-sites}

La API del estado de los recursos de Adobe Experience Manager es un marco conectable para exponer los mensajes de estado en AEM varias IU web de editor.

## Información general {#overview}

El marco Estado de los recursos para editores proporciona API del lado del servidor y del lado del cliente para mostrar los estados del editor y para interactuar con ellos de forma estándar y uniforme.

Las barras de estado del editor están disponibles de forma nativa en los editores de página, fragmento de experiencia y plantilla de AEM.

Algunos ejemplos de casos de uso para los proveedores de estado de recursos personalizados son:

* Notificación a los autores cuando una página esté dentro de las 2 horas siguientes a la activación programada
* Notificación a los autores de que una página se ha activado en los últimos 15 minutos
* Notificación a los autores de que una página se ha editado en los últimos 5 minutos y por quién

![Descripción general del estado de los recursos del editor de AEM](assets/sample-editor-resource-status-screenshot.png)

## Marco del proveedor de estado de recurso {#resource-status-provider-framework}

Al desarrollar estados de recursos personalizados, el trabajo de desarrollo consiste en:

1. La implementación de ResourceStatusProvider, que es responsable de determinar si se requiere un estado, y la información básica sobre el estado: título, mensaje, prioridad, variante, icono y acciones disponibles.
2. De forma opcional, GraniteUI JavaScript que implementa la funcionalidad de cualquier acción disponible.

   ![arquitectura del estado de los recursos](assets/sample-editor-resource-status-application-architecture.png)

3. El recurso de estado proporcionado como parte de los editores de página, fragmento de experiencia y plantilla tiene un tipo a través de los recursos &quot;[!DNL statusType]&quot;.

   * Editor de páginas: `editor`
   * Editor de fragmentos de experiencias: `editor`
   * Editor de plantillas: `template-editor`

4. El recurso de estado `statusType` coincide con registrado `CompositeStatusType` OSGi configurado `name` propiedad.

   Para todas las coincidencias, la variable `CompositeStatusType's` se recopilan y se usan para recopilar los `ResourceStatusProvider` implementaciones que tienen este tipo, mediante `ResourceStatusProvider.getType()`.

5. La coincidencia `ResourceStatusProvider` se pasa la variable `resource` en el editor y determina si la variable `resource` tiene que mostrarse el estado . Si se necesita el estado, esta implementación es responsable de la compilación de 0 o varios `ResourceStatuses` para que se devuelvan, cada uno representa un estado que se va a mostrar.

   Normalmente, un `ResourceStatusProvider` devuelve 0 o 1 `ResourceStatus` per `resource`.

6. ResourceStatus es una interfaz que puede implementar el cliente o la `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` para construir un estado. Un estado consta de:

   * Título
   * Mensaje
   * Icono
   * Variante
   * Prioridad
   * Acciones
   * Datos

7. Opcionalmente, si `Actions` se proporcionan para la variable `ResourceStatus` , se requiere clientlibs de soporte para enlazar la funcionalidad a los vínculos de acción en la barra de estado.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Cualquier JavaScript o CSS compatible para admitir las acciones debe procesarse como proxy a través de las respectivas bibliotecas de cliente de cada editor para garantizar que el código front-end esté disponible en el editor.

   * Categoría del editor de páginas: `cq.authoring.editor.sites.page`
   * Categoría del editor de fragmentos de experiencia: `cq.authoring.editor.sites.page`
   * Categoría del editor de plantillas: `cq.authoring.editor.sites.template`

## Ver el código {#view-the-code}

[Consulte código en GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Recursos adicionales {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)

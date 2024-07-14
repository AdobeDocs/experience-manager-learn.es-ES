---
title: Desarrollo de estados de recursos en AEM Sites
description: La API de estado de recursos de Adobe Experience Manager AEM es un marco conectable para exponer la mensajería de estado en la creación de varias IU web de editor de datos de usuario (IU) de la interfaz de usuario de Adobe.
doc-type: Tutorial
version: 6.4, 6.5
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 2%

---


# Desarrollo de estados de recursos {#developing-resource-statuses-in-aem-sites}

La API de estado de recursos de Adobe Experience Manager AEM es un marco conectable para exponer la mensajería de estado en la creación de varias IU web de editor de datos de usuario (IU) de la interfaz de usuario de Adobe.

## Información general {#overview}

El marco de trabajo Estado de los recursos para editores proporciona API del lado del servidor y del lado del cliente para mostrar e interactuar con los estados del editor, de una manera estándar y uniforme.

AEM Las barras de estado del editor están disponibles de forma nativa en los editores de páginas, fragmentos de experiencias y plantillas de la página de.

Casos de uso de ejemplo para proveedores de estado de recursos personalizados:

* Notificar a los autores cuando una página se encuentra dentro de las 2 horas de la activación programada
* Notificar a los autores que una página se ha activado en los últimos 15 minutos
* Notificar a los autores que una página se editó en los últimos 5 minutos y quién la editó

AEM ![Resumen del estado del recurso del editor de la](assets/sample-editor-resource-status-screenshot.png)

## Marco del proveedor de estado de recursos {#resource-status-provider-framework}

Al desarrollar estados de recursos personalizados, el trabajo de desarrollo consta de:

1. La implementación de ResourceStatusProvider, que es responsable de determinar si un estado es necesario, y la información básica sobre el estado: título, mensaje, prioridad, variante, icono y acciones disponibles.
2. De forma opcional, GraniteUI JavaScript que implementa la funcionalidad de cualquier acción disponible.

   ![arquitectura de estado de recursos](assets/sample-editor-resource-status-application-architecture.png)

3. El recurso de estado proporcionado como parte de los editores de página, fragmento de experiencia y plantilla recibe un tipo a través de la propiedad de recursos &quot;[!DNL statusType]&quot;.

   * Editor de páginas: `editor`
   * Editor de fragmentos de experiencias: `editor`
   * Editor de plantillas: `template-editor`

4. El recurso de estado `statusType` coincide con la propiedad `CompositeStatusType` OSGi configurada `name` registrada.

   Para todas las coincidencias, se recopilan los tipos `CompositeStatusType's` y se utilizan para recopilar las implementaciones `ResourceStatusProvider` que tienen este tipo, a través de `ResourceStatusProvider.getType()`.

5. El elemento `ResourceStatusProvider` coincidente se pasa al elemento `resource` en el editor y determina si el elemento `resource` tiene el estado que se va a mostrar. Si el estado es necesario, esta implementación es responsable de generar 0 o varios `ResourceStatuses` para devolver, cada uno de los cuales representa un estado que mostrar.

   Normalmente, un(a) `ResourceStatusProvider` devuelve 0 o 1 `ResourceStatus` por `resource`.

6. ResourceStatus es una interfaz que el cliente puede implementar, o bien se puede usar el elemento `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` para construir un estado. Un estado consta de:

   * Título
   * Mensaje
   * Icono
   * Variante
   * Prioridad
   * Acciones
   * Datos

7. De forma opcional, si se proporcionan `Actions` para el objeto `ResourceStatus`, se requieren clientlibs de compatibilidad para enlazar la funcionalidad a los vínculos de acción en la barra de estado.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Cualquier JavaScript o CSS compatible para admitir las acciones debe procesarse como proxy a través de las bibliotecas de cliente respectivas de cada editor para garantizar que el código front-end esté disponible en el editor.

   * Categoría del editor de páginas: `cq.authoring.editor.sites.page`
   * Categoría del editor del Fragmento de experiencia: `cq.authoring.editor.sites.page`
   * Categoría del editor de plantillas: `cq.authoring.editor.sites.template`

## Ver el código {#view-the-code}

[Ver código en GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Recursos adicionales {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)

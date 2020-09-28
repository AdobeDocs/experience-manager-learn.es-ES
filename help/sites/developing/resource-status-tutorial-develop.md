---
title: Desarrollo de estados de recursos en AEM Sites
description: 'Las API de estado de recursos de Adobe Experience Manager son un marco conectable para exponer los mensajes de estado en AEM varias IU web de editor. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Desarrollo de estados de recursos {#developing-resource-statuses-in-aem-sites}

Las API de estado de recursos de Adobe Experience Manager son un marco conectable para exponer los mensajes de estado en AEM varias IU web de editor.

## Información general {#overview}

El marco de estado de recursos para editores proporciona API del lado del servidor y del lado del cliente para mostrar e interactuar con los estados del editor, de forma estándar y uniforme.

Las barras de estado del editor están disponibles de forma nativa en los editores de página, fragmento de experiencia y plantilla de AEM.

Algunos ejemplos de casos de uso para proveedores de estado de recursos personalizados son:

* Notificar a los autores cuando una página se encuentra dentro de las 2 horas de la activación programada
* Notificación a los autores de que se activó una página en los últimos 15 minutos
* Notificación a los autores de que una página se ha editado en los últimos 5 minutos y por quién

![Descripción general del estado de los recursos del editor de AEM](assets/sample-editor-resource-status-screenshot.png)

## Marco del proveedor de estado de recursos {#resource-status-provider-framework}

Al desarrollar estados de recursos personalizados, el trabajo de desarrollo consiste en:

1. La implementación ResourceStatusProvider, que es responsable de determinar si se requiere un estado, y la información básica sobre el estado: título, mensaje, prioridad, variante, icono y acciones disponibles.
2. De forma opcional, JavaScript GraniteUI que implementa la funcionalidad de cualquier acción disponible.

   ![arquitectura de estado de recursos](assets/sample-editor-resource-status-application-architecture.png)

3. El recurso de estado proporcionado como parte de los editores Página, Fragmento de experiencia y Plantilla recibe un tipo a través de la propiedad &quot;[!DNL statusType]&quot; de recursos.

   * Page editor: `editor`
   * Experience Fragment editor: `editor`
   * Editor de plantillas: `template-editor`

4. El recurso de estado `statusType` coincide con la propiedad configurada `CompositeStatusType` OSGi registrada `name` .

   Para todas las coincidencias, los `CompositeStatusType's` tipos se recopilan y se utilizan para recopilar las `ResourceStatusProvider` implementaciones que tienen este tipo, mediante `ResourceStatusProvider.getType()`.

5. La coincidencia `ResourceStatusProvider` se pasa al editor `resource` en el editor y determina si el estado `resource` tiene que mostrarse. Si se necesita el estado, esta implementación es responsable de generar 0 o varios para `ResourceStatuses` que regresen, cada uno de los cuales representa un estado para mostrar.

   Normalmente, un `ResourceStatusProvider` devuelve 0 o 1 `ResourceStatus` por `resource`.

6. ResourceStatus es una interfaz que el cliente puede implementar, o la ayuda `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` puede utilizarse para construir un estado. Un estado consta de:

   * Título
   * Mensaje
   * Icono
   * Variante
   * Prioridad
   * Acciones
   * Datos

7. De forma opcional, si `Actions` se proporcionan para el `ResourceStatus` objeto, se requiere que los clientes de soporte técnico enlacen la funcionalidad a los vínculos de acción en la barra de estado.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Cualquier código JavaScript o CSS compatible para admitir las acciones debe procesarse como proxy a través de las bibliotecas de cliente respectivas de cada editor para garantizar que el código front-end esté disponible en el editor.

   * Categoría del editor de páginas: `cq.authoring.editor.sites.page`
   * Categoría del editor de fragmentos de experiencia: `cq.authoring.editor.sites.page`
   * Categoría del editor de plantillas: `cq.authoring.editor.sites.template`

## Vista del código {#view-the-code}

[Ver código en GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Recursos adicionales {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)

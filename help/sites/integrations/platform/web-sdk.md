---
title: Integración de AEM Sites y SDK web de Experience Platform
description: Obtenga información sobre cómo integrar AEM Sites as a Cloud Service con el SDK web de Experience Platform. Este paso fundamental es esencial para integrar productos de Adobe Experience Cloud, como Adobe Analytics, Target o productos innovadores recientes como Real-time Customer Data Platform, Customer Journey Analytics y Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
duration: 1303
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1229'
ht-degree: 1%

---

# Integración de AEM Sites y SDK web de Experience Platform

Aprenda a integrar AEM as a Cloud Service con el Experience Platform [SDK web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Este paso fundamental es esencial para integrar productos de Adobe Experience Cloud, como Adobe Analytics, Target o productos innovadores recientes como Real-time Customer Data Platform, Customer Journey Analytics y Journey Optimizer.

También aprenderá a recopilar y enviar [WKND: datos de vista de página del proyecto de Adobe Experience Manager de ejemplo](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) en el [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Después de completar esta configuración, ha implementado una base sólida. Además, está listo para avanzar en la implementación del Experience Platform mediante aplicaciones como [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=es), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) y [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). La implementación avanzada ayuda a mejorar la participación de los clientes mediante la estandarización de la web y los datos de los clientes.

## Requisitos previos

Se requiere lo siguiente al integrar el SDK web de Experience Platform.

AEM En **como Cloud Service**:

+ AEM Acceso de administrador a la configuración de AEM as a Cloud Service
+ Acceso del administrador de implementación a Cloud Manager
+ Clonar e implementar [WKND: proyecto de Adobe Experience Manager de muestra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) en el entorno de AEM as a Cloud Service.

En **Experience Platform**:

+ Acceso a la zona protegida **Prod** de producción predeterminada.
+ Acceso a **Esquemas** en Administración de datos
+ Acceso a **Conjuntos de datos** en Administración de datos
+ Acceso a **Datastreams** en Recopilación de datos
+ Acceso a **Etiquetas** en Recopilación de datos

Si no cuenta con los permisos necesarios, el administrador del sistema que use [Adobe Admin Console](https://adminconsole.adobe.com/) podrá concederle los permisos necesarios.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Crear esquema XDM: Experience Platform

El esquema del modelo de datos de experiencia (XDM) le ayuda a estandarizar los datos de experiencia del cliente. Para recopilar los datos de **WKND pageview**, cree un esquema XDM y utilice los grupos de campos `AEP Web SDK ExperienceEvent` proporcionados por el Adobe para la recopilación de datos web.

Existen modelos genéricos y específicos de sectores como, por ejemplo, minoristas, de servicios financieros, de atención sanitaria y muchos más, un conjunto de modelos de datos de referencia. Consulte [Descripción general de los modelos de datos de sector](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) para obtener más información.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Obtenga información acerca del esquema XDM y conceptos relacionados, como grupos de campos, tipos, clases y tipos de datos de [XDM System overview](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

La [descripción general del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) es un excelente recurso para obtener información sobre el esquema XDM y conceptos relacionados, como grupos de campos, tipos, clases y tipos de datos. Proporciona una comprensión completa del modelo de datos XDM y cómo crear y administrar esquemas XDM para estandarizar los datos en la empresa. Explórela para comprender mejor el esquema XDM y cómo puede beneficiar los procesos de recopilación y administración de datos.

## Crear secuencia de datos: Experience Platform

Un flujo de datos indica al Edge Network de Platform a dónde enviar los datos recopilados. Por ejemplo, se puede enviar a Experience Platform, Analytics o Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Familiarícese con el concepto de flujos de datos y temas relacionados, como el control y la configuración de datos, en la página de [Información general de flujos de datos](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html).

## Crear propiedad de etiqueta: Experience Platform

Obtenga información sobre cómo crear una propiedad de etiqueta en Experience Platform para agregar la biblioteca JavaScript del SDK web al sitio web de WKND. La propiedad de etiquetas recién definida tiene los siguientes recursos:

+ Extensiones de etiquetas: [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) y [SDK web de Adobe Experience Platform](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementos de datos: elementos de datos de tipo de código personalizado que extraen nombre de página, sección de sitio y nombre de host mediante la capa de datos del cliente de Adobe del sitio WKND. Además, el elemento de datos de tipo de objeto XDM que cumple con la incorporación del esquema XDM WKND recién creado anteriormente en el paso [Crear esquema XDM](#create-xdm-schema---experience-platform).
+ Regla: enviar datos al Edge Network de Platform cada vez que se visita una página web de WKND mediante el evento `cmp:show` activado por la capa de datos del cliente de Adobe.

Al generar y publicar la biblioteca de etiquetas con **Flujo de publicación**, puede usar el botón **Agregar todos los recursos modificados**. Para seleccionar todos los recursos, como Data Element, Rule y Tag Extensions, en lugar de identificar y seleccionar un recurso individual. Además, durante la fase de desarrollo, puede publicar la biblioteca solo en el entorno _Development_ y luego verificarla y promocionarla al entorno _Stage_ o _Production_.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>El elemento de datos y el código de evento de regla que se muestran en el vídeo están disponibles para su referencia, **expanda el elemento de acordeón siguiente**. Sin embargo, si NO utiliza la capa de datos del cliente de Adobe, debe modificar el siguiente código, pero se sigue aplicando el concepto de definir los elementos de datos y utilizarlos en la definición de regla.


+++ Elemento de datos y código de regla-evento

+ El código del elemento de datos `Page Name`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ El código del elemento de datos `Site Section`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('repo:path')) {
  let pagePath = event.component['repo:path'];
  
  let siteSection = '';
  
  //Check of html String in URL.
  if (pagePath.indexOf('.html') > -1) { 
   siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
  
   //replace slash with colon
   siteSection = siteSection.replaceAll('/', ':');
  
   //remove `:content`
   siteSection = siteSection.replaceAll(':content:','');
  }
  
      return siteSection 
  }
  ```

+ El código del elemento de datos `Host Name`.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ El código de evento de regla `all pages - on load`

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      // trigger tags Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          // include the path of the component that triggered the event
          path: evt.eventInfo.path,
          // get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      // Trigger the tags Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other tags data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  // set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  // push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


La [descripción general de etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) proporciona información detallada sobre conceptos importantes, como elementos de datos, reglas y extensiones.

AEM Para obtener información adicional sobre la integración de componentes principales de con la capa de datos del cliente de Adobe, consulte la [Guía de uso de la capa de datos del cliente de Adobe AEM con componentes principales](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es).

## AEM Conectar la propiedad de etiqueta a la

AEM Descubra cómo vincular la propiedad de etiquetas creada recientemente a las etiquetas a través de Adobe IMS y a las etiquetas en Configuración de Adobe Experience Platform AEM en. Cuando se establece un entorno de AEM as a Cloud Service, se generan automáticamente varias configuraciones de cuenta técnica de IMS de Adobe, incluidas las etiquetas. AEM Sin embargo, para la versión 6.5 de la aplicación, debe configurar una manualmente.

Después de vincular la propiedad de etiqueta, el sitio WKND puede cargar la biblioteca de JavaScript de la propiedad de etiqueta en las páginas web mediante las etiquetas de la configuración del servicio en la nube de Adobe Experience Platform.

### Verificar la propiedad de etiqueta al cargarse en WKND

Con la extensión de Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob), compruebe si la propiedad de etiqueta se está cargando en páginas WKND. Puede verificar lo siguiente:

+ Detalles de la propiedad de etiquetas, como extensión, versión, nombre y más.
+ Versión de biblioteca del SDK web de Platform, ID de flujo de datos
+ Objeto XDM como parte `events` atributo en el SDK web de Experience Platform

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Crear conjunto de datos: Experience Platform

Los datos de vista de página recopilados mediante el SDK web se almacenan en el lago de datos del Experience Platform como conjuntos de datos. El conjunto de datos es una construcción de almacenamiento y administración para una colección de datos, como una tabla de base de datos, que sigue un esquema. Obtenga información sobre cómo crear un conjunto de datos y configurar la secuencia de datos creada anteriormente para enviar datos al Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

La [descripción general de conjuntos de datos](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) proporciona más información sobre conceptos, configuraciones y otras capacidades de ingesta.


## Datos de vista de página WKND en el Experience Platform

AEM Después de configurar el SDK web con el SDK de, sobre todo en el sitio WKND, es hora de generar tráfico navegando por las páginas del sitio. A continuación, confirme que los datos de la vista de página se están introduciendo en el conjunto de datos del Experience Platform. En la interfaz de usuario del conjunto de datos, se muestran varios detalles, como los registros totales, el tamaño y los lotes ingeridos, junto con un gráfico de barras visualmente atractivo.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Resumen

¡Buen trabajo! AEM Ha completado la configuración de la configuración de con el SDK web de Experience Platform para recopilar e introducir datos de un sitio web. Con esta base, ahora puede explorar más posibilidades para mejorar e integrar productos como Analytics, Target, Customer Journey Analytics (CJA) y muchos otros para crear experiencias enriquecidas y personalizadas para sus clientes. Siga aprendiendo y explorando para liberar todo el potencial de Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Si prefiere el **vídeo completo** que cubre todo el proceso de integración en lugar de los vídeos de pasos de configuración individuales, puede hacer clic [aquí](https://video.tv.adobe.com/v/3418905/) para acceder a él.

## Recursos adicionales

+ [Uso de la capa de datos del cliente de Adobe con los componentes principales](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es)
+ [Integración de etiquetas de recopilación de datos de Experience Platform AEM y el uso de etiquetas de](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/experience-platform-data-collection-tags/overview.html?lang=es)
+ [Información general sobre el SDK web de Adobe Experience Platform y el Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriales de recopilación de datos](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [descripción general del Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

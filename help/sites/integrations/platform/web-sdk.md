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
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '1354'
ht-degree: 4%

---

# Integración de AEM Sites y SDK web de Experience Platform

AEM Obtenga información sobre cómo integrar el as a Cloud Service de la con Experience Platform [SDK web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Este paso fundamental es esencial para integrar productos de Adobe Experience Cloud, como Adobe Analytics, Target o productos innovadores recientes como Real-time Customer Data Platform, Customer Journey Analytics y Journey Optimizer.

También aprenderá a recopilar y enviar [WKND: proyecto de Adobe Experience Manager de muestra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) datos de vista de página en [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Después de completar esta configuración, ha implementado una base sólida. Además, está listo para avanzar en la implementación de Experience Platform mediante aplicaciones como [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=es), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), y [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). La implementación avanzada ayuda a mejorar la participación de los clientes mediante la estandarización de la web y los datos de los clientes.

## Requisitos previos

Se requiere lo siguiente al integrar el SDK web de Experience Platform.

Entrada **AEM como Cloud Service**:

+ AEM AEM Acceso de administrador de a un entorno as a Cloud Service
+ Acceso del administrador de implementación a Cloud Manager
+ Clonar e implementar el [WKND: proyecto de Adobe Experience Manager de muestra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM a su entorno as a Cloud Service de.

Entrada **Experience Platform**:

+ Acceso a la producción predeterminada, **Prod** zona protegida.
+ Acceso a **Esquemas** en Administración de datos
+ Acceso a **Conjuntos de datos** en Administración de datos
+ Acceso a **Datastreams** en Recopilación de datos
+ Acceso a **Etiquetas** (anteriormente conocido como Launch) en Recopilación de datos

Si no tiene los permisos necesarios, el administrador del sistema debe utilizar [Adobe Admin Console](https://adminconsole.adobe.com/) puede conceder los permisos necesarios.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Crear esquema XDM: Experience Platform

El esquema del modelo de datos de experiencia (XDM) le ayuda a estandarizar los datos de experiencia del cliente. Para recopilar los **Vista de página WKND** , cree un esquema XDM y utilice el Adobe proporcionado por los grupos de campos `AEP Web SDK ExperienceEvent` para la recopilación de datos web.

Existen modelos genéricos y de sectores específicos como, por ejemplo, minoristas, servicios financieros, sanidad y mucho más, conjuntos de modelos de datos de referencia, consulte [Resumen de modelos de datos del sector](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) para obtener más información.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Obtenga información sobre el esquema XDM y conceptos relacionados, como grupos de campos, tipos, clases y tipos de datos de [Información general del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

El [Información general del sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) es un excelente recurso para obtener información sobre el esquema XDM y conceptos relacionados, como grupos de campos, tipos, clases y tipos de datos. Proporciona una comprensión completa del modelo de datos XDM y cómo crear y administrar esquemas XDM para estandarizar los datos en la empresa. Explórela para comprender mejor el esquema XDM y cómo puede beneficiar los procesos de recopilación y administración de datos.

## Crear secuencia de datos: Experience Platform

Un flujo de datos indica a Platform Edge Network dónde enviar los datos recopilados. Por ejemplo, se puede enviar a Experience Platform, Analytics o Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Familiarícese con el concepto de flujos de datos y temas relacionados, como la gobernanza y configuración de datos, visitando la [Resumen de flujos de datos](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) página.

## Crear propiedad de etiqueta: Experience Platform

Obtenga información sobre cómo crear una propiedad de etiquetas (anteriormente conocida como Launch) en Experience Platform para agregar la biblioteca JavaScript del SDK web al sitio web de WKND. La propiedad de etiquetas recién definida tiene los siguientes recursos:

+ Extensiones de etiquetas: [Núcleo](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) y [SDK web de Adobe Experience Platform](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementos de datos: elementos de datos de tipo de código personalizado que extraen nombre de página, sección de sitio y nombre de host mediante la capa de datos del cliente de Adobe del sitio WKND. Además, el elemento de datos de tipo Objeto XDM que cumple con la incorporación del esquema XDM WKND recién creado anteriormente [Crear esquema XDM](#create-xdm-schema---experience-platform) paso.
+ Regla: enviar datos a Platform Edge Network cada vez que se visita una página web de WKND con la capa de datos del cliente de Adobe activada `cmp:show` evento.

Al crear y publicar la biblioteca de etiquetas utilizando **Flujo de publicación**, puede utilizar el **Añadir todos los recursos modificados** botón. Para seleccionar todos los recursos, como Data Element, Rule y Tag Extensions, en lugar de identificar y seleccionar un recurso individual. Además, durante la fase de desarrollo, puede publicar la biblioteca solo en _Desarrollo_ entorno, y luego verificarlo y promocionarlo en el _Fase_ o _Producción_ entorno.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>El elemento de datos y el código de evento de regla que se muestran en el vídeo están disponibles para su referencia, **expanda el elemento de acordeón siguiente**. Sin embargo, si NO utiliza la capa de datos del cliente de Adobe, debe modificar el siguiente código, pero se sigue aplicando el concepto de definir los elementos de datos y utilizarlos en la definición de regla.


+++ Elemento de datos y código de regla-evento

+ El `Page Name` Código de elemento de datos.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ El `Site Section` Código de elemento de datos.

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

+ El `Host Name` Código de elemento de datos.

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ El `all pages - on load` Código de evento de regla

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Launch Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Launch Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  //push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


El [Información general sobre etiquetas](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) proporciona conocimientos profundos sobre conceptos importantes como elementos de datos, reglas y extensiones.

AEM Para obtener información adicional sobre la integración de componentes principales de la interfaz de usuario de con la capa de datos del cliente de Adobe, consulte la [Uso de la capa de datos del cliente de Adobe AEM con la guía de componentes principales de](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es).

## AEM Conectar la propiedad de etiqueta a la

AEM Descubra cómo vincular la propiedad de etiquetas creada recientemente a las etiquetas a través de la configuración de Adobe IMS y Adobe AEM Launch en la. AEM Cuando se establece un entorno as a Cloud Service de la aplicación, se generan automáticamente varias configuraciones de cuenta técnica de IMS de Adobe, incluido Adobe Launch. AEM Sin embargo, para la versión 6.5 de la aplicación, debe configurar una manualmente.

Después de vincular la propiedad de etiqueta, el sitio WKND puede cargar la biblioteca JavaScript de la propiedad de etiqueta en las páginas web mediante la configuración del servicio en la nube de Adobe Launch.

### Verificar la propiedad de etiqueta al cargarse en WKND

Uso del Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) o [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) extensión, compruebe si la propiedad de etiqueta se está cargando en las páginas WKND. Puede verificar lo siguiente:

+ Detalles de la propiedad de etiquetas, como extensión, versión, nombre y más.
+ Versión de biblioteca del SDK web de Platform, ID de flujo de datos
+ Objeto XDM como parte `events` atributo en el SDK web de Experience Platform

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Crear conjunto de datos: Experience Platform

Los datos de vista de página recopilados mediante el SDK web se almacenan en el lago de datos del Experience Platform como conjuntos de datos. El conjunto de datos es una construcción de almacenamiento y administración para una colección de datos, como una tabla de base de datos, que sigue un esquema. Obtenga información sobre cómo crear un conjunto de datos y configurar la secuencia de datos creada anteriormente para enviar datos al Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

El [Información general sobre conjuntos de datos](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) proporciona más información sobre conceptos, configuraciones y otras capacidades de ingesta.


## Datos de vista de página WKND en el Experience Platform

AEM Después de configurar el SDK web con el SDK de, sobre todo en el sitio WKND, es hora de generar tráfico navegando por las páginas del sitio. A continuación, confirme que los datos de la vista de página se están introduciendo en el conjunto de datos del Experience Platform. En la interfaz de usuario del conjunto de datos, se muestran varios detalles, como los registros totales, el tamaño y los lotes ingeridos, junto con un gráfico de barras visualmente atractivo.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Resumen

¡Buen trabajo! AEM Ha completado la configuración de la configuración de con el SDK web de Experience Platform para recopilar e introducir datos de un sitio web. Con esta base, ahora puede explorar más posibilidades para mejorar e integrar productos como Analytics, Target, Customer Journey Analytics (CJA) y muchos otros para crear experiencias enriquecidas y personalizadas para sus clientes. Siga aprendiendo y explorando para liberar todo el potencial de Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Si prefiere el **vídeo de extremo a extremo** que abarca todo el proceso de integración en lugar de los vídeos de los pasos de configuración individuales, puede hacer clic en [aquí](https://video.tv.adobe.com/v/3418905/) para acceder a él.

## Recursos adicionales

+ [Uso de la capa de datos del cliente de Adobe con los componentes principales](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es)
+ [Integración de etiquetas y etiquetas de recopilación de datos de Experience PlatformAEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/experience-platform-data-collection-tags/overview.html?lang=es)
+ [Información general sobre Adobe Experience Platform Web SDK y Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriales de recopilación de datos](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [información general de Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

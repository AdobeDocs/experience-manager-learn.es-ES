---
title: Integración de AEM Sites y Adobe Analytics con Platform Web SDK
description: Integre AEM Sites y Adobe Analytics con el enfoque moderno de Platform Web SDK.
version: Experience Manager as a Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2252
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 1%

---

# Integración de AEM Sites y Adobe Analytics con Platform Web SDK

Conozca el **enfoque moderno** sobre cómo integrar Adobe Experience Manager (AEM) y Adobe Analytics mediante Platform Web SDK. Este completo tutorial lo guiará a través del proceso de recopilar sin problemas [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) vista de página y datos de clics de CTA. Obtenga valiosas perspectivas visualizando los datos recopilados en Adobe Analysis Workspace, donde puede explorar varias métricas y dimensiones. Además, explore el conjunto de datos de Platform para comprobar y analizar los datos. Únase a nosotros en este recorrido para aprovechar el poder de AEM y Adobe Analytics para la toma de decisiones basada en datos.

## Información general

Obtener perspectivas sobre el comportamiento del usuario es un objetivo crucial para cada equipo de marketing. Al comprender cómo los usuarios interactúan con su contenido, los equipos pueden tomar decisiones informadas, optimizar estrategias e impulsar mejores resultados. El equipo de marketing de WKND, una entidad ficticia, ha puesto su mira en implementar Adobe Analytics en su sitio web para lograr este objetivo. El objetivo principal es recopilar datos en dos métricas clave: vistas de página y clics en la página principal de call-to-action (CTA).

Al realizar un seguimiento de las vistas de página, el equipo puede analizar qué páginas reciben la mayor atención de los usuarios. Además, el seguimiento de los clics en CTA en la página de inicio proporciona información valiosa sobre la eficacia de los elementos call-to-action del equipo. Estos datos pueden revelar qué CTA resuenan con los usuarios, cuáles necesitan ajustes y potencialmente descubrir nuevas oportunidades para mejorar la participación del usuario y dirigir las conversiones.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Requisitos previos

Se requiere lo siguiente al integrar Adobe Analytics mediante Platform Web SDK.

Ha completado los pasos de configuración del tutorial **[Integrar Experience Platform Web SDK](./web-sdk.md)**.

En **AEM as Cloud Service**:

+ [Acceso de administrador de AEM al entorno de AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=es)
+ Acceso del administrador de implementación a Cloud Manager
+ Clonar e implementar [WKND: proyecto de Adobe Experience Manager de muestra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) en el entorno de AEM as a Cloud Service.

En **Adobe Analytics**:

+ Acceso para crear **grupo de informes**
+ Acceso para crear **Analysis Workspace**

En **Experience Platform**:

+ Acceso a la zona protegida **Prod** de producción predeterminada.
+ Acceso a **Esquemas** en Administración de datos
+ Acceso a **Conjuntos de datos** en Administración de datos
+ Acceso a **Datastreams** en Recopilación de datos
+ Acceso a **Etiquetas** en Recopilación de datos

Si no cuenta con los permisos necesarios, el administrador del sistema que use [Adobe Admin Console](https://adminconsole.adobe.com/) podrá concederle los permisos necesarios.

Antes de profundizar en el proceso de integración de AEM y Analytics mediante Platform Web SDK, _recapitulemos los componentes esenciales y los elementos clave_ que se establecieron en el tutorial [Integrar Experience Platform Web SDK](./web-sdk.md). Proporciona una base sólida para la integración.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Después del resumen de las conexiones de Esquema XDM, Flujo de datos, Conjunto de datos, Propiedad de etiqueta y Propiedad de etiqueta, AEM y Propiedad de etiqueta, vamos a embarcarnos en el recorrido de integración.

## Definir el documento Diseño de referencia de la solución de Analytics (SDR)

Como parte del proceso de implementación, se recomienda crear un documento Diseño de referencia de la solución (DRS). Este documento desempeña un papel crucial como modelo para definir los requisitos empresariales y diseñar estrategias eficaces de recopilación de datos.

El documento de DEG ofrece una visión general del plan de implementación, asegurando que todas las partes interesadas estén alineadas y comprendan los objetivos y el alcance del proyecto.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Para obtener más información sobre los conceptos y los diversos elementos que deben incluirse en el documento de SDR, visite el [Documento de referencia de diseño de la solución (SDR)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). También puede descargar una plantilla de Excel de ejemplo, aunque la versión específica de WKND también está disponible [aquí](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Configuración de Analytics: grupo de informes, Analysis Workspace

El primer paso es configurar Adobe Analytics, específicamente un grupo de informes con variables de conversión (o eVar) y eventos de éxito. Las variables de conversión se utilizan para medir causa y efecto. Los eventos de éxito se utilizan para rastrear acciones.

En este tutorial, `eVar5, eVar6, and eVar7` rastreará _WKND Page Name, WKND CTA ID y WKND CTA Name_ respectivamente, y `event7` se utilizará para rastrear _WKND CTA Click Event_.

Para analizar, recopilar perspectivas y compartirlas con otros a partir de los datos recopilados, se crea un proyecto en Analysis Workspace.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Para obtener más información sobre la configuración y los conceptos de Analytics, se recomiendan los siguientes recursos:

+ [Grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html)
+ [Variables de conversión](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [Eventos de éxito](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## Actualizar secuencia de datos - Agregar servicio de Analytics

Un flujo de datos indica a Platform Edge Network dónde enviar los datos recopilados. En el [tutorial anterior](./web-sdk.md), se ha configurado un conjunto de datos para enviar los datos a Experience Platform. Este conjunto de datos se ha actualizado para enviar los datos al grupo de informes de Analytics que se configuró en el paso [anterior](#setup-analytics---report-suite-analysis-workspace).

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Crear esquema XDM

El esquema del modelo de datos de experiencia (XDM) le ayuda a estandarizar los datos recopilados. En el [tutorial anterior](./web-sdk.md), se crea un esquema XDM con `AEP Web SDK ExperienceEvent` un grupo de campos. Además, con este esquema XDM, se crea un conjunto de datos para almacenar los datos recopilados en Experience Platform.

Sin embargo, ese esquema XDM no tiene grupos de campos específicos de Adobe Analytics para enviar los datos de evento de eVar. Se crea un nuevo esquema XDM en lugar de actualizar el esquema existente para evitar almacenar los datos de evento de eVar en la plataforma.

El esquema XDM recién creado tiene `AEP Web SDK ExperienceEvent` y `Adobe Analytics ExperienceEvent Full Extension` grupos de campos.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Actualizar propiedad de etiqueta

En el [tutorial anterior](./web-sdk.md), se crea una propiedad de etiqueta, que tiene elementos de datos y una regla para recopilar, asignar y enviar los datos de vista de página. Debe mejorarse para lo siguiente:

+ Asignando el nombre de página a `eVar5`
+ Activando la llamada de **pageview** Analytics ( o enviar señalización)
+ Recopilación de datos de CTA mediante la capa de datos del cliente de Adobe
+ Asignando el nombre y el ID de CTA a `eVar6` y `eVar7` respectivamente. Además, el recuento de clics de CTA de `event7`
+ Activando la llamada a **link click** Analytics ( o enviar señalización)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>El elemento de datos y el código de evento de regla que se muestran en el vídeo están disponibles para su referencia, **expanda el elemento de acordeón siguiente**. Sin embargo, si NO utiliza la capa de datos del cliente de Adobe, debe modificar el siguiente código, pero se sigue aplicando el concepto de definir los elementos de datos y utilizarlos en la definición de regla.

+++ Elemento de datos y código de regla-evento

+ El código del elemento de datos `Component ID`.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ El código del elemento de datos `Component Name`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ El código `all pages - on load` **Rule-Condition**

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ El código `home page - cta click` **Rule-Event**

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ El código `home page - cta click` **Rule-Condition**

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

Para obtener información adicional sobre la integración de los componentes principales de AEM con la capa de datos del cliente de Adobe, consulte la guía [Uso de la capa de datos del cliente de Adobe con los componentes principales de AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es).


>[!INFO]
>
>Para obtener información detallada sobre los detalles de la propiedad de la pestaña **Mapa de variables** en el documento Referencia de diseño de la solución (SDR), acceda a la versión completa específica de WKND para descargar [aquí](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## Verificar la propiedad de etiquetas actualizada en WKND

Para asegurarse de que la propiedad de etiqueta actualizada se crea, publica y funciona correctamente en las páginas del sitio WKND. Utilice la [extensión de Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) del explorador web Google Chrome:

+ Para asegurarse de que la propiedad de etiqueta es la última versión, compruebe la fecha de compilación.

+ Para comprobar los datos de evento XDM tanto para PageView como para HomePage CTA Click, utilice la opción de menú Experience Platform Web SDK en la extensión.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Simular tráfico web: automatización de Selenium

Para generar una cantidad significativa de tráfico con fines de prueba, se desarrolla un script de automatización de Selenium. Este script personalizado simula las interacciones del usuario con el sitio web de WKND, como la vista de página y hacer clic en las CTA.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Verificación del conjunto de datos: vista de página WKND, datos de CTA

El conjunto de datos es una construcción de almacenamiento y administración para una colección de datos, como una tabla de base de datos, que sigue un esquema. El conjunto de datos creado en el [tutorial anterior](./web-sdk.md) se reutiliza para comprobar que los datos de vista de página y clics de CTA se han introducido en el conjunto de datos de Experience Platform. En la interfaz de usuario del conjunto de datos, se muestran varios detalles, como los registros totales, el tamaño y los lotes ingeridos, junto con un gráfico de barras visualmente atractivo.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics: vista de página WKND, visualización de datos CTA

Analysis Workspace es una potente herramienta dentro de Adobe Analytics que permite explorar y visualizar datos de una manera flexible e interactiva. Proporciona una interfaz de arrastrar y soltar para crear informes personalizados, realizar una segmentación avanzada y aplicar varias visualizaciones de datos.

Volvamos a abrir el proyecto de Analysis Workspace creado en el paso [Análisis de instalación](#setup-analytics---report-suite-analysis-workspace). En la sección **Páginas principales**, examine diversas métricas, como visitas, visitantes únicos, entradas, tasa de salida hacia otro sitio y más. Para evaluar el rendimiento de las páginas de WKND y de las CTA de la página de inicio, arrastre y suelte las dimensiones específicas de WKND (Nombre de página de WKND, Nombre de CTA de WKND) y las métricas (Evento de clic de CTA de WKND). Estas perspectivas son valiosas para los especialistas en marketing, a fin de comprender qué CTA son más eficaces y tomar decisiones basadas en datos, alineadas con sus objetivos comerciales.

Para visualizar los recorridos del usuario, utilice la visualización de flujo, empezando por **WKND Page Name** y expandiéndose en varias rutas.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Resumen

¡Buen trabajo! Ha completado la configuración de AEM y Adobe Analytics mediante Platform Web SDK para recopilar, analizar la vista de página y hacer clic en datos de CTA.

La implementación de Adobe Analytics es crucial para que los equipos de marketing obtengan perspectivas sobre el comportamiento del usuario, tomen decisiones informadas, permitiéndoles optimizar su contenido y tomar decisiones basadas en datos.

Al implementar los pasos recomendados y utilizar los recursos proporcionados, como el documento Referencia de diseño de la solución (SDR) y comprender los conceptos clave de Analytics, los especialistas en marketing pueden recopilar y analizar datos de forma eficaz.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Si prefiere el **vídeo completo** que cubre todo el proceso de integración en lugar de los vídeos de pasos de configuración individuales, puede hacer clic [aquí](https://video.tv.adobe.com/v/3419889/) para acceder a él.


## Recursos adicionales

+ [Integrar Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [Uso de la capa de datos del cliente de Adobe con los componentes principales](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es)
+ [Integración de etiquetas de recopilación de datos de Experience Platform y AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/experience-platform-data-collection-tags/overview.html?lang=es)
+ [Información general sobre Adobe Experience Platform Web SDK y Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriales de recopilación de datos](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Información general de Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

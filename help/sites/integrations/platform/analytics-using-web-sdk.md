---
title: Integración de AEM Sites y Adobe Analytics con el SDK web de Platform
description: Integre AEM Sites y Adobe Analytics con el enfoque moderno del SDK web de Platform.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '1637'
ht-degree: 3%

---

# Integración de AEM Sites y Adobe Analytics con el SDK web de Platform

Conozca las **enfoque moderno** Obtenga información sobre cómo integrar Adobe Experience Manager AEM () y Adobe Analytics mediante el SDK web de Platform. Este completo tutorial le guía a través del proceso de recopilación sin problemas [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) vista de página y datos de clics en CTA. Obtenga valiosas perspectivas visualizando los datos recopilados en Adobe Analysis Workspace, donde puede explorar varias métricas y dimensiones. Además, explore el conjunto de datos de Platform para comprobar y analizar los datos. Únase a nosotros en este recorrido AEM para aprovechar el poder de las soluciones de y de Adobe Analytics para la toma de decisiones basada en datos.

## Información general

Obtener perspectivas sobre el comportamiento del usuario es un objetivo crucial para cada equipo de marketing. Al comprender cómo los usuarios interactúan con su contenido, los equipos pueden tomar decisiones informadas, optimizar estrategias e impulsar mejores resultados. El equipo de marketing de WKND, una entidad ficticia, ha puesto su mira en implementar Adobe Analytics en su sitio web para lograr este objetivo. El objetivo principal es recopilar datos en dos métricas clave: vistas de página y clics en la llamada a la acción (CTA) de la página principal.

Al realizar un seguimiento de las vistas de página, el equipo puede analizar qué páginas reciben la mayor atención de los usuarios. Además, el seguimiento de los clics en CTA de la página de inicio proporciona información valiosa sobre la eficacia de los elementos de llamada a la acción del equipo. Estos datos pueden revelar qué CTA resuenan con los usuarios, cuáles necesitan ajustes y potencialmente descubrir nuevas oportunidades para mejorar la participación del usuario y dirigir las conversiones.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Requisitos previos

Se requiere lo siguiente al integrar Adobe Analytics mediante el SDK web de Platform.

Ha completado los pasos de configuración desde el **[Integración del SDK web de Experience Platform](./web-sdk.md)** tutorial.

Entrada **AEM como Cloud Service**:

+ [AEM AEM Acceso de administrador de a un entorno as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=es)
+ Acceso del administrador de implementación a Cloud Manager
+ Clonar e implementar el [WKND: proyecto de Adobe Experience Manager de muestra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM a su entorno as a Cloud Service de.

Entrada **Adobe Analytics**:

+ Acceso para crear **Grupo de informes**
+ Acceso para crear **Analysis Workspace**

Entrada **Experience Platform**:

+ Acceso a la producción predeterminada, **Prod** zona protegida.
+ Acceso a **Esquemas** en Administración de datos
+ Acceso a **Conjuntos de datos** en Administración de datos
+ Acceso a **Datastreams** en Recopilación de datos
+ Acceso a **Etiquetas** (anteriormente conocido como Launch) en Recopilación de datos

Si no tiene los permisos necesarios, el administrador del sistema debe utilizar [Adobe Admin Console](https://adminconsole.adobe.com/) puede conceder los permisos necesarios.

AEM Antes de profundizar en el proceso de integración de y Analytics mediante el SDK web de Platform, vamos a _recapitular los componentes esenciales y los elementos clave_ que se establecieron en la [Integración del SDK web de Experience Platform](./web-sdk.md) tutorial. Proporciona una base sólida para la integración.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

AEM Después del resumen de la conexión de las propiedades Esquema XDM, Flujo de datos, Conjunto de datos, Etiqueta, Propiedad y Etiquetas, la propiedad de etiquetas y la propiedad de etiquetas, vamos a embarcarnos en el recorrido de la integración.

## Definir el documento Diseño de referencia de la solución de Analytics (SDR)

Como parte del proceso de implementación, se recomienda crear un documento Diseño de referencia de la solución (DRS). Este documento desempeña un papel crucial como modelo para definir los requisitos empresariales y diseñar estrategias eficaces de recopilación de datos.

El documento de DEG ofrece una visión general del plan de implementación, asegurando que todas las partes interesadas estén alineadas y comprendan los objetivos y el alcance del proyecto.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Para obtener más información sobre los conceptos y diversos elementos que deben incluirse en el documento de la SDR, visite la [Creación y mantenimiento de un documento Diseño de referencia de la solución (DRS)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). También puede descargar una plantilla de Excel de ejemplo, aunque también está disponible una versión específica de WKND [aquí](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Configuración de Analytics: grupo de informes, Analysis Workspace

El primer paso es configurar Adobe Analytics, específicamente un grupo de informes, con variables de conversión (o eVar) y eventos de éxito. Las variables de conversión se utilizan para medir causa y efecto. Los eventos de éxito se utilizan para rastrear acciones.

En este tutorial,  `eVar5, eVar6, and eVar7` track  _Nombre de página WKND, ID de CTA WKND y nombre de CTA WKND_ respectivamente, y `event7` se utiliza para realizar el seguimiento  _Evento de clic de CTA WKND_.

Para analizar, recopilar perspectivas y compartirlas con otros a partir de los datos recopilados, se crea un proyecto en Analysis Workspace.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Para obtener más información sobre la configuración y los conceptos de Analytics, se recomiendan los siguientes recursos:

+ [Grupo de informes](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html)
+ [Variables de conversión](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [Eventos de éxito](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## Actualizar secuencia de datos - Agregar servicio de Analytics

Un flujo de datos indica a Platform Edge Network dónde enviar los datos recopilados. En el [tutorial anterior](./web-sdk.md), se configura una secuencia de datos para enviar los datos al Experience Platform. Este conjunto de datos se actualiza para enviar los datos al grupo de informes de Analytics configurado en la variable [superior](#setup-analytics---report-suite-analysis-workspace) paso.

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Crear esquema XDM

El esquema del modelo de datos de experiencia (XDM) le ayuda a estandarizar los datos recopilados. En el [tutorial anterior](./web-sdk.md), un esquema XDM con `AEP Web SDK ExperienceEvent` se crea un grupo de campos. Además, con este esquema XDM, se crea un conjunto de datos para almacenar los datos recopilados en el Experience Platform.

Sin embargo, ese esquema XDM no tiene grupos de campos específicos de Adobe Analytics para enviar el eVar, datos de evento. Se crea un nuevo esquema XDM en lugar de actualizar el esquema existente para evitar almacenar el eVar y los datos de evento en la plataforma.

El esquema XDM recién creado tiene `AEP Web SDK ExperienceEvent` y `Adobe Analytics ExperienceEvent Full Extension` grupos de campos.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Actualizar propiedad de etiqueta

En el [tutorial anterior](./web-sdk.md)A continuación, se crea una propiedad de etiqueta, que tiene elementos de datos y una regla para recopilar, asignar y enviar los datos de la vista de página. Debe mejorarse para lo siguiente:

+ Asignación del nombre de página a `eVar5`
+ Activación de la **pageview** Llamada de Analytics ( o enviar señalización)
+ Recopilación de datos de CTA mediante la capa de datos del cliente de Adobe
+ Asignación del ID y el nombre de CTA a `eVar6` y `eVar7` respectivamente. Además, el recuento de clics de CTA de `event7`
+ Activación de la **clic en vínculo** Llamada de Analytics ( o enviar señalización)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>El elemento de datos y el código de evento de regla que se muestran en el vídeo están disponibles para su referencia, **expanda el elemento de acordeón siguiente**. Sin embargo, si NO utiliza la capa de datos del cliente de Adobe, debe modificar el siguiente código, pero se sigue aplicando el concepto de definir los elementos de datos y utilizarlos en la definición de regla.

+++ Elemento de datos y código de regla-evento

+ El `Component ID` Código de elemento de datos.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ El `Component Name` Código de elemento de datos.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ El `all pages - on load` **Regla-condición** código

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ El `home page - cta click` **Rule-Event** código

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

+ El `home page - cta click` **Regla-condición** código

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

AEM Para obtener información adicional sobre la integración de componentes principales de la interfaz de usuario de con la capa de datos del cliente de Adobe, consulte la [Uso de la capa de datos del cliente de Adobe AEM con la guía de componentes principales de](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es).


>[!INFO]
>
>Para una comprensión exhaustiva de la **Mapa de variables** para obtener información detallada sobre la propiedad en el documento Referencia de diseño de la solución (SDR), acceda a la versión completa específica de WKND para descargarla [aquí](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## Verificar la propiedad de etiquetas actualizada en WKND

Para asegurarse de que la propiedad de etiqueta actualizada se crea, publica y funciona correctamente en las páginas del sitio WKND. Utilice el explorador web de Google Chrome [extensión de Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob):

+ Para asegurarse de que la propiedad de etiqueta es la última versión, compruebe la fecha de compilación.

+ Para comprobar los datos de evento XDM tanto para PageView como para HomePage CTA Click, utilice la opción de menú SDK web de Experience Platform dentro de la extensión.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Simular tráfico web: automatización de Selenium

Para generar una cantidad significativa de tráfico con fines de prueba, se desarrolla un script de automatización de Selenium. Este script personalizado simula las interacciones del usuario con el sitio web de WKND, como la vista de página y hacer clic en las CTA.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Verificación del conjunto de datos: vista de página de WKND, datos de CTA

El conjunto de datos es una construcción de almacenamiento y administración para una colección de datos, como una tabla de base de datos, que sigue un esquema. El conjunto de datos creado en [tutorial anterior](./web-sdk.md) se reutiliza para comprobar que los datos de vista de página y clics de CTA se han introducido en el conjunto de datos de Experience Platform. En la interfaz de usuario del conjunto de datos, se muestran varios detalles, como los registros totales, el tamaño y los lotes ingeridos, junto con un gráfico de barras visualmente atractivo.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics: vista de página WKND, visualización de datos CTA

Analysis Workspace es una potente herramienta dentro de Adobe Analytics que permite explorar y visualizar datos de una manera flexible e interactiva. Proporciona una interfaz de arrastrar y soltar para crear informes personalizados, realizar una segmentación avanzada y aplicar varias visualizaciones de datos.

Volvamos a abrir el proyecto de Analysis Workspace creado en [Análisis de configuración](#setup-analytics---report-suite-analysis-workspace) paso. En el **Páginas principales** Examine varias métricas, como visitas, visitantes únicos, entradas, tasa de salida hacia otro sitio, etc. Para evaluar el rendimiento de las páginas de WKND y de las CTA de la página principal, arrastre y suelte las dimensiones (nombre de página de WKND, nombre de CTA de WKND) y métricas (evento de clic de CTA de WKND) específicas. Estas perspectivas son valiosas para los especialistas en marketing, a fin de comprender qué CTA son más eficaces y tomar decisiones basadas en datos, alineadas con sus objetivos comerciales.

Para visualizar los recorridos del usuario, utilice la Visualización de flujo, empezando por **Nombre de página WKND** y expandiéndose en varias rutas.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Resumen

¡Buen trabajo! AEM Ha completado la configuración de y Adobe Analytics mediante el SDK web de Platform para recopilar, analizar la vista de página y hacer clic en datos de CTA.

La implementación de Adobe Analytics es crucial para que los equipos de marketing obtengan perspectivas sobre el comportamiento del usuario, tomen decisiones informadas, permitiéndoles optimizar su contenido y tomar decisiones basadas en datos.

Al implementar los pasos recomendados y utilizar los recursos proporcionados, como el documento Referencia de diseño de la solución (SDR) y comprender los conceptos clave de Analytics, los especialistas en marketing pueden recopilar y analizar datos de forma eficaz.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Si prefiere el **vídeo de extremo a extremo** que abarca todo el proceso de integración en lugar de los vídeos de los pasos de configuración individuales, puede hacer clic en [aquí](https://video.tv.adobe.com/v/3419889/) para acceder a él.


## Recursos adicionales

+ [Integración del SDK web de Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [Uso de la capa de datos del cliente de Adobe con los componentes principales](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=es)
+ [Integración de etiquetas y etiquetas de recopilación de datos de Experience PlatformAEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/experience-platform-data-collection-tags/overview.html?lang=es)
+ [Información general sobre Adobe Experience Platform Web SDK y Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriales de recopilación de datos](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [información general de Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

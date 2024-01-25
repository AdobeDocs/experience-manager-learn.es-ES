---
title: AEM Integración de Target y sin encabezado de
description: AEM Aprenda a integrar sin encabezado y Adobe Target para personalizar experiencias sin encabezado mediante el SDK web de Experience Platform.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM sin encabezado as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1620
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 0%

---

# AEM Integración de Target y sin encabezado de

AEM Obtenga información sobre cómo integrar sin encabezado con Adobe Target AEM, mediante la exportación de fragmentos de contenido de a Adobe Target, y utilizarlos para personalizar experiencias sin encabezado mediante alloy.js del SDK web de Adobe Experience Platform. El [Aplicación React WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) se utiliza para explorar cómo se puede añadir a la experiencia una actividad de Target personalizada mediante Ofertas de fragmentos de contenido para promocionar una aventura WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

AEM El tutorial trata sobre los pasos necesarios para configurar la configuración de la aplicación y de Adobe Target:

1. [Crear la configuración de IMS de Adobe para Adobe Target](#adobe-ims-configuration) AEM en el
2. [Crear Cloud Service de Adobe Target](#adobe-target-cloud-service) AEM en el
3. [Aplicar el Cloud Service de Adobe Target a las carpetas de AEM Assets](#configure-asset-folders) AEM en el
4. [Permiso del Cloud Service de Adobe Target](#permission) en Adobe Admin Console
5. [Exportar fragmentos de contenido](#export-content-fragments) AEM de autor de la a destino
6. [Crear una actividad mediante ofertas de fragmentos de contenido](#activity) en Adobe Target
7. [Crear una secuencia de datos de Experience Platform](#datastream-id) en Experience Platform
8. [AEM Integración de la personalización en una aplicación sin encabezado basada en React](#code) mediante el SDK web de Adobe.

## Configuración de IMS de Adobe{#adobe-ims-configuration}

AEM Una configuración de IMS de Adobe que facilita la autenticación entre y Adobe Target

Revisar [la documentación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) para obtener instrucciones paso a paso sobre cómo crear una configuración de IMS de Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Cloud Service de Adobe Target{#adobe-target-cloud-service}

Se crea un Cloud Service AEM de Adobe Target en la para facilitar la exportación de fragmentos de contenido a Adobe Target.

Revisar [la documentación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) para obtener instrucciones paso a paso sobre cómo crear un Cloud Service de Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configuración de carpetas de recursos{#configure-asset-folders}

El Cloud Service de Adobe Target, configurado en una configuración según el contexto, debe aplicarse a la jerarquía de carpetas de AEM Assets que contiene los fragmentos de contenido para exportar a Adobe Target.

+++Amplíe para obtener instrucciones paso a paso

1. Iniciar sesión en __AEM Servicio de autor de__ como administrador de DAM
1. Vaya a __Recursos > Archivos__, busque la carpeta de recursos que tenga el `/conf` aplicado a
1. Seleccione la carpeta de recursos y seleccione __Propiedades__ desde la barra de acciones superior
1. Seleccione el __Cloud Service__ pestaña
1. Asegúrese de que la Configuración de nube está establecida en la configuración según el contexto (`/conf`) que contiene la configuración de Cloud Service de Adobe Target.
1. Seleccionar __Adobe Target__ desde el __Configuraciones del Cloud Service__ desplegable.
1. Seleccionar __Guardar y cerrar__ en la parte superior derecha

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## AEM Permiso para la integración con Target de la{#permission}

La integración de Adobe Target, que se manifiesta como un proyecto developer.adobe.com, debe tener el __Editor__ función de producto en Adobe Admin Console para exportar fragmentos de contenido a Adobe Target.

+++Amplíe para obtener instrucciones paso a paso

1. Inicie sesión en el Experience Cloud como usuario que puede administrar el producto Adobe Target en Adobe Admin Console
1. Abra el [Adobe Admin Console](https://adminconsole.adobe.com)
1. Seleccionar __Productos__ y luego abrir __Adobe Target__
1. En el __Perfiles de producto__ pestaña, seleccione __*DefaultWorkspace*__
1. Seleccione el __Credenciales de API__ pestaña
1. Busque su aplicación developer.adobe.com en esta lista y establezca su __Función del producto__ hasta __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Exportar fragmentos de contenido a Target{#export-content-fragments}

Fragmentos de contenido que existen en [jerarquía de carpetas de AEM Assets configurada](#apply-adobe-target-cloud-service-to-aem-assets-folders) se puede exportar a Adobe Target como Ofertas de fragmentos de contenido. Estas ofertas de fragmentos de contenido, una forma especial de ofertas JSON en Target, se pueden utilizar en actividades de Target para ofrecer experiencias personalizadas en aplicaciones sin encabezado.

+++Amplíe para obtener instrucciones paso a paso

1. Iniciar sesión en __AEM Autor de__ como usuario DAM
1. Vaya a __Recursos > Archivos__ y busque Fragmentos de contenido para exportar como JSON a Target en la carpeta &quot;Adobe Target habilitado&quot;
1. Seleccione los fragmentos de contenido que desea exportar a Adobe Target
1. Seleccionar __Exportar a ofertas de Adobe Target__ desde la barra de acciones superior
   + Esta acción exporta la representación JSON completamente hidratada del fragmento de contenido a Adobe Target como una &quot;Oferta de fragmento de contenido&quot;
   + AEM La representación de JSON completamente hidratada se puede revisar en la sección de
      + Seleccione el fragmento de contenido
      + Expandir el panel lateral
      + Seleccionar __Previsualizar__ en el panel lateral izquierdo
      + La representación JSON exportada a Adobe Target se muestra en la vista principal
1. Iniciar sesión en [Adobe Experience Cloud](https://experience.adobe.com) con un usuario en la función Editor para Adobe Target
1. Desde el [Experience Cloud](https://experience.adobe.com), seleccione __Target__ en el conmutador de productos, en la parte superior derecha, para abrir Adobe Target.
1. Asegúrese de que la opción Espacio de trabajo predeterminado está seleccionada en __Conmutador de Workspace__ en la parte superior derecha.
1. Seleccione el __Ofertas__ en la barra de navegación superior
1. Seleccione el __Tipo__ y seleccionar. __Fragmentos de contenido__
1. AEM Compruebe que el fragmento de contenido exportado desde el que aparece la lista aparece en la lista.
   + Pase el ratón sobre la oferta y seleccione __Ver__ botón
   + Revise la __Información de oferta__ y vea el __AEM vínculo profundo de la__ AEM que abre el fragmento de contenido directamente en el servicio de autor de

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Actividad de Target que utiliza ofertas de fragmentos de contenido{#activity}

En Adobe Target AEM, se puede crear una actividad que utilice la oferta de fragmento de contenido JSON como contenido, lo que permite experiencias personalizadas en una aplicación sin encabezado con contenido creado y administrado en la.

En este ejemplo, utilizamos una actividad A/B simple, aunque se puede utilizar cualquier actividad de Target.

+++Amplíe para obtener instrucciones paso a paso

1. Seleccione el __Actividades__ en la barra de navegación superior
1. Seleccionar __+ Crear actividad__ y, a continuación, seleccione el tipo de actividad que desea crear.
   + En este ejemplo se crea un __Prueba A/B__ pero las ofertas de fragmentos de contenido pueden potenciar cualquier tipo de actividad
1. En el __Crear actividad__ asistente
   + Seleccionar __Web__
   + Entrada __Elegir Compositor de experiencias__, seleccione __Form__
   + Entrada __Elija Workspace__, seleccione __Workspace predeterminado__
   + Entrada __Elegir propiedad__, seleccione la propiedad en la que está disponible la actividad o seleccione __Sin restricciones de propiedad__ para permitir que se utilice en todas las propiedades.
   + Seleccionar __Siguiente__ para crear la actividad
1. Cambie el nombre de la actividad seleccionando __renombrar__ en la parte superior izquierda
   + Dé un nombre significativo a la actividad
1. En la experiencia inicial, establezca __Ubicación 1__ para que la actividad se dirija
   + En este ejemplo, establezca como objetivo una ubicación personalizada denominada `wknd-adventure-promo`
1. En __Contenido__ seleccione el contenido predeterminado y seleccione __Cambiar fragmento de contenido__
1. Seleccione el fragmento de contenido exportado que servirá para esta experiencia y seleccione __Listo__
1. AEM Revise el JSON de oferta de fragmento de contenido en el área de texto Contenido; se trata del mismo JSON disponible en el servicio de autor a través de la acción de previsualización de fragmento de contenido.
1. En el carril izquierdo, añada una experiencia y seleccione una oferta de fragmento de contenido diferente para ofrecer
1. Seleccionar __Siguiente__ y configure las reglas de segmentación según sea necesario para la actividad
   + En este ejemplo, deje la prueba A/B como una división manual al 50/50.
1. Seleccionar __Siguiente__ y complete la configuración de la actividad
1. Seleccionar __Guardar y cerrar__ y ponerle un nombre significativo
1. En Actividad en Adobe Target, seleccione __Activar__ en el menú desplegable Inactivo/Activar/Archivar en la parte superior derecha.

La actividad de Adobe Target dirigida a `wknd-adventure-promo` AEM Ahora, esta ubicación se puede integrar y exponer en una aplicación sin encabezado de la aplicación de.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## ID de flujo de datos de Experience Platform{#datastream-id}

Un [Flujo de datos Adobe Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) AEM Se requiere un ID para que las aplicaciones sin encabezado de la interactúen con Adobe Target mediante [SDK web de Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Amplíe para obtener instrucciones paso a paso

1. Vaya a [Adobe Experience Cloud](https://experience.adobe.com/)
1. Abrir __Experience Platform__
1. Seleccionar __Recopilación de datos > Flujos de datos__ y seleccione __Nueva secuencia de datos__
1. En el asistente Nuevo flujo de datos, introduzca:
   + Nombre: `AEM Target integration`
   + Descripción: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Esquema de evento: `Leave blank`
1. Seleccionar __Guardar__
1. Seleccionar __Añadir servicio__
1. Entrada __Servicio__ select __Adobe Target__
   + Habilitado: __Sí__
   + Token de propiedad: __Dejar en blanco__
   + ID de entorno de destino: __Dejar en blanco__
      + El entorno de destino se puede establecer en Adobe Target en __Administración > Hosts__.
   + Área de nombres de ID de terceros de destino: __Dejar en blanco__
1. Seleccionar __Guardar__
1. En el lado derecho, copie el __ID de flujo de datos__ para su uso en [SDK web de Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) llamada de configuración.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## AEM Adición de personalización a una aplicación sin encabezado de{#code}

Este tutorial explora la personalización de una aplicación React simple mediante Ofertas de fragmentos de contenido en Adobe Target a través de [SDK web de Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Este método se puede utilizar para personalizar cualquier experiencia web basada en JavaScript.

Las experiencias móviles de Android™ y iOS se pueden personalizar siguiendo patrones similares utilizando [SDK móvil de Adobe](https://developer.adobe.com/client-sdks/documentation/).

### Requisitos previos

+ Node.js 14
+ Git
+ [WKND compartido 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) AEM instalado en los servicios de autor y publicación de as a Cloud

### Configuración de

1. Descargue el código fuente de la aplicación React de ejemplo desde [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra el código base en `~/Code/aem-guides-wknd-graphql/personalization-tutorial` en su IDE favorito
1. AEM Actualice el host del servicio de al que desea conectarse la aplicación `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. AEM Ejecute la aplicación y asegúrese de que se conecte al servicio de configuración de la aplicación de la aplicación de. Desde la línea de comandos, ejecute:

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Instale el [SDK web de Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) como paquete NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   El SDK web se puede utilizar en el código para recuperar la oferta de fragmento de contenido JSON por ubicación de actividad.

   Al configurar el SDK web, se requieren dos ID:

   + `edgeConfigId` que es el [ID de flujo de datos](#datastream-id)
   + `orgId` AEM El ID de organización de Adobe as a Cloud Service/de destino que se puede encontrar en __Experience Cloud > Perfil > Información de la cuenta > ID de organización actual__

   Al invocar el SDK web, la ubicación de la actividad de Adobe Target (en nuestro ejemplo, `wknd-adventure-promo`) debe establecerse como el valor en `decisionScopes` matriz.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementación

1. Creación de un componente de React `AdobeTargetActivity.js` para que aparezcan actividades de Adobe Target.

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   El componente React de actividad de Adobe Target se invoca de la siguiente manera:

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Creación de un componente de React `AdventurePromo.js` para procesar la aventura que sirve Adobe Target JSON.

   Este componente de React toma el JSON completamente hidratado, que representa un fragmento de contenido de aventura y se muestra de forma promocional. Los componentes de React que muestran el JSON atendido desde Ofertas de fragmentos de contenido de Adobe Target pueden ser tan variados y complejos como sea necesario en función de los fragmentos de contenido que se exportan a Adobe Target.

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   Este componente React se invoca de la siguiente manera:

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. Añadir el componente AdobeTargetActivity a la aplicación de React `Home.js` encima de la lista de aventuras.

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. Si la aplicación React no se está ejecutando, reinicie con `npm run start`.

   Abra la aplicación React en dos exploradores diferentes, de modo que permita que la prueba A/B ofrezca las diferentes experiencias a cada explorador. Si ambos navegadores muestran la misma oferta de aventura, intente cerrar o volver a abrir uno de los navegadores hasta que se muestre la otra experiencia.

   La siguiente imagen muestra las dos ofertas de fragmentos de contenido diferentes que se muestran para la `wknd-adventure-promo` Actividad, según la lógica de Adobe Target.

   ![Ofertas de experiencias](./assets/target/offers-in-app.png)

## Enhorabuena.

AEM Ahora que hemos configurado la exportación de fragmentos de contenido a Adobe Target, hemos utilizado las ofertas de fragmentos de contenido en una actividad de Adobe Target AEM y hemos as a Cloud Service la exportación de los fragmentos de contenido a, hemos utilizado dicha actividad en una aplicación sin encabezado de y hemos personalizado la experiencia.

---
title: Personalización de AEM sin encabezado y Target
description: En este tutorial se explica cómo se exportan AEM fragmentos de contenido a Adobe Target y cómo se utilizan para personalizar experiencias sin encabezado mediante el SDK web de Adobe.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
source-git-commit: b3cc9c4fbd36cdf5be46e4546a174fea0c8da05c
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 2%

---

# Personalización AEM experiencias sin encabezado con fragmentos de contenido

>[!IMPORTANT]
>
> La exportación de fragmentos de contenido de Adobe Experience Manager a Adobe Target está disponible en el as a Cloud Service de AEM [canal de prelanzamiento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=es#new-features).



En este tutorial se explica cómo se exportan AEM fragmentos de contenido a Adobe Target y cómo se utilizan para personalizar experiencias sin encabezado mediante el SDK web de Adobe. La variable [React WKND App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) se utiliza para explorar cómo se puede añadir a la experiencia una actividad de Target personalizada mediante ofertas de fragmentos de contenido para promocionar una aventura WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

El tutorial trata los pasos necesarios para configurar AEM y Adobe Target:

1. [Crear la configuración de Adobe IMS para Adobe Target](#adobe-ims-configuration) en AEM Author
1. [Crear Cloud Service de Adobe Target](#adobe-target-cloud-service) en AEM Author
1. [Aplicación del Cloud Service de Adobe Target a las carpetas de AEM Assets](#configure-asset-folders) en AEM Author
1. [Permiso para el Cloud Service de Adobe Target](#permission) en Adobe Admin Console
1. [Exportar fragmentos de contenido](#export-content-fragments) de AEM Author a Target
1. [Crear una actividad mediante ofertas de fragmentos de contenido](#activity) en Adobe Target
1. [Crear un conjunto de datos de Experience Platform](#datastream-id) en Experience Platform
1. [Integración de la personalización en una aplicación AEM sin encabezado basada en React](#code) uso del SDK web de Adobe.

## Configuración de Adobe IMS{#adobe-ims-configuration}

Configuración de Adobe IMS que facilita la autenticación entre AEM y Adobe Target.

Consulte [la documentación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) para obtener instrucciones paso a paso sobre cómo crear una configuración de Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Cloud Service de Adobe Target{#adobe-target-cloud-service}

Se crea un Cloud Service de Adobe Target en AEM para facilitar la exportación de fragmentos de contenido a Adobe Target.

Consulte [la documentación](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) para obtener instrucciones paso a paso sobre cómo crear un Cloud Service de Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configuración de carpetas de recursos{#configure-asset-folders}

El Cloud Service de Adobe Target, configurado en una configuración según el contexto, debe aplicarse a la jerarquía de carpetas de AEM Assets que contiene los fragmentos de contenido para exportar a Adobe Target.

+++Expandir para obtener instrucciones paso a paso

1. Iniciar sesión en __Servicio Autor de AEM__ como administrador de DAM
1. Vaya a __Assets > Archivos__, busque la carpeta de recursos que tiene la variable `/conf` aplicado a
1. Seleccione la carpeta de recursos y seleccione __Propiedades__ desde la barra de acciones superior
1. Seleccione la pestaña __Cloud Services__
1. Asegúrese de que la Configuración de nube esté configurada en la configuración según el contexto (`/conf`) que contiene la configuración de Cloud Services de Adobe Target.
1. Select __Adobe Target__ de la variable __Configuraciones del Cloud Service__ lista desplegable.
1. Select __Guardar y cerrar__ en la parte superior derecha

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Permiso para la integración AEM Target{#permission}

Se debe conceder a la integración de Adobe Target, que se manifiesta como proyecto developer.adobe.com, el __Editor__ función de producto en Adobe Admin Console, para exportar fragmentos de contenido a Adobe Target.

+++Expandir para obtener instrucciones paso a paso

1. Inicie sesión en el Experience Cloud como usuario que puede administrar el producto Adobe Target en Adobe Admin Console
1. Abra el [Adobe Admin Console](https://adminconsole.adobe.com)
1. Select __Productos__ y, a continuación, abra __Adobe Target__
1. En el __Perfiles de producto__ , seleccione __*DefaultWorkspace*__
1. Seleccione el __Credenciales de API__ ficha
1. Busque la aplicación developer.adobe.com en esta lista y establezca su __Función del producto__ a __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Exportar fragmentos de contenido a Target{#export-content-fragments}

Fragmentos de contenido que existen en la sección [jerarquía de carpetas de AEM Assets configurada](#apply-adobe-target-cloud-service-to-aem-assets-folders) se puede exportar a Adobe Target como ofertas de fragmento de contenido. Estas ofertas de fragmento de contenido, una forma especial de ofertas JSON en Target, se pueden utilizar en actividades de Target para ofrecer experiencias personalizadas en aplicaciones sin encabezado.

+++Expandir para obtener instrucciones paso a paso

1. Iniciar sesión en __Autor de AEM__ como usuario DAM
1. Vaya a __Assets > Archivos__ y busque Fragmentos de contenido para exportar como JSON a Target en la carpeta &quot;Adobe Target habilitado&quot;
1. Seleccione los fragmentos de contenido que desea exportar a Adobe Target
1. Select __Exportación a ofertas de Adobe Target__ desde la barra de acciones superior
   + Esta acción exporta la representación JSON completamente hidratada del fragmento de contenido a Adobe Target como una &quot;Oferta de fragmento de contenido&quot;
   + La representación JSON completamente hidratada se puede revisar en AEM
      + Seleccione el fragmento de contenido
      + Expandir el panel lateral
      + Select __Vista previa__ en el panel lateral izquierdo
      + La representación JSON que se exporta a Adobe Target se muestra en la vista principal
1. Iniciar sesión en [Adobe Experience Cloud](https://experience.adobe.com) con un usuario en la función Editor para Adobe Target
1. En el [Experience Cloud](https://experience.adobe.com), seleccione __Target__ desde el conmutador de productos en la parte superior derecha para abrir Adobe Target.
1. Asegúrese de que el espacio de trabajo predeterminado esté seleccionado en la __Conmutador de Workspace__ en la parte superior derecha.
1. Seleccione el __Ofertas__ en la barra de navegación superior
1. Seleccione el __Tipo__ lista desplegable y selección __Fragmentos de contenido__
1. Compruebe que el fragmento de contenido exportado desde AEM aparece en la lista
   + Pase el ratón sobre la oferta y seleccione la opción __Ver__ botón
   + Consulte la __Información de la oferta__ y consulte la __AEM vínculo profundo__ que abre el fragmento de contenido directamente en el servicio de AEM Author

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Segmentar actividades mediante ofertas de fragmentos de contenido{#activity}

En Adobe Target, se puede crear una actividad que utilice la oferta de fragmentos de contenido JSON como contenido, lo que permite experiencias personalizadas en aplicaciones sin encabezado con contenido creado y administrado en AEM.

En este ejemplo, utilizamos una actividad A/B simple, pero se puede utilizar cualquier actividad de Target.

+++Expandir para obtener instrucciones paso a paso

1. Seleccione el __Actividades__ en la barra de navegación superior
1. Select __+ Crear actividad__ y, a continuación, seleccione el tipo de actividad que desea crear.
   + Este ejemplo crea un __Prueba A/B__ pero las ofertas de fragmentos de contenido pueden impulsar cualquier tipo de actividad
1. En el __Crear actividad__ asistente
   + Select __Web__
   + En __Elegir Compositor de experiencias__, seleccione __Formulario__
   + En __Elegir espacio de trabajo__, seleccione __Espacio de trabajo predeterminado__
   + En __Elegir propiedad__, seleccione la propiedad en la que está disponible la actividad o seleccione __Sin restricciones de propiedad__ para permitir que se utilice en todas las propiedades.
   + Select __Siguiente__ para crear la actividad
1. Cambie el nombre de la actividad seleccionando __cambiar nombre__ en la parte superior izquierda
   + Asigne un nombre significativo a la actividad
1. En la experiencia inicial, establezca __Ubicación 1__ para que la actividad sea de destino
   + En este ejemplo, establezca como objetivo una ubicación personalizada denominada `wknd-adventure-promo`
1. En __Contenido__ seleccione Contenido predeterminado y, a continuación, __Cambiar fragmento de contenido__
1. Seleccione el fragmento de contenido exportado para esta experiencia y, a continuación, seleccione __Listo__
1. Revise el JSON de oferta de fragmento de contenido en el área de texto Contenido , este es el mismo JSON disponible en el servicio Autor de AEM a través de la acción Vista previa del fragmento de contenido .
1. En el carril izquierdo, añada una Experiencia y seleccione una oferta de fragmento de contenido diferente para mostrar
1. Select __Siguiente__ y configure las reglas de segmentación como sea necesario para la actividad
   + En este ejemplo, deje la prueba A/B como una división 50/50 manual.
1. Select __Siguiente__ y complete la configuración de la actividad
1. Select __Guardar y cerrar__ y asígnele un nombre significativo
1. En Actividad en Adobe Target, seleccione __Activar__ en el menú desplegable Inactivo/Activar/Archivar de la parte superior derecha.

La actividad de Adobe Target que segmenta el `wknd-adventure-promo` La ubicación ahora se puede integrar y exponer en una aplicación AEM sin encabezado.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## ID del conjunto de datos del Experience Platform{#datastream-id}

Un [Almacén de datos de Adobe Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) Se requiere un ID para que AEM aplicaciones sin encabezado interactúen con Adobe Target mediante la variable [SDK web de Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Expandir para obtener instrucciones paso a paso

1. Vaya a [Adobe Experience Cloud](https://experience.adobe.com/)
1. Apertura __Experience Platform__
1. Select __Recopilación de datos > Datastreams__ y seleccione __Nuevo conjunto de datos__
1. En el Asistente para nuevo conjunto de datos, introduzca:
   + Nombre: `AEM Target integration`
   + Descripción: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Esquema de eventos: `Leave blank`
1. Seleccione __Guardar__
1. Select __Añadir servicio__
1. En __Servicio__ select __Adobe Target__
   + Habilitado: __Sí__
   + Token de propiedad: __Dejar en blanco__
   + ID de entorno de Target: __Dejar en blanco__
      + El entorno de Target se puede establecer en Adobe Target en __Administración > Hosts__.
   + Área de nombres de ID de terceros de Target: __Dejar en blanco__
1. Seleccione __Guardar__
1. En el lado derecho, copie el __ID de almacén de datos__ para uso en [SDK web de Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) llamada de configuración.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Añadir personalización a una aplicación AEM sin encabezado{#code}

Este tutorial explora la personalización de una aplicación React simple mediante ofertas de fragmentos de contenido en Adobe Target a través de [SDK web de Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Este método se puede utilizar para personalizar cualquier experiencia web basada en JavaScript.

Las experiencias móviles de Android™ y iOS se pueden personalizar siguiendo patrones similares usando la variable [SDK móvil de Adobe](https://developer.adobe.com/client-sdks/documentation/).

### Requisitos previos

+ Node.js 14
+ Git
+ [WKND compartido 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) instalado en AEM as a Cloud Author y Publish Services

### Configuración

1. Descargue el código fuente de la aplicación React de ejemplo desde [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra la base de código en `~/Code/aem-guides-wknd-graphql/personalization-tutorial` en su IDE favorito
1. Actualice el host del servicio de AEM al que desea que se conecte la aplicación `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Ejecute la aplicación y asegúrese de que se conecta al servicio de AEM configurado. Desde la línea de comandos, ejecute:

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

   El SDK web se puede utilizar en código para recuperar el JSON de la oferta de fragmento de contenido por ubicación de la actividad.

   Al configurar el SDK web, se requieren dos ID:

   + `edgeConfigId` que es el [ID de almacén de datos](#datastream-id)
   + `orgId` el ID de organización de Adobe as a Cloud Service de AEM/Target que se puede encontrar en __Experience Cloud > Perfil > Información de la cuenta > ID de organización actual__

   Al invocar el SDK web, la ubicación de la actividad de Adobe Target (en nuestro ejemplo, `wknd-adventure-promo`) debe configurarse como el valor de la variable `decisionScopes` matriz.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementación

1. Crear un componente React `AdobeTargetActivity.js` para que aparezcan actividades de Adobe Target.

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

   El componente AdobeTargetActivity React se invoca de la siguiente manera:

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Crear un componente React `AdventurePromo.js` para procesar los servidores de adventure JSON Adobe Target.

   Este componente React toma el JSON totalmente hidratado que representa un fragmento de contenido de aventura y se muestra de forma promocional. Los componentes React que muestran el JSON servido desde Ofertas de fragmentos de contenido de Adobe Target pueden ser tan variados y complejos como sea necesario en función de los fragmentos de contenido que se exportan a Adobe Target.

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

1. Agregue el componente AdobeTargetActivity a la aplicación React `Home.js` por encima de la lista de aventuras.

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

1. Si la aplicación React no se está ejecutando, vuelva a utilizarla `npm run start`.

   Abra la aplicación React en dos navegadores diferentes para permitir que la prueba A/B muestre las diferentes experiencias a cada navegador. Si ambos exploradores muestran la misma oferta de aventura, intente cerrar o volver a abrir uno de los exploradores hasta que se muestre la otra experiencia.

   La imagen siguiente muestra las dos ofertas de fragmento de contenido diferentes que se muestran para la variable `wknd-adventure-promo` Actividad, basada en la lógica de Adobe Target.

   ![Ofertas de experiencias](./assets/target/offers-in-app.png)

## Enhorabuena.

Ahora que hemos configurado AEM as a Cloud Service para exportar fragmentos de contenido a Adobe Target, hemos utilizado las ofertas de fragmentos de contenido en una actividad de Adobe Target y hemos mostrado esa actividad en una aplicación sin encabezado de AEM, personalizando la experiencia.

---
title: Crear una propiedad de etiqueta
description: AEM Obtenga información sobre cómo crear una propiedad de etiqueta con la configuración mínima con la que integrar los elementos de la interfaz de usuario de. Los usuarios se familiarizan con la interfaz de usuario de etiquetas y con las extensiones, las reglas y los flujos de trabajo de publicación.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---

# Crear una propiedad de etiqueta {#create-tag-property}

Obtenga información sobre cómo crear una propiedad Tag con la configuración mínima para integrarla con Adobe Experience Manager. Los usuarios se familiarizan con la interfaz de usuario de etiquetas y con las extensiones, las reglas y los flujos de trabajo de publicación.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Creación de propiedades de etiquetas

Para crear una propiedad Tag, complete los siguientes pasos.

1. En el explorador, vaya a la página [Página principal de Adobe Experience Cloud](https://experience.adobe.com/) e inicie sesión con su Adobe ID.

1. Haga clic en la aplicación **Recopilación de datos** de la sección _Acceso rápido_ de la página principal de Adobe Experience Cloud.

1. Haga clic en el elemento de menú **Etiquetas** en el panel de navegación izquierdo y, a continuación, haga clic en **Nueva propiedad** en la esquina superior derecha.

1. Asigne un nombre a la propiedad Tag mediante el campo obligatorio **Name**. En el campo Dominios, escriba su nombre de dominio o, si usa el entorno de AEM as a Cloud Service, escriba `adobeaemcloud.com` y haga clic en **Guardar**.

   ![Propiedades de etiqueta](assets/tag-properties.png)

## Crear una nueva regla

Abra la propiedad Tag recién creada haciendo clic en su nombre en la vista **Propiedades de la etiqueta**. También bajo el encabezado _Mi actividad reciente_ debería ver que la extensión principal se le agregó. La extensión de etiquetas Core es la extensión predeterminada y proporciona tipos de eventos básicos como carga de página, explorador, formulario y otros tipos de eventos. Consulte [Información general sobre la extensión Core](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=es) para obtener más información.

AEM Las reglas permiten especificar lo que debe suceder a medida que el visitante interactúa con el sitio de la. Para simplificar las cosas, vamos a registrar dos mensajes en la consola del explorador para mostrar cómo la integración de etiquetas de recopilación de datos puede insertar código de JavaScript AEM AEM en el sitio de la sin actualizar el código de proyecto de la aplicación.

Para crear una regla, complete los siguientes pasos.

1. Haga clic en **Reglas** de la sección _CREACIÓN_ de la navegación izquierda y luego haga clic en **Crear nueva regla**

1. Asigne un nombre a la regla con el campo obligatorio **Name**.

1. Haga clic en **Agregar** desde la sección _EVENTOS_ y, a continuación, en el formulario _Configuración de eventos_, en el menú desplegable **Tipo de evento**, seleccione la opción _Biblioteca cargada (Principio de página)_ y haga clic en **Conservar cambios**.

1. Haga clic en **Agregar** desde la sección _ACCIONES_ y, a continuación, en el formulario _Configuración de la acción_, en el menú desplegable **Tipo de acción**, seleccione la opción _Código personalizado_ y haga clic en **Abrir editor**.

1. En el modal _Editar código_, escribe el siguiente fragmento de código JavaScript, luego haz clic en **Guardar** y, por último, haz clic en **Conservar cambios**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Haga clic en **Guardar** para finalizar el proceso de creación de reglas.

   ![Nueva regla](assets/new-rule.png)

## Añadir biblioteca y publicarla

La propiedad Tag _Rules_ se activa usando una biblioteca, piense en la biblioteca como un paquete que contiene código JavaScript. Active la regla recién creada siguiendo los pasos.

1. Haga clic en **Flujo de publicación** desde la sección _PUBLICACIÓN_ de la navegación izquierda y luego haga clic en **Agregar biblioteca**

1. Asigne un nombre a la biblioteca utilizando el campo **Name** y seleccione la opción _Desarrollo(desarrollo)_ para el menú desplegable **Entorno**.

1. Para seleccionar todos los recursos modificados desde la creación de la propiedad Tag, haga clic en **+ Agregar todos los recursos modificados**. Esta acción añade la regla recién creada y el recurso de extensión principal a la biblioteca. Finalmente, haga clic en **Guardar y generar en desarrollo**.

1. Una vez creada la biblioteca para la ruta de natación **Development**, con _elipses_, seleccione **Enviar para aprobación**

1. A continuación, en la pista de natación **Submitted** con _elipses_, seleccione **Aprobar para publicación**; del mismo modo, **Build &amp; Publish para producción** en la pista de natación **Approved**.

![Biblioteca publicada](assets/published-library.png)


El paso anterior completa la creación de la propiedad de etiqueta simple que tiene una regla para registrar un mensaje en la consola del explorador cuando se carga la página. Además, la regla y la extensión principal se publican creando una biblioteca.

## Siguientes pasos

[AEM Conexión con la propiedad de etiqueta de mediante IMS](connect-aem-tag-property-using-ims.md)


## Recursos adicionales {#additional-resources}

* [Crear una propiedad de etiqueta](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=es)

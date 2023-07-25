---
title: Crear una propiedad de etiqueta
description: AEM Obtenga información sobre cómo crear una propiedad de etiqueta con la configuración mínima con la que integrar los elementos de la interfaz de usuario de. Los usuarios se familiarizan con la interfaz de usuario de etiquetas y con las extensiones, las reglas y los flujos de trabajo de publicación.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 1%

---

# Crear una propiedad de etiqueta {#create-tag-property}

Obtenga información sobre cómo crear una propiedad Tag con la configuración mínima para integrarla con Adobe Experience Manager. Los usuarios se familiarizan con la interfaz de usuario de etiquetas y con las extensiones, las reglas y los flujos de trabajo de publicación.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Creación de propiedades de etiquetas

Para crear una propiedad Tag, complete los siguientes pasos.

1. En el explorador, vaya a [Inicio de Adobe Experience Cloud](https://experience.adobe.com/) e inicie sesión con su Adobe ID.

1. Haga clic en **Recopilación de datos** aplicación desde el _Acceso rápido_ de la página de inicio de Adobe Experience Cloud.

1. Haga clic en **Etiquetas** del menú de la navegación izquierda y, a continuación, haga clic en **Nueva propiedad** desde la esquina superior derecha.

1. Asigne un nombre a la propiedad Tag mediante **Nombre** campo obligatorio. AEM Para el campo Dominios, introduzca el nombre de dominio o, si utiliza un entorno as a Cloud Service, introduzca: `adobeaemcloud.com` y haga clic en **Guardar**.

   ![Propiedades de etiqueta](assets/tag-properties.png)

## Crear una nueva regla

Abra la propiedad Tag recién creada haciendo clic en su nombre en **Propiedades de etiqueta** vista. También en _Mi actividad reciente_ debe ver que la extensión Core se ha añadido a ella. La Extensión principal de etiquetas es la extensión predeterminada y proporciona tipos de eventos básicos como carga de página, explorador, formulario y otros tipos de eventos. Consulte [Información general sobre la extensión principal](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) para obtener más información.

AEM Las reglas permiten especificar lo que debe suceder a medida que el visitante interactúa con el sitio de la. AEM AEM Para simplificar las cosas, vamos a registrar dos mensajes en la consola del explorador para mostrar cómo la integración de etiquetas de recopilación de datos puede insertar código JavaScript en el sitio de la sin actualizar el código de proyecto de la aplicación.

Para crear una regla, complete los siguientes pasos.

1. Clic **Reglas** desde el _CREACIÓN_ de la barra de navegación izquierda y haga clic en **Crear nueva regla**

1. Asigne un nombre a la regla mediante **Nombre** campo obligatorio.

1. Clic **Añadir** desde el _EVENTOS_ y, a continuación, en _Configuración de eventos_ formulario, en el **Tipo de evento** selección desplegable _Library Loaded (Page Top)_ y haga clic en **Conservar cambios**.

1. Clic **Añadir** desde el _ACCIONES_ y, a continuación, en _Configuración de acción_ formulario, en el **Tipo de acción** selección desplegable _Código personalizado_ y haga clic en **Abrir editor**.

1. En el _Editar código_ modal, introduzca el siguiente fragmento de código JavaScript y haga clic en **Guardar** y, finalmente, haga clic en **Conservar cambios**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Clic **Guardar** para finalizar el proceso de creación de reglas.

   ![Nueva regla](assets/new-rule.png)

## Añadir biblioteca y publicarla

La propiedad Tag _Reglas_ Cuando se activan mediante una biblioteca, piense en la biblioteca como un paquete que contiene código JavaScript. Active la regla recién creada siguiendo los pasos.

1. Clic **Flujo de publicación** desde el _PUBLICACIÓN_ de la navegación izquierda y, a continuación, haga clic en **Añadir biblioteca**

1. Asigne un nombre a la biblioteca mediante **Nombre** y seleccione _Development(desarrollo)_ opción para **Entorno** desplegable.

1. Para seleccionar todos los recursos modificados desde la creación de la propiedad Tag, haga clic en **+ Agregar todos los recursos modificados**. Esta acción añade la regla recién creada y el recurso de extensión principal a la biblioteca. Finalmente, haga clic **Guardar y generar en desarrollo**.

1. Una vez creada la biblioteca para el **Desarrollo** carril para nadar, uso _elipses_ seleccione el **Enviar para aprobación**

1. A continuación, en la **Enviado** carril para nadar usando _elipses_ seleccione el **Aprobar para publicación**, del mismo modo **Generar y publicar en producción** en el **Aprobado** callejuela de natación.

![Biblioteca publicada](assets/published-library.png)


El paso anterior completa la creación de la propiedad de etiqueta simple que tiene una regla para registrar un mensaje en la consola del explorador cuando se carga la página. Además, la regla y la extensión principal se publican creando una biblioteca.

## Pasos siguientes

[AEM Conexión con la propiedad de etiqueta de mediante IMS](connect-aem-tag-property-using-ims.md)


## Recursos adicionales {#additional-resources}

* [Crear una propiedad de etiqueta](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)

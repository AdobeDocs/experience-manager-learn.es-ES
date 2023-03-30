---
title: Crear una propiedad de etiqueta
description: Aprenda a crear una propiedad Tag con la configuración mínima para integrarla con AEM. Los usuarios se familiarizan con la interfaz de usuario de etiquetas y aprenden sobre las extensiones, las reglas y los flujos de trabajo de publicación.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Crear una propiedad de etiqueta {#create-tag-property}

Obtenga información sobre cómo crear una propiedad Tag con la configuración mínima para integrarla con Adobe Experience Manager. Los usuarios se familiarizan con la interfaz de usuario de etiquetas y aprenden sobre las extensiones, las reglas y los flujos de trabajo de publicación.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Creación de propiedad de etiqueta

Para crear una propiedad Tag , complete los siguientes pasos.

1. En el navegador, vaya a la [Página principal de Adobe Experience Cloud](https://experience.adobe.com/) e inicie sesión con su Adobe ID.

1. Haga clic en el **Recopilación de datos** de _Acceso rápido_ de la página principal de Adobe Experience Cloud.

1. Haga clic en el **Etiquetas** elemento de menú del panel de navegación izquierdo y, a continuación, haga clic en **Nueva propiedad** desde la esquina superior derecha.

1. Asigne un nombre a la propiedad Tag usando la variable **Nombre** campo obligatorio. En el campo Dominios , introduzca su nombre de dominio o, si utiliza AEM entorno as a Cloud Service, introduzca `adobeaemcloud.com` y haga clic en **Guardar**.

   ![Propiedades de la etiqueta](assets/tag-properties.png)

## Crear una regla nueva

Abra la propiedad Tag recién creada haciendo clic en su nombre en el **Propiedades de la etiqueta** vista. También en _Mi actividad reciente_ debería ver que la extensión principal se le ha añadido. La extensión de la etiqueta principal es la extensión predeterminada y proporciona tipos de eventos básicos, como carga de página, explorador, formulario y otros tipos de eventos, consulte [Información general de la extensión principal](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) para obtener más información.

Las reglas permiten especificar lo que debe suceder a medida que el visitante interactúa con el sitio AEM. Para que las cosas sean sencillas, registremos dos mensajes en la consola del explorador para demostrar cómo la integración de etiquetas de recopilación de datos puede insertar código JavaScript en el sitio AEM sin actualizar AEM código del proyecto.

Para crear una regla, complete los siguientes pasos.

1. Haga clic en **Reglas** de la variable _CREACIÓN_ de la navegación izquierda y, a continuación, haga clic en **Crear nueva regla**

1. Asigne un nombre a la regla mediante la variable **Nombre** campo obligatorio.

1. Haga clic en **Agregar** de la variable _EVENTOS_ y luego en la sección _Configuración de eventos_ en el **Tipo de evento** selección desplegable _Biblioteca cargada (Principio de página)_ y haga clic en **Conservar cambios**.

1. Haga clic en **Agregar** de la variable _ACCIONES_ y luego en la sección _Configuración de la acción_ en el **Tipo de acción** selección desplegable _Código personalizado_ y haga clic en **Abrir editor**.

1. En el _Editar código_ modal, introduzca el siguiente fragmento de código JavaScript y, a continuación, haga clic en **Guardar** y, finalmente, haga clic en **Conservar cambios**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Haga clic en **Guardar** para finalizar el proceso de creación de reglas.

   ![Nueva regla](assets/new-rule.png)

## Agregar biblioteca y publicarla

La propiedad Tag _Reglas_ se activan mediante una biblioteca, piense en la biblioteca como un paquete que contiene código JavaScript. Active la regla recién creada siguiendo los pasos.

1. Haga clic en **Flujo de publicación** de la variable _PUBLICACIÓN_ de la navegación izquierda, haga clic en **Agregar biblioteca**

1. Asigne un nombre a la biblioteca mediante la variable **Nombre** y seleccione _Desarrollo (desarrollo)_ para **Entorno** lista desplegable.

1. Para seleccionar todos los recursos modificados desde la creación de la propiedad Tag , haga clic en **+ Agregar todos los recursos modificados**. Esta acción agrega la regla recién creada y el recurso de extensión principal a la biblioteca . Haga clic en **Guardar y crear en desarrollo**.

1. Una vez creada la biblioteca para la variable **Desarrollo** carril de nado, usar _elipses_ seleccione **Enviar para aprobación**

1. A continuación, en el **Enviado** carril de nado que utiliza _elipses_ seleccione **Aprobar para publicación**, del mismo modo **Generar y publicar en producción** en el **Aprobado** carril de nado.

![Biblioteca publicada](assets/published-library.png)


El paso anterior completa la creación sencilla de la propiedad Tag que tiene una regla para registrar un mensaje en la consola del explorador cuando se carga la página. Además, la regla y la extensión principal se publican creando una biblioteca.

## Pasos siguientes

[Conectar AEM con la propiedad de etiqueta mediante IMS](connect-aem-tag-property-using-ims.md)


## Recursos adicionales {#additional-resources}

* [Crear una propiedad de etiqueta](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)

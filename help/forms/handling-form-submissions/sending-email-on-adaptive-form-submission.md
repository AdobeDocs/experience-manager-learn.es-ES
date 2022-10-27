---
title: Envío de correo electrónico en envío de formulario adaptable
seo-title: Sending Email on Adaptive Form Submission
description: Enviar correo electrónico de confirmación sobre el envío de formulario adaptable mediante el componente de envío de correo electrónico
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 3%

---

# Envío de correo electrónico en envío de formulario adaptable {#sending-email-on-adaptive-form-submission}

Una de las acciones comunes es enviar un correo electrónico de confirmación al remitente cuando el formulario adaptable se envía correctamente. Para ello, seleccione &quot;Enviar correo electrónico&quot; como acción de envío.

Puede utilizar la plantilla de correo electrónico o simplemente escribir el cuerpo del correo electrónico como se muestra en esta captura de pantalla a continuación.

Observe la sintaxis para insertar valores de campo de formulario en el correo electrónico. También tenemos la opción de incluir archivos adjuntos de formulario en el correo electrónico, seleccionando la casilla &quot;incluir archivos adjuntos&quot; en las propiedades de configuración.

Cuando se envía el formulario adaptable, el destinatario recibe un correo electrónico.

![SendEmail](assets/sendemailaction.gif)

## Configuraciones necesarias {#configurations-needed}

Tendrá que configurar el servicio Day CQ Mail. Esto se puede configurar señalando el navegador a [Administrador de configuración de Felix](http://localhost:4502/system/console/configMgr)

La captura de pantalla muestra las propiedades de configuración del servidor de correo de adobe.

![mailservice](assets/mailservice.png)

Para probar esto en su servidor, siga estas instrucciones:

* [Importación de recursos](assets/timeoffrequest.zip) asociado con este artículo en AEM usando el administrador de paquetes.

* Abra el [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Rellene los detalles. Asegúrese de proporcionar una dirección de correo electrónico válida en el campo de correo electrónico.

* Enviar el formulario.

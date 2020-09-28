---
title: Envío de correo electrónico en envío de formulario adaptable
seo-title: Envío de correo electrónico en envío de formulario adaptable
description: Enviar correo electrónico de confirmación al envío de formulario adaptable mediante el componente Enviar correo electrónico
seo-description: Enviar correo electrónico de confirmación al envío de formulario adaptable mediante el componente Enviar correo electrónico
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: adaptive-forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
translation-type: tm+mt
source-git-commit: 272e43ee4aeb756a3a1fd0ce55eaca94a9553fa4
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# Envío de correo electrónico en envío de formulario adaptable {#sending-email-on-adaptive-form-submission}

Una de las acciones comunes es enviar un mensaje de correo electrónico de confirmación al remitente cuando el formulario adaptable se envía correctamente. Para lograrlo, seleccionaremos la acción &quot;Enviar correo electrónico&quot; como envío.

Puede usar la plantilla de correo electrónico o simplemente escribir el cuerpo del mensaje de correo electrónico como se muestra en esta captura de pantalla a continuación.

Observe la sintaxis para insertar valores de campo de formulario en el correo electrónico.También tenemos la opción de incluir datos adjuntos de formulario en el correo electrónico, seleccionando la casilla de verificación &quot;incluir datos adjuntos&quot; en las propiedades de configuración.

Cuando se envía el formulario adaptable, el destinatario recibe un mensaje de correo electrónico.

![SendEmail](assets/sendemailaction.gif)

## Configuraciones necesarias {#configurations-needed}

Tendrá que configurar el servicio Day CQ Mail. Esto se puede configurar señalando el explorador al Administrador de configuración de [Felix](http://localhost:4502/system/console/configMgr)

La captura de pantalla muestra las propiedades de configuración del servidor de correo de adobe.

![mailservice](assets/mailservice.png)

Para probar esto en el servidor, siga estas instrucciones:

* [Importe los recursos](assets/timeoffrequest.zip) asociados con este artículo en AEM mediante el administrador de paquetes.

* Abra [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Rellene los detalles.Asegúrese de proporcionar una dirección de correo electrónico válida en el campo Correo electrónico.

* Envíe el formulario.

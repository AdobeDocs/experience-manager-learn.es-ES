---
title: Envío de correo electrónico en envío de formulario adaptable
seo-title: Envío de correo electrónico en envío de formulario adaptable
description: Enviar correo electrónico de confirmación sobre el envío de formulario adaptable mediante el componente de envío de correo electrónico
seo-description: Enviar correo electrónico de confirmación sobre el envío de formulario adaptable mediante el componente de envío de correo electrónico
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# Envío de correo electrónico en envío de formulario adaptable {#sending-email-on-adaptive-form-submission}

Una de las acciones comunes es enviar un correo electrónico de confirmación al remitente cuando el formulario adaptable se envía correctamente. Para ello, seleccione &quot;Enviar correo electrónico&quot; como acción de envío.

Puede utilizar la plantilla de correo electrónico o simplemente escribir el cuerpo del correo electrónico como se muestra en esta captura de pantalla a continuación.

Observe la sintaxis para insertar valores de campo de formulario en el correo electrónico. También tenemos la opción de incluir archivos adjuntos de formulario en el correo electrónico, seleccionando la casilla &quot;incluir archivos adjuntos&quot; en las propiedades de configuración.

Cuando se envía el formulario adaptable, el destinatario recibe un correo electrónico.

![SendEmail](assets/sendemailaction.gif)

## Configuraciones necesarias {#configurations-needed}

Tendrá que configurar el servicio Day CQ Mail. Esto se puede configurar apuntando su navegador a [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

La captura de pantalla muestra las propiedades de configuración del servidor de correo de adobe.

![mailservice](assets/mailservice.png)

Para probar esto en su servidor, siga estas instrucciones:

* [Importe los ](assets/timeoffrequest.zip) recursos asociados a este artículo en AEM mediante el administrador de paquetes.

* Abra [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Rellene los detalles. Asegúrese de proporcionar una dirección de correo electrónico válida en el campo de correo electrónico.

* Envíe el formulario.

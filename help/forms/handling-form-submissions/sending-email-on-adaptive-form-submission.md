---
title: Enviar correo electrónico al enviar formulario adaptable
description: Enviar correo electrónico de confirmación sobre el envío de formularios adaptables mediante el componente Enviar correo electrónico
feature: Adaptive Forms
doc-type: article
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
duration: 40
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 2%

---

# Enviar correo electrónico al enviar formulario adaptable {#sending-email-on-adaptive-form-submission}

Una de las acciones comunes es enviar un correo electrónico de confirmación al remitente cuando el formulario adaptable se haya enviado correctamente. Para ello, seleccionaremos la acción &quot;Enviar correo electrónico&quot; como acción de envío.

Puede utilizar una plantilla de correo electrónico o simplemente escribir el cuerpo del correo electrónico como se muestra en esta captura de pantalla a continuación.

Observe la sintaxis para insertar valores de campo de formulario en el correo electrónico. También tenemos la opción de incluir archivos adjuntos del formulario en el correo electrónico, activando la casilla de verificación &quot;incluir archivos adjuntos&quot; en las propiedades de configuración.

Cuando se envía el formulario adaptable, el destinatario recibe un correo electrónico.

![SendEmail](assets/sendemailaction.gif)

## Configuraciones necesarias {#configurations-needed}

Tendrá que configurar el servicio Day CQ Mail. Esto se puede configurar apuntando el explorador a [Administrador de configuración de Felix](http://localhost:4502/system/console/configMgr)

La captura de pantalla muestra las propiedades de configuración del servidor de correo de Adobe.

![mailservice](assets/mailservice.png)

Para probar esto en el servidor, siga estas instrucciones:

* [Importar los recursos](assets/timeoffrequest.zip) AEM asociado con este artículo en el uso del administrador de paquetes para el uso de la.

* Abra el [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* Rellene los detalles. Asegúrese de proporcionar una dirección de correo electrónico válida en el campo de correo electrónico.

* Enviar el formulario.

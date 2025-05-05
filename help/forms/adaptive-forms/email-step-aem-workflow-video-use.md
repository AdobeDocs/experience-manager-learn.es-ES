---
title: Uso de los pasos de envío de correo electrónico de Forms Workflow
description: Los pasos para enviar correo electrónico se introdujeron en AEM Forms 6.4. Con este paso podemos crear procesos o flujos de trabajo empresariales que le permitan enviar correos electrónicos con o sin archivos adjuntos. El siguiente vídeo muestra los pasos para configurar el componente Enviar correo electrónico
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 314
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 2%

---

# Uso de los pasos de envío de correo electrónico de Forms Workflow {#using-send-email-step-of-forms-workflow}

Los pasos para enviar correo electrónico se introdujeron en AEM Forms 6.4. Con este paso podemos crear procesos o flujos de trabajo empresariales que le permitan enviar correos electrónicos con o sin archivos adjuntos. El siguiente vídeo muestra los pasos para configurar el componente Enviar correo electrónico.

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

Como parte de este artículo, le guiaremos por el siguiente caso de uso:

1. Un usuario rellena el formulario de solicitud de días libres
1. Al enviar el formulario, se activa el flujo de trabajo de AEM
1. El flujo de trabajo de AEM utiliza el componente Enviar correo electrónico para enviar un correo electrónico con el DoR como archivo adjunto

Antes de usar Enviar correo electrónico, asegúrese de configurar el servicio Day CQ Mail desde [configMgr](http://localhost:4502/system/console/configMgr). Proporcione los valores específicos de su entorno

![Configurar el servicio Day CQ Mail](assets/mailservice.png)

Como parte de los recursos asociados con este artículo, obtendrá lo siguiente

1. Formulario adaptable que almacenará en déclencheur el flujo de trabajo en el envío
1. Flujo de trabajo de ejemplo que enviará un correo electrónico con el documento de registro como archivo adjunto
1. Paquete OSGi que crea las propiedades de los metadatos

Para que el ejemplo se ejecute en el sistema, haga lo siguiente:

1. [Implementar el paquete Develingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Descargue e instale el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)Este paquete contiene el código para crear las propiedades de metadatos como parte del paso de proceso del flujo de trabajo.
1. [Configurar el servicio Day CQ Mail](https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importe e instale los recursos asociados con este artículo mediante el administrador de paquetes en CRX](assets/emaildoraemformskt.zip)
1. Iniciar [formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Rellene los campos obligatorios y envíe.
1. Debe recibir un correo electrónico con DocumentOfRecord como archivo adjunto

Explorar el [modelo de flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Eche un vistazo al paso del proceso del flujo de trabajo. El código personalizado asociado con el paso de proceso creará nombres de propiedades de metadatos y establecerá sus valores a partir de los datos enviados. Estos valores se utilizan en el componente de envío de correo electrónico.

>[!NOTE]
>
>En AEM Forms 6.5 y versiones posteriores, no necesita este código personalizado para crear propiedades de metadatos. Utilice la capacidad de variables en el flujo de trabajo de AEM

Asegúrese de que la pestaña Archivos adjuntos del componente Enviar correo electrónico esté configurada según la captura de pantalla siguiente
![Enviar ficha de datos adjuntos de correo electrónico](assets/sendemailcomponentconfigure.jpg)El valor &quot;DOR.pdf&quot; debe coincidir con el valor especificado en la ruta de documento de registro especificada en las opciones de envío del formulario adaptable.

---
title: Uso del flujo de trabajo Enviar paso de correo electrónico de Forms
seo-title: Uso del flujo de trabajo Enviar paso de correo electrónico de Forms
description: El paso Enviar correo electrónico se ha introducido en AEM Forms 6.4. Con este paso, podemos crear procesos o flujos de trabajo empresariales que le permitan enviar correos electrónicos con o sin archivos adjuntos. El siguiente vídeo muestra los pasos para configurar el componente de envío de correo electrónico
seo-description: El paso Enviar correo electrónico se ha introducido en AEM Forms 6.4. Con este paso, podemos crear procesos o flujos de trabajo empresariales que le permitan enviar correos electrónicos con o sin archivos adjuntos. El siguiente vídeo muestra los pasos para configurar el componente de envío de correo electrónico
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---


# Uso del paso de envío de correo electrónico del flujo de trabajo de formularios {#using-send-email-step-of-forms-workflow}

El paso Enviar correo electrónico se ha introducido en AEM Forms 6.4. Con este paso, podemos crear procesos o flujos de trabajo empresariales que le permitan enviar correos electrónicos con o sin archivos adjuntos. El siguiente vídeo muestra los pasos para configurar el componente de envío de correo electrónico.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

Como parte de este artículo, le explicaremos el siguiente caso de uso:

1. Un usuario rellena el formulario de solicitud de tiempo de espera
1. Al enviar el formulario, se activa el flujo de trabajo de AEM
1. El flujo de trabajo de AEM utiliza el componente Enviar correo electrónico para enviar un correo electrónico con el DoR como archivo adjunto

Antes de utilizar el paso Enviar correo electrónico, asegúrese de configurar el servicio de correo de CQ Day desde [configMgr](http://localhost:4502/system/console/configMgr). Proporcione los valores específicos de su entorno

![Configurar el servicio Day CQ Mail](assets/mailservice.png)

Como parte de los recursos asociados con este artículo, recibirá lo siguiente

1. Formulario adaptable que activará el flujo de trabajo en el envío
1. Flujo de trabajo de muestra que enviará un correo electrónico con DOR como archivo adjunto
1. Paquete OSGi que crea las propiedades de metadatos

Para que la muestra se ejecute en su sistema, haga lo siguiente:

1. [Implementar el paquete de usuario Desarrollo con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Descargar e instalar el ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)paquete setvalueEste paquete contiene el código para crear las propiedades de los metadatos como parte del paso de proceso del flujo de trabajo.
1. [Configurar el servicio Day CQ Mail](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importe e instale los activos asociados con este artículo mediante el administrador de paquetes en CRX](assets/emaildoraemformskt.zip)
1. Inicie el [formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). Complete los campos obligatorios y envíe.
1. Debe obtener un correo electrónico con DocumentOfRecord como archivo adjunto

Explorar el [modelo de flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Eche un vistazo al paso del proceso del flujo de trabajo. El código personalizado asociado con el paso de proceso crea nombres de propiedades de metadatos y establece sus valores a partir de los datos enviados. Estos valores los utiliza el componente de envío de correo electrónico.

>[!NOTE]
>
>En AEM Forms 6.5 y posteriores no necesita este código personalizado para crear propiedades de metadatos. Utilice la capacidad de variables en el flujo de trabajo de AEM

Asegúrese de que la pestaña Attachments del componente Send Email esté configurada según la captura de pantalla siguiente
![Send Email Attachment Tab](assets/sendemailcomponentconfigure.jpg)El valor &quot;DOR.pdf&quot; debe coincidir con el valor especificado en la ruta de registro del documento especificada en las opciones de envío del formulario adaptable.


---
title: Uso del paso de envío de correo electrónico del Forms Workflow
seo-title: Uso del paso de envío de correo electrónico del Forms Workflow
description: El paso Enviar correo electrónico se introdujo en AEM Forms 6.4. Con este paso podemos crear procesos de negocio o flujo de trabajo que le permitirán enviar correos electrónicos con o sin datos adjuntos. El siguiente vídeo recorre los pasos para configurar el componente de envío de correo electrónico
seo-description: El paso Enviar correo electrónico se introdujo en AEM Forms 6.4. Con este paso podemos crear procesos de negocio o flujo de trabajo que le permitirán enviar correos electrónicos con o sin datos adjuntos. El siguiente vídeo recorre los pasos para configurar el componente de envío de correo electrónico
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---


# Uso del paso de envío de correo electrónico del Forms Workflow {#using-send-email-step-of-forms-workflow}

El paso Enviar correo electrónico se introdujo en AEM Forms 6.4. Con este paso podemos crear procesos de negocio o flujo de trabajo que le permitirán enviar correos electrónicos con o sin datos adjuntos. El siguiente vídeo muestra los pasos para configurar el componente de envío de correo electrónico.

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

Como parte de este artículo, le explicaremos el siguiente caso de uso:

1. Un usuario rellena el formulario de solicitud de tiempo de espera
1. Al enviar el formulario, se activa AEM flujo de trabajo
1. El flujo de trabajo AEM utiliza el componente Enviar correo electrónico para enviar un correo electrónico con el documento de trabajo como datos adjuntos

Antes de utilizar el paso Enviar correo electrónico, asegúrese de configurar el servicio de correo de Day CQ desde [configMgr](http://localhost:4502/system/console/configMgr). Proporcione los valores específicos del entorno

![Configurar el servicio de correo CQ Day](assets/mailservice.png)

Como parte de los recursos asociados con este artículo, obtendrá lo siguiente

1. Formulario adaptable que activará el flujo de trabajo al enviar
1. Flujo de trabajo de muestra que enviará un correo electrónico con el DOR como archivo adjunto
1. Paquete OSGi que crea las propiedades de metadatos

Para que la muestra se ejecute en el sistema, haga lo siguiente:

1. [Implementar el paquete DevelopmentWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Descargar e instalar el](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)paquete setvalueEste paquete contiene el código para crear las propiedades de metadatos como parte del paso de proceso del flujo de trabajo.
1. [Configurar el servicio de correo CQ Day](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [Importar e instalar los recursos asociados con este artículo mediante el administrador de paquetes en CRX](assets/emaildoraemformskt.zip)
1. Inicie el formulario [](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)adaptable. Rellene los campos obligatorios y envíe.
1. Debe obtener un correo electrónico con DocumentOfRecord como archivo adjunto

Explorar el modelo de [flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

Observe el paso del proceso del flujo de trabajo. El código personalizado asociado con el paso del proceso creará nombres de propiedades de metadatos y establecerá sus valores a partir de los datos enviados. Estos valores se utilizarán posteriormente en el componente de envío de correo electrónico.

>[!NOTE]
>
>En AEM Forms 6.5 y versiones posteriores no necesita este código personalizado para crear propiedades de metadatos. Utilice la capacidad de variables en AEM flujo de trabajo

Asegúrese de que la ficha Archivos adjuntos del componente Enviar correo electrónico esté configurada según la captura de pantalla debajo![de la ficha Enviar](assets/sendemailcomponentconfigure.jpg)archivo adjunto de correo electrónicoEl valor &quot;DOR.pdf&quot; debe coincidir con el valor especificado en el Documento de la ruta de registro especificado en las opciones de envío del formulario adaptable.


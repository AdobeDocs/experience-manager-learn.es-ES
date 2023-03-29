---
title: Conectar AEM con la propiedad de etiqueta mediante IMS
description: Obtenga información sobre cómo conectar AEM con Tag Property mediante la configuración de IMS en AEM. Esta configuración autentica AEM con la API de Launch y permite que los AEM se comuniquen mediante las API de Launch para acceder a las propiedades de la etiqueta.
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 2%

---

# Conectar AEM con la propiedad de etiqueta mediante IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>El proceso de cambiar el nombre de Adobe Experience Platform Launch como un conjunto de tecnologías de recopilación de datos se está implementando en la interfaz de usuario, el contenido y la documentación del producto de AEM, por lo que el término Launch se sigue utilizando aquí.

Obtenga información sobre cómo conectar AEM con Tag Property mediante la configuración de IMS (Sistema Identity Management) en AEM. Esta configuración autentica AEM con la API de Launch y permite que los AEM se comuniquen mediante las API de Launch para acceder a las propiedades de la etiqueta.

## Crear o reutilizar la configuración de IMS

La configuración de IMS que utiliza el proyecto de la consola de Adobe Developer es necesaria para integrar AEM con la propiedad de etiqueta recién creada. Esta configuración permite AEM comunicación con la aplicación de etiquetas mediante las API de Launch e IMS gestiona el aspecto de seguridad de esta integración.

Cada vez que se aprovisiona un entorno de AEM como Cloud Service, se crean automáticamente algunas configuraciones de IMS, como Asset compute, Adobe Analytics y Adobe Launch. La creación automática **Adobe Launch** La configuración de IMS se puede usar o se debe crear una nueva configuración de IMS si utiliza AEM entorno 6.X.

Revisar creado automáticamente **Adobe Launch** Configuración de IMS siguiendo estos pasos.

1. En AEM, abra el menú **Herramientas**

1. En la sección Seguridad , seleccione Configuraciones de IMS de Adobe.

1. Seleccione el **Adobe Launch** tarjeta y haga clic en **Propiedades**, revise los detalles de **Certificado** y **Cuenta** pestañas. A continuación, haga clic en **Cancelar** para volver sin modificar ningún detalle creado automáticamente.

1. Seleccione el **Adobe Launch** tarjeta y este clic **Comprobar estado**, debería ver el **Correcto** como a continuación.

   ![Configuración de IMS saludable de Adobe Launch](assets/adobe-launch-healthy-ims-config.png)


## Pasos siguientes

[Crear una configuración de Cloud Service de Launch en AEM](create-aem-launch-cloud-service.md)

---
title: Conexión de AEM Sites con la propiedad Tag mediante IMS
description: Obtenga información sobre cómo conectar AEM Sites AEM con la propiedad Tag mediante la configuración de IMS en la configuración de la.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 72
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 1%

---

# Conexión de AEM Sites con la propiedad Tag mediante IMS{#connect-aem-and-tag-property-using-ims}

AEM Obtenga información sobre cómo conectarse con la propiedad de etiquetas mediante la configuración de IMS (Identity Management AEM System) en la. AEM AEM Esta configuración se autentica con la API de etiquetas y permite a los usuarios comunicarse a través de las API de etiquetas para acceder a las propiedades de las etiquetas.

## Crear o reutilizar la configuración de IMS

La configuración de IMS que utiliza el proyecto de la consola de Adobe Developer AEM es necesaria para integrar la propiedad de etiquetas con la propiedad de etiquetas recién creada. AEM Esta configuración permite a los usuarios comunicarse con la aplicación Etiquetas mediante las API de etiquetas y el IMS gestiona el aspecto de seguridad de esta integración.

AEM Siempre que se aprovisiona un entorno de Cloud Service de as a, se crean automáticamente algunas configuraciones de IMS, como Asset compute, Adobe Analytics y etiquetas. El creado automáticamente **etiquetas en Adobe Experience Platform** AEM Se puede utilizar la configuración de IMS o se debe crear una nueva configuración de IMS si utiliza el entorno 6.X de la versión de.

Revisión creada automáticamente **etiquetas en Adobe Experience Platform** Configuración de IMS mediante los siguientes pasos.

1. AEM En el Autor de la, abra **Herramientas** menú
1. En la sección Seguridad, seleccione Configuraciones de IMS de Adobe.
1. Seleccione el **Adobe Launch** y haga clic en **Propiedades**, revise los detalles desde **Certificado** y **Cuenta** pestañas. Luego haga clic en **Cancelar** para volver sin modificar ningún detalle creado automáticamente.
1. Seleccione el **Adobe Launch** y esta vez haga clic en **Comprobar estado**, debería ver el **Correcto** mensaje como el siguiente.

   ![Configuración de IMS correcta de etiquetas](assets/adobe-launch-healthy-ims-config.png)

## Pasos siguientes

[Creación de una configuración de Cloud Service AEM de etiquetas en el](create-aem-launch-cloud-service.md)

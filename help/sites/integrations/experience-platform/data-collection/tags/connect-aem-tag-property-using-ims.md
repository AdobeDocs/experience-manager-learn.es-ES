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
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 1%

---

# Conexión de AEM Sites con la propiedad Tag mediante IMS{#connect-aem-and-tag-property-using-ims}

AEM Obtenga información sobre cómo conectarse con la propiedad de etiquetas mediante la configuración de IMS (Identity Management AEM System) en la. AEM AEM Esta configuración se autentica con la API de etiquetas y permite a los usuarios comunicarse a través de las API de etiquetas para acceder a las propiedades de las etiquetas.

## Crear o reutilizar la configuración de IMS

La configuración de IMS que utiliza el proyecto de Adobe Developer Console AEM es necesaria para integrar la propiedad de etiquetas con la propiedad de etiquetas recién creada. AEM Esta configuración permite a los usuarios comunicarse con la aplicación Etiquetas mediante las API de etiquetas y el IMS gestiona el aspecto de seguridad de esta integración.

AEM Siempre que se aprovisiona un entorno de Cloud Service de as a, se crean automáticamente algunas configuraciones de IMS, como Asset compute, Adobe Analytics y etiquetas. Se pueden usar las **etiquetas creadas automáticamente en la configuración de IMS de Adobe Experience Platform AEM**, o se debe crear una nueva configuración de IMS si usa un entorno de 6.X en el que se usa la configuración de.

Revise las **etiquetas creadas automáticamente en la configuración de Adobe Experience Platform** IMS siguiendo los pasos siguientes.

1. AEM En el Autor de la, abra el menú **Herramientas**
1. En la sección Seguridad, seleccione Configuraciones de IMS de Adobe.
1. Seleccione la tarjeta **Adobe Launch** y haga clic en **Propiedades**, revise los detalles de las pestañas **Certificado** y **Cuenta**. A continuación, haga clic en **Cancelar** para volver sin modificar los detalles creados automáticamente.
1. Seleccione la tarjeta **Adobe Launch** y esta vez haga clic en **Comprobar estado**. Debería ver el mensaje **Éxito** como el que se muestra a continuación.

   ![Etiquetas con configuración de IMS correcta](assets/adobe-launch-healthy-ims-config.png)

## Siguientes pasos

[Creación de una configuración de Cloud Service AEM de etiquetas en el](create-aem-launch-cloud-service.md)

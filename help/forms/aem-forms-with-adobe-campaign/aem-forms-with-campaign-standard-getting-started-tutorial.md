---
title: Introducción a AEM Forms y Adobe Campaign Standard
description: Integre AEM Forms con Adobe Campaign Standard mediante el modelo de datos de formulario de AEM Forms para recuperar información del perfil de la campaña de ACS, etc.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Introducción a AEM Forms y Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Este tutorial enumerará los distintos casos de uso para integrar AEM Forms con Adobe Campaign Standard (ACS).

ACS tiene un completo conjunto de API expuestas, lo que permite a ACS interconectarse con la tecnología de su elección. En este tutorial, nos concentraremos en la interfaz de AEM Forms con ACS.

Para integrar AEM Forms con ACS deberá seguir los siguientes pasos:

* [Configure el acceso a la API en la instancia de ACS.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* Cree el token web JSON.
* Intercambie el token web JSON con el servicio Identity Management de Adobe por un token de acceso.
* Incluya este token de acceso en el encabezado HTTP de autorización, junto con X-API-Key en cada solicitud a la instancia de ACS.

Para empezar, siga las siguientes instrucciones

* [Descargue y descomprima los recursos relacionados con este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implementación de los paquetes mediante [Consola web Felix](http://localhost:4502/system/console/bundles)
* Proporcione la configuración adecuada para Adobe Campaign en la configuración de Felix OSGI.
* [Cree un usuario de servicio como se menciona en este artículo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Asegúrese de implementar el paquete OSGi asociado con el artículo.
* Almacene la clave privada ACS en etc/key/campaign/private.key. Debe crear una carpeta llamada campaña en etc/key.
* [Proporcione acceso de lectura a la carpeta de la campaña a los &quot;datos&quot; del usuario del servicio.](http://localhost:4502/useradmin)

## Pasos siguientes

[Generar JWT y token de acceso](partone.md)

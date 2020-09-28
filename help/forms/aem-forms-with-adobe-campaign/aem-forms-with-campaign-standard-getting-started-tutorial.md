---
title: Introducción a AEM Forms y Adobe Campaign Standard
seo-title: Introducción a AEM Forms y Adobe Campaign Standard
description: Integre AEM Forms con Adobe Campaign Standard mediante el Modelo de datos de formulario de AEM Forms para obtener información sobre perfiles de campaña de ACS, etc.
seo-description: Integre AEM Forms con Adobe Campaign Standard mediante el Modelo de datos de formulario de AEM Forms para obtener información sobre perfiles de campaña de ACS, etc.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
translation-type: tm+mt
source-git-commit: 3b44a9e2341ce23f737e6ef75c67fadd9870d2ac
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Introducción a AEM Forms y Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Este tutorial lista los diversos casos de uso para integrar AEM Forms con Adobe Campaign Standard(ACS).

ACS tiene un completo conjunto de API expuestas que permite a ACS ser interconectado con la tecnología de nuestra elección. En este tutorial, nos concentraremos en conectar AEM Forms con ACS.

Para integrar AEM Forms con ACS, deberá seguir los siguientes pasos:

* [Configure el acceso a la API en la instancia de ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Crear un token web JSON.
* Intercambiar el autentificador web JSON con el servicio Identity Management de Adobe para un Token de acceso.
* Incluya este Token de acceso en el Encabezado HTTP de autorización, junto con X-API-Key en cada solicitud a la instancia de ACS.

Para empezar, siga las siguientes instrucciones

* [Descargue y descomprima los recursos relacionados con este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implementar los paquetes con la consola web [Felix](http://localhost:4502/system/console/bundles)
* Proporcione la configuración adecuada para Adobe Campaign en la configuración de Felix OSGI.
* [Cree un usuario de servicio como se indica en este artículo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Asegúrese de implementar el paquete OSGi asociado al artículo.
* Guarde la clave privada de ACS en etc/key/campaign/private.key. Deberá crear una carpeta llamada campaña en etc/key.
* [Proporcionar acceso de lectura a la carpeta de campaña a los &quot;datos&quot; del usuario del servicio.](http://localhost:4502/useradmin)

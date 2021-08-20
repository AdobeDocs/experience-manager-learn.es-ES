---
title: Introducción a AEM Forms y Adobe Campaign Standard
description: Integre AEM Forms con Adobe Campaign Standard mediante el Modelo de datos de formulario de AEM Forms para recuperar información de perfil de campaña ACS, etc.
feature: Forms adaptable, Modelo de datos de formulario
version: 6.3,6.4,6.5
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# Introducción a AEM Forms y Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

Este tutorial muestra los distintos casos de uso para la integración de AEM Forms con Adobe Campaign Standard (ACS).

ACS tiene un completo conjunto de API expuestos que permite a ACS ser interconectado con la tecnología de nuestra elección. En este tutorial, nos concentraremos en la interfaz de AEM Forms con ACS.

Para integrar AEM Forms con ACS, deberá seguir los siguientes pasos:

* [Configure el acceso a la API en la instancia ACS.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* Cree un token web JSON.
* Intercambie el token web de JSON con el servicio Identity Management de Adobe para obtener un token de acceso.
* Incluya este token de acceso en el encabezado HTTP de autorización, junto con X-API-Key en cada solicitud a la instancia ACS.

Para empezar, siga las siguientes instrucciones

* [Descargue y descomprima los recursos relacionados con este tutorial.](assets/aem-forms-and-acs-bundles.zip)
* Implementar los paquetes utilizando la consola web [Felix](http://localhost:4502/system/console/bundles)
* Proporcione la configuración adecuada para Adobe Campaign en la configuración Felix OSGI.
* [Cree un usuario de servicio como se menciona en este artículo](/help/forms/adaptive-forms/service-user-tutorial-develop.md). Asegúrese de implementar el paquete OSGi asociado con el artículo.
* Guarde la clave privada ACS en etc/key/campaign/private.key. Debe crear una carpeta denominada campaign en etc/key.
* [Proporcionar acceso de lectura a la carpeta de campañas al usuario de servicios &quot;data&quot;.](http://localhost:4502/useradmin)

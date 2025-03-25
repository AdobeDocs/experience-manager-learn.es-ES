---
title: Integrando con [!DNL ServiceNow]
description: Cree y muestre todos los incidentes con el modelo de datos de formulario.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 3%

---

# Integración de AEM Forms con [!DNL ServiceNow]

Crear y mostrar incidencias en [!DNL ServiceNow] mediante el modelo de datos de formulario en AEM Forms.

## Requisitos previos

* Cuenta de [!DNL ServiceNow].
* Familiarizado con [crear orígenes de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiarizado con [Modelo de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=es)

## Assets de muestra

Los recursos de ejemplo proporcionados con este artículo incluyen lo siguiente

* Configuración de Cloud Service
* Archivos Swagger para crear un incidente y recuperar todo   incidentes
* Modelo de datos de formulario basado en los archivos Swagger
* Formulario adaptable para crear y enumerar [!DNL ServiceNow] incidentes

## Implementación de los recursos en el servidor

* Descargar [recursos de muestra](assets/service-now.zip)
* Importe los recursos en AEM usando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* El archivo swagger utilizado para esta integración se encuentra en la carpeta ```/conf/9957/settings/cloudconfigs/fdm``` del repositorio crx
* Edite la [configuración del servicio en la nube CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)para que coincida con su instancia de ServiceNow.
* Edite la configuración del servicio en la nube [GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) para que coincida con su instancia de ServiceNow. Deberá cambiar el host, el nombre de usuario y la contraseña para que coincidan con las credenciales de la instancia de ServiceNow.

## Credenciales de instancia de Access ServiceNow

* Haga clic en su perfil de usuario
  ![haga clic en el perfil de usuario](assets/snow-1.png)

* Haga clic en Administrar contraseña de instancia
* Los detalles de la instancia se muestran a continuación
  ![detalles de instancia](assets/snow-3.png)

## Prueba de la integración

* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Introduzca algunos valores en el campo descripción y comentarios y haga clic en el botón Crear Incidente
* El ID de incidente del incidente recién creado debe rellenarse en el campo de texto y la tabla siguiente debe enumerar todos los incidentes.

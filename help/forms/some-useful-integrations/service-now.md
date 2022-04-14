---
title: Integración con [!DNL ServiceNow]
description: Cree y muestre todos los incidentes utilizando el modelo de datos de formulario.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
source-git-commit: 81a15fb0182760aaac8cb58cccbfe28de7323492
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---

# Integrar AEM Forms con [!DNL ServiceNow]

Crear y mostrar incidencias en [!DNL ServiceNow] uso del modelo de datos de formulario en AEM Forms.

## Requisitos previos

* [!DNL ServiceNow] cuenta.
* Familiarizado con [creación de fuentes de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiarizado con [Modelo de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Recursos de ejemplo

Los recursos de ejemplo que se proporcionan con este artículo son los siguientes
* Configuración del servicio en la nube
* Intercambiar archivos para crear un incidente y recuperar todos los incidentes
* Modelo de datos de formulario basado en los archivos de cambio
* Formulario adaptable para crear y enumerar [!DNL ServiceNow] incidentes

## Implementar los recursos en el servidor

* Descargue el [recursos de ejemplo](assets/service-now.zip)
* Importar los recursos en AEM mediante [gestor de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Edite el [Configuración del servicio en la nube CreateIncidente](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)para que coincida con su instancia de ServiceNow.
* Edite el [Configuración del servicio en la nube GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) para que coincida con su instancia de ServiceNow

## Probar la integración

* [Abrir el formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Introduzca algunos valores en el campo descripción y comentarios y haga clic en el botón Crear incidente .
* El ID de incidente del incidente recién creado debe rellenarse en el campo de texto y la siguiente tabla debe enumerar todos los incidentes.

---
title: Integración de AEM Forms Cloud Service y Marketo (parte 2)
description: Aprenda a integrar AEM Forms y Marketo mediante el modelo de datos de formulario de AEM Forms.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="Cloud Service de AEM Forms" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
source-git-commit: 835e76695824cc1f155720567ca104a50be4bab8
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 3%

---

# Crear Source de datos

Las API de REST de Marketo están autenticadas con OAuth 2.0 de 2 patas. Podemos crear fácilmente una fuente de datos usando el archivo swagger descargado en el paso anterior

## Crear contenedor de configuración

* AEM Inicie sesión en el servicio de IDs.
* Haz clic en el menú de herramientas y luego en **Explorador de configuración**, como se muestra a continuación

* ![menú de herramientas](assets/datasource3.png)

* Haz clic en **Crear** y proporciona un nombre significativo como se muestra a continuación. Asegúrese de seleccionar la opción Configuraciones de nube como se muestra a continuación

* ![contenedor de configuración](assets/datasource4.png)

## Crear servicios en la nube

* Vaya al menú de herramientas y, a continuación, haga clic en Cloud Services -> Fuentes de datos

* ![servicios en la nube](assets/datasource5.png)

* Seleccione el contenedor de configuración creado en el paso anterior y haga clic en **Crear** para crear una nueva fuente de datos.Proporcione un nombre significativo, seleccione el servicio RESTful en la lista desplegable Tipo de servicio y haga clic en **Siguiente**
* ![nuevo-origen de datos](assets/datasource6.png)

* Cargue el archivo swagger y especifique el tipo de concesión, el ID de cliente, el secreto de cliente y la URL del token de acceso específicos de su instancia de Marketo, como se muestra en la captura de pantalla siguiente.

* Pruebe la conexión y, si la conexión se realiza correctamente, asegúrese de hacer clic en el botón azul **Crear** para finalizar el proceso de creación del origen de datos.

* ![configuración de origen de datos](assets/datasource1.png)


## Siguientes pasos

[Crear modelo de datos de formulario](./part3.md)

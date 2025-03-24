---
title: Configuración de la API de comunicación de AEM Forms
description: Configuración de las API de comunicación de AEM Forms basadas en OpenAPI para la autenticación de servidor a servidor
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 2%

---

# Configuración de las API de comunicación de AEM Forms basadas en OpenAPI en AEM Forms as a Cloud Service

## Requisitos previos

* Última instancia de AEM Forms as a Cloud Service.
* Se han agregado todos los [perfiles de producto necesarios al entorno.](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* Habilite el acceso de la API de AEM al perfil del producto como se muestra a continuación
  ![perfil_producto1](assets/product-profiles1.png)
  ![perfil_producto](assets/product-profiles.png)

## Crear proyecto de Adobe Developer Console

Inicie sesión en [Adobe Developer Console](https://developer.adobe.com/console/) con su Adobe ID.
Para crear un nuevo proyecto, haga clic en el icono correspondiente
![nuevo-proyecto](assets/new-project.png)

Asigne un nombre significativo al proyecto y haga clic en el icono Añadir API
![nuevo-proyecto](assets/new-project2.png)

Seleccionar Experience Cloud
![nuevo-proyecto3](assets/new-project3.png)
Seleccione la API de comunicaciones de AEM Forms y haga clic en Siguiente
![nuevo-proyecto4](assets/new-project4.png)

Asegúrese de haber seleccionado autenticación de servidor a servidor y haga clic en siguiente
![nuevo-proyecto5](assets/new-project5.png)
Seleccione los perfiles y haga clic en el botón Guardar API configurada para guardar la configuración.
![nuevo-proyecto6](assets/new-project6.png)
Haga clic en el servidor a servidor de OAuth.
![nuevo-proyecto7](assets/new-project7.png)
Copie el ID de cliente, el secreto de cliente y los ámbitos
![nuevo proyecto8](assets/new-project8.png)

## Configuración de la instancia de AEM para habilitar la comunicación del proyecto ADC

Si ya tiene un proyecto de AEM Forms, [siga estas instrucciones](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) para habilitar la credencial ClientID de servidor a servidor OAuth del proyecto de Adobe Developer Console para que se comunique con la instancia de AEM

Si no tiene un proyecto de AEM Forms, cree un [proyecto de AEM Forms siguiendo esta documentación.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) y, a continuación, habilite la credencial ClientID de servidor a servidor OAuth del proyecto de Adobe Developer Console para comunicarse con la instancia de AEM [mediante esta documentación.](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## Siguientes pasos

[Generar token de acceso](./generate-access-token.md)

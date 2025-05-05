---
title: Credenciales del servicio de AEM Forms
description: Descargue las credenciales de servicio de Developer Console de AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Credenciales del servicio de AEM Forms

Las integraciones con AEM as a Cloud Service deben poder autenticarse de forma segura en AEM. Developer Console de AEM genera credenciales de servicio, que utilizan aplicaciones, sistemas y servicios externos para interactuar mediante programación con los servicios de AEM Author o Publish a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/344047?quality=12&learn=on&captions=spa)

El archivo de credenciales de servicio descargado se almacena como un archivo de recursos llamado service_token.json en el eclipse proporcionado. Los valores del archivo service_token se utilizan para generar el JWT e intercambiar el JWT por un token de acceso. La clase de utilidad GetServiceCredentials se utiliza para recuperar los valores de propiedad del archivo de recursos service_token.json.

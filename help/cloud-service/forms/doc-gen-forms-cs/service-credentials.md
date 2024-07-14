---
title: Credenciales del servicio de AEM Forms
description: AEM Descargue las credenciales de servicio desde Developer Console de la aplicación de.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Credenciales del servicio de AEM Forms

Las integraciones con AEM as a Cloud Service AEM deben poder autenticarse de forma segura para la autenticación de la. AEM Developer Console AEM genera credenciales de servicio, que utilizan las aplicaciones, los sistemas y los servicios externos para interactuar mediante programación con los servicios de Publish de Autor o de a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

El archivo de credenciales de servicio descargado se almacena como un archivo de recursos llamado service_token.json en el eclipse proporcionado. Los valores del archivo service_token se utilizan para generar el JWT e intercambiar el JWT por un token de acceso. La clase de utilidad GetServiceCredentials se utiliza para recuperar los valores de propiedad del archivo de recursos service_token.json.

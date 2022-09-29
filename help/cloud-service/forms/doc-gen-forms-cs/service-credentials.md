---
title: Credenciales del servicio de AEM Forms
description: Descargue las credenciales del servicio de AEM Developer Console.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# Credenciales del servicio de AEM Forms

Las integraciones con AEM as a Cloud Service deben poder autenticarse de forma segura en AEM. AEM Developer Console genera credenciales de servicio, que utilizan aplicaciones, sistemas y servicios externos para interactuar mediante programación con AEM Author o Publish Services a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

El archivo de credenciales del servicio descargado se almacena como un archivo de recursos llamado service_token.json en el eclipse proporcionado. Los valores del archivo service_token se utilizan para generar el JWT e intercambiar el JWT por un token de acceso. La clase de utilidad GetServiceCredentials se utiliza para recuperar los valores de propiedad del archivo de recursos service_token.json.

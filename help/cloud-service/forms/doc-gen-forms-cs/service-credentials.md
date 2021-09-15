---
title: Credenciales del servicio de AEM
description: Descargue las credenciales del servicio de AEM Developer Console.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 8192
thumbnail: 330519.jpg
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---


# Credenciales de servicio

Las integraciones con AEM como Cloud Service deben poder autenticarse de forma segura en AEM. AEM Developer Console genera credenciales de servicio, que utilizan aplicaciones, sistemas y servicios externos para interactuar mediante programación con AEM Author o Publish Services a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

El archivo de credenciales del servicio descargado se almacena como un archivo de recursos llamado service_token.json en el eclipse proporcionado. Los valores del archivo service_token se utilizan para generar el JWT e intercambiar el JWT por un token de acceso. La clase de utilidad GetServiceCredentials se utiliza para recuperar los valores de propiedad del archivo de recursos service_token.json.

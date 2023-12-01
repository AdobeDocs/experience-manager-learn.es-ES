---
title: AEM Credenciales del servicio de
description: AEM Descargar credenciales de servicio desde la consola de desarrollador de.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Credenciales del servicio

AEM Las integraciones con as a Cloud Service AEM deben poder autenticarse de forma segura para la autenticación de la. AEM AEM La consola de desarrollador de genera credenciales de servicio, que utilizan las aplicaciones, los sistemas y los servicios externos para interactuar mediante programación con los servicios de autor o publicación de la aplicación a través de HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

El archivo de credenciales de servicio descargado se almacena como un archivo de recursos llamado service_token.json en el eclipse proporcionado. Los valores del archivo service_token se utilizan para generar el JWT e intercambiar el JWT por un token de acceso. La clase de utilidad GetServiceCredentials se utiliza para recuperar los valores de propiedad del archivo de recursos service_token.json.

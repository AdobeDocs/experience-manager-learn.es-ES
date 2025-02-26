---
title: Crear una configuración de servicios en la nube
description: Crear un origen de datos para conectarse a Salesforce con las credenciales de OAuth
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: ce22dd482417a54d222165deaf485ff69c2856b7
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 16%

---

# Crear Source de datos

Cree una fuente de datos respaldada por REST utilizando el archivo swagger creado en el paso anterior.

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| Configuración | Valor |
|---------------------|-----------------------------------------------------------------|
| URL de OAuth | https://login.salesforce.com/services/oauth2/authorize |
| Ámbito de autorización | api chat_api id completo openid refresh_token visualforce web |
| URL de token de actualización | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| URL de token de acceso | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**Los nombres de dominio de la URL del token de actualización y acceso tendrán que cambiar para que coincidan con la configuración de la cuenta de Salesforce**
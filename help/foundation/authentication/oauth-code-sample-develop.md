---
title: Desarrollo de ámbitos de OAuth en AEM
description: Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitud en el contexto de AEM.
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# Desarrollo de ámbitos de OAuth

Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitud en el contexto de AEM.

![Flujo De Ámbitos De Oa](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM proporciona tres ámbitos:

* Perfil
* Acceso sin conexión
* Replicar

AEM ámbitos de OAuth ampliables permiten definir otros ámbitos personalizados. Por ejemplo, se puede desarrollar e implementar un ámbito personalizado en AEM que permita que una aplicación móvil autorizada mediante OAuth esté restringida a la lectura, pero no a la escritura de recursos.

OAuth es el método preferido para autorizar una aplicación cliente, ya que utiliza un token de acceso en lugar de exigir que se proporcionen a dicha aplicación las credenciales de un usuario AEM.

* [Ver el código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)

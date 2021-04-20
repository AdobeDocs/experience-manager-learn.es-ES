---
title: Desarrollo de ámbitos de OAuth en AEM
description: Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitudes en el contexto de AEM.
version: 6.3, 6.4, 6.5
feature: Users and Groups
topics: authentication, security
activity: develop
audience: developer
doc-type: code
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 3%

---


# Desarrollo de ámbitos de OAuth

Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitudes en el contexto de AEM.

![Flujo De Ámbitos De Oa](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM ofrece tres ámbitos:

* Perfil
* Acceso sin conexión
* Replicar

Los ámbitos de OAuth ampliables de AEM permiten definir otros ámbitos personalizados. Por ejemplo, se puede desarrollar e implementar un ámbito personalizado en AEM que permita que una aplicación móvil autorizada mediante OAuth esté restringida a la lectura, pero no a la escritura de recursos.

OAuth es el método preferido para autorizar una aplicación cliente, ya que utiliza un token de acceso en lugar de exigir que se proporcionen a dicha aplicación las credenciales de un usuario de AEM.

* [Ver el código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)

---
title: Desarrollo de ámbitos de OAuth en AEM
description: Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso de recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitudes en el contexto de AEM.
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: develop
audience: developer
doc-type: code
translation-type: tm+mt
source-git-commit: b351a57e6e5be0fe5696dc09842fa77fdd036a27
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---


# Desarrollo de ámbitos de OAuth

Los ámbitos OAuth extensibles de Adobe Experience Manager permiten el control de acceso de recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitudes en el contexto de AEM.

![Flujo De Ámbitos De Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM proporciona tres ámbitos:

* Perfil
* Acceso sin conexión
* Replicar

AEM ámbitos OAuth ampliables permiten definir otros ámbitos personalizados. Por ejemplo, se puede desarrollar e implementar un ámbito personalizado en AEM que permita que una aplicación móvil autorizada mediante OAuth esté restringida a la lectura, pero no a la escritura de recursos.

OAuth es el método preferido para autorizar una aplicación cliente, ya que utiliza un token de acceso en lugar de exigir que se proporcionen las credenciales de un usuario AEM a esa aplicación.

* [Vista del código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)

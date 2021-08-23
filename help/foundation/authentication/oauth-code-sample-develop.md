---
title: Desarrollo de ámbitos de OAuth en AEM
description: Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitud en el contexto de AEM.
version: 6.3, 6.4, 6.5
feature: Usuarios y grupos
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '183'
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

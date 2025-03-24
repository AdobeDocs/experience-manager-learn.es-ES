---
title: Desarrollo de ámbitos de OAuth en AEM
description: Los ámbitos ampliables de OAuth de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitud en el contexto de AEM.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 1%

---

# Desarrollo de ámbitos de OAuth

Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. El diagrama siguiente ilustra el flujo de solicitud en el contexto de AEM.

![Flujo De Ámbitos De Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM proporciona tres ámbitos:

* Perfil
* Acceso sin conexión
* Replicar

Los ámbitos OAuth ampliables de AEM permiten definir otros ámbitos personalizados. Por ejemplo, se puede desarrollar e implementar un ámbito personalizado en AEM que permita que una aplicación móvil autorizada mediante OAuth se restrinja a leer, pero no a escribir recursos.

OAuth es el método preferido para autorizar una aplicación cliente, ya que utiliza un token de acceso en lugar de requerir que se proporcionen las credenciales de un usuario de AEM a esa aplicación.

* [Ver el código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)

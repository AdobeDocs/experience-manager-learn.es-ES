---
title: AEM Desarrollo de ámbitos de OAuth en el
description: Los ámbitos ampliables de OAuth de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. AEM El diagrama siguiente ilustra el flujo de solicitud en el contexto de la creación de un flujo de trabajo de la.
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 34
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 1%

---

# Desarrollo de ámbitos de OAuth

Los ámbitos OAuth ampliables de Adobe Experience Manager permiten el control de acceso para los recursos de una aplicación cliente autorizada por un usuario final. AEM El diagrama siguiente ilustra el flujo de solicitud en el contexto de la creación de un flujo de trabajo de la.

![Flujo de ámbitos de OAuth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM El ámbito de la incluye tres ámbitos:

* Perfil
* Acceso sin conexión
* Replicar

AEM Los ámbitos de OAuth ampliables permiten definir otros ámbitos personalizados. AEM Por ejemplo, se puede desarrollar e implementar un ámbito personalizado para que permita que una aplicación móvil autorizada mediante OAuth se restrinja a leer, pero no a escribir recursos.

AEM OAuth es el método preferido para autorizar una aplicación cliente, ya que utiliza un token de acceso en lugar de requerir que se proporcionen las credenciales de un usuario en la aplicación.

* [Ver el código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)

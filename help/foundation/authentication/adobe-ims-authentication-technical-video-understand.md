---
title: Comprensión de la autenticación IMS de Adobe con AEM en Adobe Managed Services
description: Adobe Experience Manager introduce compatibilidad con Admin Console para instancias AEM y autenticación basada en Adobe IMS (Identity Management System) para AEM en Managed Services.   Esta integración permite a AEM clientes de Managed Services administrar todos los usuarios Experience Cloud en una única consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados con instancias de AEM, lo que otorga acceso administrado de forma centralizada a instancias de AEM específicas.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 0%

---

# Comprensión de la autenticación IMS de Adobe con AEM en Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce compatibilidad con Admin Console para instancias AEM y autenticación basada en Adobe IMS (Identity Management System) para AEM en Managed Services.   Esta integración permite a AEM clientes de Managed Services administrar todos los usuarios Experience Cloud en una única consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados con instancias de AEM, lo que concede acceso administrado de forma centralizada a instancias de AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La compatibilidad con la autenticación IMS de Adobe Experience Manager solo es para usuarios &quot;internos&quot; (autores, revisores, administradores, desarrolladores, etc.) y no para usuarios finales externos como, por ejemplo, visitantes de sitios web.
* [Admin Console](https://adminconsole.adobe.com/) representará a AEM clientes de Managed Services como organizaciones IMS y a las instancias de AEM como contextos de producto. Los administradores de sistemas y productos de Admin Console pueden definir y administrar.
* AEM Managed Services sincroniza la topología con el Admin Console, creando una asignación de 1 a 1 entre un contexto de producto y AEM instancia.
* El perfil de producto en el Admin Console determinará a qué instancias de AEM puede acceder un usuario.
* La compatibilidad con la autenticación incluye los IDP compatibles con SAML2 del cliente para SSO.
* Solo se admiten Enterprise ID o Federated ID (para SSO del cliente) (no se admiten los ID de Adobe personal).

*&#42;Esta función es compatible con AEM 6.4 SP3 y posteriores para los clientes de Adobe Managed Services.*

## Prácticas recomendadas {#best-practices}

### Aplicación de permisos en Admin Console

La aplicación de permisos y acceso a nivel de usuario debe evitarse tanto en el Admin Console como en Adobe Experience Manager.

En el Admin Console, se debe conceder acceso a los usuarios a través de Grupos de usuarios en el nivel de Contexto del producto. Los grupos de usuarios suelen expresarse mejor por la función lógica dentro de la organización para promover la reutilización de los grupos en todos los productos de Adobe Experience Cloud.

>[!NOTE]
>
> Si utiliza AEM as a Cloud Service, asigne usuarios de Admin Console directamente a Perfiles de producto. Los permisos transitivos entre usuarios de Admin Console y perfiles de producto a través de grupos de usuarios de Admin Console no son compatibles con AEM as a Cloud Service.

### Aplicación de permisos en Adobe Experience Manager

En Adobe Experience Manager, los grupos de usuarios sincronizados con Adobe IMS deben agregarse como términos a [Grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), que vienen preconfigurados con los permisos adecuados para ejecutar conjuntos específicos de tareas en AEM. Los usuarios sincronizados con Adobe IMS no deben agregarse directamente a [Grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).

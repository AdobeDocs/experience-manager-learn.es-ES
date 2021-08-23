---
title: Comprensión de la autenticación IMS de Adobe con AEM en Adobe Managed Services
description: Adobe Experience Manager introduce compatibilidad con Admin Console para instancias AEM y autenticación basada en IMS de Adobe (Identity Management System) para AEM en Managed Services.   Esta integración permite a AEM clientes de Managed Services administrar todos los usuarios Experience Cloud en una única consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados con instancias de AEM, lo que otorga acceso administrado de forma centralizada a instancias de AEM específicas.
version: 6.4, 6.5
feature: Usuarios y grupos
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Arquitectura
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---


# Comprensión de la autenticación IMS de Adobe con AEM en Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce compatibilidad con Admin Console para instancias AEM y autenticación basada en IMS de Adobe (Identity Management System) para AEM en Managed Services.   Esta integración permite a AEM clientes de Managed Services administrar todos los usuarios Experience Cloud en una única consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados con instancias de AEM, lo que concede acceso administrado de forma centralizada a instancias de AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La compatibilidad con la autenticación IMS de Adobe Experience Manager solo es para usuarios &quot;internos&quot; (autores, revisores, administradores, desarrolladores, etc.) y no para usuarios finales externos como, por ejemplo, visitantes de sitios web.
* [Admin ](https://adminconsole.adobe.com/) Consoleada representará a AEM clientes de Managed Services como organizaciones IMS y a las instancias de AEM como contextos de producto. Los administradores de sistemas y productos de Admin Console pueden definir y administrar.
* AEM Managed Services sincroniza la topología con el Admin Console, creando una asignación de 1 a 1 entre un contexto de producto y AEM instancia.
* El perfil de producto en el Admin Console determinará a qué instancias de AEM puede acceder un usuario.
* La compatibilidad con la autenticación incluye los IDP compatibles con SAML2 del cliente para SSO.
* Solo se admitirán los Enterprise ID o Federated ID (para SSO del cliente) (no se admiten los ID de Adobe personal).

** Esta función es compatible con AEM 6.4 SP3 y posteriores para los clientes de Adobe Managed Services.*

## Prácticas recomendadas {#best-practices}

### Aplicación de permisos en Admin Console

La aplicación de permisos y acceso a nivel de usuario debe evitarse tanto en el Admin Console como en Adobe Experience Manager.

En el Admin Console, se debe conceder acceso a los usuarios a través de Grupos de usuarios en el nivel de Contexto del producto. Los grupos de usuarios suelen expresarse mejor por la función lógica dentro de la organización para promover la reutilización de los grupos en todos los productos de Adobe Experience Cloud.

>[!NOTE]
>
> Si utiliza AEM como Cloud Service, asigne usuarios de Admin Console directamente a Perfiles de producto. Como Cloud Service, no se admiten permisos transitivos entre usuarios Admin Console y Perfiles de producto a través de grupos de usuarios Admin Console.

### Aplicación de permisos en Adobe Experience Manager

En Adobe Experience Manager, los grupos de usuarios sincronizados con IMS de Adobe deben agregarse a los [grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), que vienen preconfigurados con los permisos adecuados para ejecutar conjuntos específicos de tareas en AEM. Los usuarios sincronizados con IMS de Adobe no deben agregarse directamente a [grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).

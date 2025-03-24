---
title: Explicación de la autenticación IMS de Adobe con AEM en Adobe Managed Services
description: Adobe Experience Manager presenta la compatibilidad de Admin Console con las instancias de AEM y la autenticación basada en Adobe IMS (Identity Management System) para AEM en Managed Services.   Esta integración permite a los clientes de AEM Managed Services administrar todos los usuarios de Experience Cloud en una única consola web unificada. Los usuarios y grupos pueden asignarse a perfiles de producto asociados a instancias de AEM, lo que otorga acceso administrado de forma centralizada a instancias de AEM específicas.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---

# Explicación de la autenticación IMS de Adobe con AEM en Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager presenta la compatibilidad de Admin Console con las instancias de AEM y la autenticación basada en Adobe Identity Management System (IMS) para AEM en Managed Services.   Esta integración permite a los clientes de AEM Managed Services administrar todos los usuarios de Experience Cloud en una única consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados a instancias de AEM, lo que otorga acceso administrado de forma centralizada a instancias de AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La compatibilidad con la autenticación IMS de Adobe Experience Manager solo es para usuarios &quot;internos&quot; (autores, revisores, administradores, desarrolladores, etc.), y no para usuarios finales externos como visitantes de sitios web.
* [Admin Console](https://adminconsole.adobe.com/) representa a los clientes de AEM Managed Services como organizaciones de IMS y las instancias de AEM como contextos de producto. Los administradores de sistemas y productos de Admin Console pueden definir y administrar.
* AEM Managed Services sincroniza su topología con Admin Console y crea una asignación individual entre una instancia de contexto de producto y una instancia de AEM.
* El perfil de producto en Admin Console determina qué instancias de AEM puede acceder un usuario.
* El soporte de autenticación incluye IDPs compatibles con SAML2 para SSO.
* Solo se admiten Enterprise ID o Federated ID (para el SSO del cliente) (no se admiten los Adobe ID personales).

*&#42;Esta característica es compatible con AEM 6.4 SP3 y versiones posteriores para clientes de Adobe Managed Services.*

## Prácticas recomendadas {#best-practices}

### Aplicación de permisos en Admin Console

Se debe evitar aplicar permisos y acceso en el nivel de usuario tanto en Admin Console como en Adobe Experience Manager.

En Admin Console, los usuarios deben tener acceso a través de grupos de usuarios en el nivel de contexto de producto. Los grupos de usuarios suelen expresarse mejor por la función lógica dentro de la organización para promover la reutilización de los grupos en los productos de Adobe Experience Cloud.

### Aplicación de permisos en Adobe Experience Manager

En Adobe Experience Manager, los grupos de usuarios sincronizados desde Adobe IMS deben agregarse a término a [grupos de usuarios proporcionados por AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=es), que vienen preconfigurados con los permisos adecuados para ejecutar conjuntos específicos de tareas en AEM. Los usuarios sincronizados desde Adobe IMS no deben agregarse directamente a [grupos de usuarios proporcionados por AEM](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=es).

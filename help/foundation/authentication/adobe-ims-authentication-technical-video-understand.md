---
title: Explicación de la autenticación IMS de Adobe con AEM en Adobe Managed Services
description: Adobe Experience Manager introduce la compatibilidad con Admin Console para instancias de AEM y la autenticación basada en Adobe IMS (Identity Management System) para AEM en Managed Services.   Esta integración permite a los clientes de AEM Managed Services administrar todos los usuarios de Experience Cloud en una única consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados con instancias de AEM, lo que concede acceso administrado de forma centralizada a instancias de AEM específicas.
version: 6.4, 6.5
feature: autenticación
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---


# Comprensión de la autenticación IMS de Adobe con AEM en Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce la compatibilidad con Admin Console para instancias de AEM y la autenticación basada en Adobe IMS (Identity Management System) para AEM en Managed Services.   Esta integración permite a los clientes de AEM Managed Services administrar todos los usuarios de Experience Cloud en una única consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados con instancias de AEM, lo que concede acceso administrado de forma centralizada a instancias de AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La compatibilidad con la autenticación IMS de Adobe Experience Manager solo es para usuarios &quot;internos&quot; (autores, revisores, administradores, desarrolladores, etc.) y no para usuarios finales externos como, por ejemplo, visitantes de sitios web.
* [Admin ](https://adminconsole.adobe.com/) Console representará a los clientes de AEM Managed Services como organizaciones IMS y a las instancias de AEM como contextos de producto. Los administradores de sistemas y productos de Admin Console pueden definir y administrar.
* Los servicios administrados de AEM sincronizan la topología con Admin Console, creando una asignación de 1 a 1 entre un contexto de producto y una instancia de AEM.
* El perfil de producto en Admin Console determinará a qué instancias de AEM puede acceder un usuario.
* La compatibilidad con la autenticación incluye los IDP compatibles con SAML2 del cliente para SSO.
* Solo se admitirán los Enterprise ID o Federated ID (para SSO del cliente) (no se admiten los Adobe ID personales).

** Esta función es compatible con AEM 6.4 SP3 y posteriores para los clientes de Adobe Managed Services.*

## Prácticas recomendadas {#best-practices}

### Aplicación de permisos en Admin Console

La aplicación de permisos y acceso a nivel de usuario debe evitarse tanto en Admin Console como en Adobe Experience Manager.

En Admin Console, se debe conceder acceso a los usuarios a través de Grupos de usuarios en el nivel de Contexto del producto. Los grupos de usuarios suelen expresarse mejor con el rol lógico dentro de la organización para promover la reutilización de los grupos en los productos de Adobe Experience Cloud.

>[!NOTE]
>
> Si utiliza AEM as a Cloud Service, asigne usuarios de Admin Console directamente a Perfiles de producto. Los permisos transitivos entre los usuarios de Admin Console y los perfiles de producto a través de los grupos de usuarios de Admin Console no son compatibles con AEM as a Cloud Service.

### Aplicación de permisos en Adobe Experience Manager

En Adobe Experience Manager, los grupos de usuarios sincronizados con Adobe IMS deben agregarse como término a los [grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), que vienen preconfigurados con los permisos adecuados para ejecutar conjuntos específicos de tareas en AEM. Los usuarios sincronizados con Adobe IMS no deben agregarse directamente a [grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
